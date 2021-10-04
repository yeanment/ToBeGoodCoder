# Distributed Locks

## Introduction
Distributed locks are a very useful primitive in many environments where different processes must operate with shared resources in a mutually exclusive way.

We are going to model our design with just three properties that, from our point of view, are the minimum guarantees needed to use distributed locks in an effective way.
- Safety property: Mutual exclusion. At any given moment, only one client can hold a lock.
- Liveness property A: Deadlock free. Eventually it is always possible to acquire a lock, even if the client that locked a resource crashes or gets partitioned.
- Liveness property B: Fault tolerance. As long as the majority of Redis nodes are up, clients are able to acquire and release locks.

## Redis Solution
The simplest way to use Redis to lock a resource is to create a key with limited living time in an instance, using the Redis expires feature, so that eventually it will get released (property 2 in our list). When the client needs to release the resource, it deletes the key. However, if there is a single point of failure in our architecture, e.g. the Redis master, simply adding a replica is possiblely not viable. By doing so we can’t implement our safety property of mutual exclusion, because Redis replication is asynchronous. Sometimes it is perfectly fine that under special circumstances, like during a failure, multiple clients can hold the lock at the same time. If this is the case, you can use your replication based solution. Otherwise we suggest to implement the solution described in this document.

There is an obvious race condition with this model:
1. Client A acquires the lock in the master.
2. The master crashes before the write to the key is transmitted to the replica.
3. The replica gets promoted to master.
4. Client B acquires the lock to the same resource A already holds a lock for. **SAFETY VIOLATION!**

### Implementation with a Single Instance
To acquire the lock, the way to go is the following: `SET resource_name my_random_value NX PX 30000`. The command will set the key only if it does not already exist (NX option), with an expire of 30000 milliseconds (PX option). The key is set to a value “my_random_value”. This value must be unique across all clients and all lock requests to avoid removing a lock that was created by another client. Basically the random value is used in order to release the lock in a safe way, with a script that tells Redis: remove the key only if it exists and the value stored at the key is exactly the one I expect to be. This is accomplished by the following Lua script:
```Lua
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```
For example a client may acquire the lock, get blocked in some operation for longer than the lock validity time (the time at which the key will expire), and later remove the lock, that was already acquired by some other client. Using just DEL is not safe as a client may remove the lock of another client. With the above script instead every lock is “signed” with a random string, so the lock will be removed only if it is still the one that was set by the client trying to remove it.

What should this random string be? I assume it’s 20 bytes from`/dev/urandom`, but you can find cheaper ways to make it unique enough for your tasks. A simpler solution is to use a combination of unix time with microseconds resolution, concatenating it with a client ID, it is not as safe, but probably up to the task in most environments.

The time we use as the key time to live, is called the “lock validity time”. It is both the auto release time, and the time the client has in order to perform the operation required before another client may be able to acquire the lock again, without technically violating the mutual exclusion guarantee, which is only limited to a given window of time from the moment the lock is acquired.

### Redlock Algorithm
**Assumptions**
- N Redis masters
- All nodes are totally independent. So we don’t use replication or any other implicit coordination system.
- While there is no synchronized clock across the processes, still the local time in every process flows approximately at the same rate, with an error which is small compared to the auto-release time of the lock. This assumption closely resembles a real-world computer: every computer has a local clock and we can usually rely on different computers to have a clock drift which is small.
- The algorithm discussed above is used to acquire and release the lock in a single instance. 

In our examples we set N=5, which is a reasonable value. In order to acquire the lock, the client performs the following operations:
1. It gets the current time in milliseconds.
2. It tries to acquire the lock in all the N instances sequentially, using the same key name and random value in all the instances. During step 2, when setting the lock in each instance, the client uses a timeout which is small compared to the total lock auto-release time in order to acquire it. For example if the auto-release time is 10 seconds, the timeout could be in the ~ 5-50 milliseconds range. This prevents the client from remaining blocked for a long time trying to talk with a Redis node which is down: if an instance is not available, we should try to talk with the next instance ASAP.
3. The client computes how much time elapsed in order to acquire the lock, by subtracting from the current time the timestamp obtained in step 1. If and only if the client was able to acquire the lock in the majority of the instances (at least 3), and the total time elapsed to acquire the lock is less than lock validity time, the lock is considered to be acquired.
4. If the lock was acquired, its validity time is considered to be the initial validity time minus the time elapsed, as computed in step 3. The client holding the lock will terminate its work within the lock validity time (as obtained in step 3), minus some time (just a few milliseconds in order to compensate for clock drift between processes).
5. If the client failed to acquire the lock for some reason (either it was not able to lock N/2+1 instances or the validity time is negative), it will try to unlock all the instances (even the instances it believed it was not able to lock). So there is no need to wait for key expiry in order for the lock to be acquired again (however if a network partition happens and the client is no longer able to communicate with the Redis instances, there is an availability penalty to pay as it waits for key expiration).

**Retry on failure**
When a client is unable to acquire the lock, it should try again after a random delay in order to try to desynchronize multiple clients trying to acquire the lock for the same resource at the same time (this may result in a split brain condition where nobody wins). Also the faster a client tries to acquire the lock in the majority of Redis instances, the smaller the window for a split brain condition (and the need for a retry), so ideally the client should try to send the SET commands to the N instances at the same time using multiplexing.

**Releasing the lock**
Releasing the lock is simple and involves just releasing the lock in all instances, whether or not the client believes it was able to successfully lock a given instance.

**Safety arguments**
To start let’s assume that a client is able to acquire the lock in the majority of instances. All the instances will contain a key with the same time to live. However, the key was set at different times, so the keys will also expire at different times. But if the first key was set at worst at time T1 (the time we sample before contacting the first server) and the last key was set at worst at time T2 (the time we obtained the reply from the last server), we are sure that the first key to expire in the set will exist for at least `MIN_VALIDITY=TTL-(T2-T1)-CLOCK_DRIFT`. All the other keys will expire later, so we are sure that the keys will be simultaneously set for at least this time. During the time that the majority of keys are set, another client will not be able to acquire the lock, since `N/2+1 SET NX` operations can’t succeed if `N/2+1` keys already exist. So if a lock was acquired, it is not possible to re-acquire it at the same time (violating the mutual exclusion property).

If a client locked the majority of instances using a time near, or greater, than the lock maximum validity time (the TTL we use for SET basically), it will consider the lock invalid and will unlock the instances, so we only need to consider the case where a client was able to lock the majority of instances in a time which is less than the validity time. In this case for the argument already expressed above, for `MIN_VALIDITY` no client should be able to re-acquire the lock. So multiple clients will be able to lock `N/2+1` instances at the same time (with "time" being the end of Step 2) only when the time to lock the majority was greater than the TTL time, making the lock invalid.

**Liveness arguments**
The system liveness is based on three main features:
1. The auto release of the lock (since keys expire): eventually keys are available again to be locked.
2. The fact that clients, usually, will cooperate removing the locks when the lock was not acquired, or when the lock was acquired and the work terminated, making it likely that we don’t have to wait for keys to expire to re-acquire the lock.
3. The fact that when a client needs to retry a lock, it waits a time which is comparably greater than the time needed to acquire the majority of locks, in order to probabilistically make split brain conditions during resource contention unlikely.

However, we pay an availability penalty equal to TTL time on network partitions, so if there are continuous partitions, we can pay this penalty indefinitely. This happens every time a client acquires a lock and gets partitioned away before being able to remove the lock. Basically if there are infinite continuous network partitions, the system may become not available for an infinite amount of time.

**Performance, crash-recovery and fsync**
Many users using Redis as a lock server need high performance in terms of both latency to acquire and release a lock, and number of acquire / release operations that it is possible to perform per second. In order to meet this requirement, the strategy to talk with the N Redis servers to reduce latency is definitely multiplexing (or poor man's multiplexing, which is, putting the socket in non-blocking mode, send all the commands, and read all the commands later, assuming that the RTT between the client and each instance is similar).

However there is another consideration to do about persistence if we want to target a crash-recovery system model. Basically to see the problem here, let’s assume we configure Redis without persistence at all. A client acquires the lock in 3 of 5 instances. One of the instances where the client was able to acquire the lock is restarted, at this point there are again 3 instances that we can lock for the same resource, and another client can lock it again, violating the safety property of exclusivity of lock.

If we enable AOF persistence, things will improve quite a bit. For example we can upgrade a server by sending SHUTDOWN and restarting it. Because Redis expires are semantically implemented so that virtually the time still elapses when the server is off, all our requirements are fine. However everything is fine as long as it is a clean shutdown. What about a power outage? If Redis is configured, as by default, to fsync on disk every second, it is possible that after a restart our key is missing. In theory, if we want to guarantee the lock safety in the face of any kind of instance restart, we need to enable fsync=always in the persistence setting. This in turn will totally ruin performances to the same level of CP systems that are traditionally used to implement distributed locks in a safe way.

However things are better than what they look like at a first glance. Basically the algorithm safety is retained as long as when an instance restarts after a crash, it no longer participates to any currently active lock, so that the set of currently active locks when the instance restarts, were all obtained by locking instances other than the one which is rejoining the system. To guarantee this we just need to make an instance, after a crash, unavailable for at least a bit more than the max TTL we use, which is, the time needed for all the keys about the locks that existed when the instance crashed, to become invalid and be automatically released. Using delayed restarts it is basically possible to achieve safety even without any kind of Redis persistence available, however note that this may translate into an availability penalty. For example if a majority of instances crash, the system will become globally unavailable for TTL (here globally means that no resource at all will be lockable during this time).

**Lock Extension Mechanism**
The client, if in the middle of the computation while the lock validity is approaching a low value, may extend the lock by sending a Lua script to all the instances that extends the TTL of the key if the key exists and its value is still the random value the client assigned when the lock was acquired. The client should only consider the lock re-acquired if it was able to extend the lock into the majority of instances, and within the validity time (basically the algorithm to use is very similar to the one used when acquiring the lock).


## Zookeeper Solution

ZooKeeper is a distributed, open-source coordination service for distributed applications. It uses a data model styled after the familiar directory tree structure of file systems, and allows distributed processes to coordinate with each other through a shared hierarchical namespace which is organized similarly to a standard file system. The namespace consists of data registers - called znodes, in ZooKeeper parlance - and these are similar to files and directories. Unlike a typical file system, which is designed for storage, ZooKeeper data is kept in-memory, which means ZooKeeper can achieve high throughput and low latency numbers. The ZooKeeper implementation puts a premium on high performance, highly available, strictly ordered access. The performance aspects of ZooKeeper means it can be used in large, distributed systems. The reliability aspects keep it from being a single point of failure. The strict ordering means that sophisticated synchronization primitives can be implemented at the client.

### Design Goals
**ZooKeeper is replicated.** ZooKeeper itself is intended to be replicated over a set of hosts called an ensemble. The servers that make up the ZooKeeper service must all know about each other. They maintain an in-memory image of state, along with a transaction logs and snapshots in a persistent store. As long as a majority of the servers are available, the ZooKeeper service will be available. Clients connect to a single ZooKeeper server. The client maintains a TCP connection through which it sends requests, gets responses, gets watch events, and sends heart beats. If the TCP connection to the server breaks, the client will connect to a different server.

**ZooKeeper is ordered.** ZooKeeper stamps each update with a number that reflects the order of all ZooKeeper transactions. Subsequent operations can use the order to implement higher-level abstractions, such as synchronization primitives.

**ZooKeeper is fast.** It is especially fast in "read-dominant" workloads. ZooKeeper applications run on thousands of machines, and it performs best where reads are more common than writes, at ratios of around 10:1.

### Data Model and the Hierarchical Namespace
The namespace provided by ZooKeeper is much like that of a standard file system. A name is a sequence of path elements separated by a slash (/). Every node in ZooKeeper's namespace is identified by a path. Each node in a ZooKeeper namespace can have data associated with it as well as children. It is like having a file-system that allows a file to also be a directory. (ZooKeeper was designed to store coordination data: status information, configuration, location information, etc., so the data stored at each node is usually small, in the byte to kilobyte range.) We use the term znode to make it clear that we are talking about ZooKeeper data nodes.

Znodes maintain a stat structure that includes version numbers for data changes, ACL changes, and timestamps, to allow cache validations and coordinated updates. Each time a znode's data changes, the version number increases. For instance, whenever a client retrieves data it also receives the version of the data. The data stored at each znode in a namespace is read and written atomically. Reads get all the data bytes associated with a znode and a write replaces all the data. Each node has an Access Control List (ACL) that restricts who can do what.

ZooKeeper also has the notion of ephemeral nodes. These znodes exists as long as the session that created the znode is active. When the session ends the znode is deleted.

ZooKeeper supports the concept of watches. Clients can set a watch on a znode. A watch will be triggered and removed when the znode changes. When a watch is triggered, the client receives a packet saying that the znode has changed. If the connection between the client and one of the ZooKeeper servers is broken, the client will receive a local notification. Clients can also set permanent, recursive watches on a znode that are not removed when triggered and that trigger for changes on the registered znode as well as any children znodes recursively.

### Guarantees
ZooKeeper is very fast and very simple. Since its goal, though, is to be a basis for the construction of more complicated services, such as synchronization, it provides a set of guarantees. These are:
- Sequential Consistency - Updates from a client will be applied in the order that they were sent.
- Atomicity - Updates either succeed or fail. No partial results.
- Single System Image - A client will see the same view of the service regardless of the server that it connects to. i.e., a client will never see an older view of the system even if the client fails over to a different server with the same session.
- Reliability - Once an update has been applied, it will persist from that time forward until a client overwrites the update.
- Timeliness - The clients view of the system is guaranteed to be up-to-date within a certain time bound.

### Implementation
The replicated database is an in-memory database containing the entire data tree. Updates are logged to disk for recoverability, and writes are serialized to disk before they are applied to the in-memory database. 

Every ZooKeeper server services clients. Clients connect to exactly one server to submit requests. Read requests are serviced from the local replica of each server database. Requests that change the state of the service, write requests, are processed by an agreement protocol.

As part of the agreement protocol all write requests from clients are forwarded to a single server, called the leader. The rest of the ZooKeeper servers, called followers, receive message proposals from the leader and agree upon message delivery. The messaging layer takes care of replacing leaders on failures and syncing followers with leaders.

ZooKeeper uses a custom atomic messaging protocol. Since the messaging layer is atomic, ZooKeeper can guarantee that the local replicas never diverge. When the leader receives a write request, it calculates what the state of the system is when the write is to be applied and transforms this into a transaction that captures this new state.

### Reliability
First, if followers fail and recover quickly, then ZooKeeper is able to sustain a high throughput despite the failure. But maybe more importantly, the leader election algorithm allows for the system to recover fast enough to prevent throughput from dropping substantially. In our observations, ZooKeeper takes less than 200ms to elect a new leader. Third, as followers recover, ZooKeeper is able to raise throughput again once they start processing requests.

### Distributed Lock Based on Zookeeper
- Create a lock directory /lock;
- When a client needs to obtain a lock, create temporary and orderly child nodes under /lock;
- The client obtains the child node list under /lock and determines whether the child node created by itself is the child node with the lowest sequence number in the current child node list. If so, it is considered to have obtained the lock; Otherwise, listen to your previous child node, and repeat this step after obtaining the change notification of the child node until you obtain the lock;
- Execute the transaction, and then delete the corresponding child node.

If a session that has obtained the lock times out, because it is creating a temporary node, the temporary node corresponding to the session will be deleted, and other sessions can obtain the lock. It can be seen that this implementation will not cause the failure of releasing the lock of the unique index implementation of the database.

A node does not obtain a lock and only needs to listen to its previous child node. This is because if it listens to all child nodes, the state of any child node will change and all other child nodes will receive a notification (herd effect. When one sheep moves, other sheep will coax up), and we only want its next child node to receive a notification.

# Distributed Transaction
The transaction occurs in different nodes and is required to keep aAtomicity, consistency, isolation and durabilily (ACID). The key point lies on the keeping ACID for all operations within a single transactions. 

## CAP Theorem
The distributed system cannot meet the requirements of consistency, availability and partition tolerance at the same time. At most, it can only meet two of them at the same time. In distributed systems, partition tolerance is essential because the network needs to always be assumed to be unreliable. Therefore, CAP theorem is actually a trade-off between availability and consistency. Availability and consistency are often conflicting, and it is difficult to meet them at the same time. When synchronizing data between multiple nodes,
- In order to ensure consistency (CP), nodes that are not synchronized can not be accessed, and some availability will be lost;
- In order to ensure availability (AP), it is allowed to read the data of all nodes, but the data may be inconsistent.

### Consistency
Consistency refers to whether multiple data copies can maintain consistency. Under the condition of consistency, the system can transfer from the consistency state to another consistency state after performing data update operations. After a successful data update, if all users can read the latest value, the system is considered to achieve strong consistency.

### Availability
Availability refers to the ability of providing normal services with various exceptions. It can be measured by the ratio of the system available time to the total time. Under the condition of availability, the services provided by the system are required to be available all the time, and the results can always be returned in a limited time for each operation request of the user.

### Partition Tolerance
Under the condition of partition tolerance, the distributed system still needs services that can provide consistency and availability when encountering any network partition failure, unless the whole network environment fails.


### Basically Available, Soft State, and Eventually Consistent (BASE)
BASE theory is the trade-off between consistency and availability in CAP. Its core idea is that even if strong consistency cannot be achieved, each application can adopt appropriate methods to achieve the final consistency of the system according to its own business characteristics. ACID requires strong consistency and is usually used in traditional database systems. BASE requires final consistency and achieves availability by sacrificing strong consistency. It is usually used in large-scale distributed systems.

**Basically Available** means that when the distributed system fails, the core availability is guaranteed and partial availability is allowed to be lost.

**Soft State** means that the data in the system is allowed to have an intermediate state, and it is considered that the intermediate state will not affect the overall availability of the system, that is, there is a delay in the process of allowing the synchronization between the data copies of different nodes of the system.

**Final Consistency** emphasizes that all data copies in the system can finally reach a consistent state after a period of synchronization.

## Two/Three-phase Commit
In a distributed system, although each node can know the success or failure of its own operation, it cannot know the success or failure of the operation of other nodes. When a transaction spans multiple nodes, in order to maintain the acid characteristics of the transaction, it is necessary to introduce a coordinator to uniformly control the operation results of all nodes (called participants) and finally indicate whether these nodes want to commit the operation results (such as writing the updated data to disk, etc.). The two-phase commit algorithm is as follows: 
1. The coordinator will ask all participant nodes whether they can perform the submission operation.
    - Each participant starts preparations for transaction execution, such as locking resources, reserving resources, writing undo / redo log
    - The participant responds to the coordinator. If the preparation of the transaction is successful, the participant responds "can submit". Otherwise, the participant responds "refuse to submit"
2. If all participants respond "can submit", the coordinator sends a "formal submit" command to all participants. 
    - Participants complete the formal submission and release all resources, and then respond to "completion". The coordinator collects the "completion" responses of each node and ends the global transaction.
    - If a participant responds to "reject submission", the coordinator sends a "rollback operation" to all participants, releases all resources, and then responds to "rollback complete". After collecting the "rollback" responses of each node, the coordinator cancels the global transaction.

The two-phaese commit algorithm is viewed as making vote in the first stage and making decisions in the second stage.It achieves strong consistency. In some system designs, a series of operations are required to complete a transaction. Each step will allocate some resources or rewrite some data. If we can't do a certain step, we need to do the reverse operation to recycle all the resources allocated in front. Therefore, the operation is more complex.

There are some problems. One of them is synchronous blocking operation, which will greatly affect the performance. Another major problem is on timeout, for example,
- If the participant does not receive the inquiry request in the first stage, or the participant's response does not reach the coordinator. Then, the coordinator needs to handle the timeout. Once the timeout occurs, it can be regarded as a failure or retried.
- In the second stage, after the formal submission is sent, if some participants do not receive it, or the confirmation information after submission/rollback is not returned, once the participant's response times out, either try again, or mark that participant as a problem node and eliminate the whole cluster, so as to ensure the data consistency of the service nodes.
- In the second stage, if the participant fails to receive the commit / fallback instruction from the coordinator, the participant will be in the "state unknown" stage, and the participant does not know what to do. For example, if all participants complete the reply in the first stage (maybe all yes, maybe all no, maybe some yes and some no), If the coordinator dies at this time. Then all the nodes don't know what to do (ask other participants). For consistency, either wait for the coordinator or reissue the yes / no command of the first stage. If the participants do not receive the decision in the second stage after the first stage is completed, the data node will enter the "overwhelmed" state, which will block the whole transaction. In other words, the coordinator is very important for the completion of the transaction, and the availability of the coordinator is the key. 

For these reasons, we introduce three-phase commit. It breaks the first phase into two stages: query, and then lock resources. Finally, the real submission. It do not lock resources when querying, and do not start locking resources unless everyone agrees. Theoretically, if all nodes in the first stage return success, there is reason to believe that the probability of successful submission is very high. In this way, the probability that the state of participant is unknown can be reduced. In other words, once the participant receives precommit, it means that he knows that everyone actually agrees to modify it. This is very important. 

## Paxos
Paxos algorithm is a consensus algorithm based on message passing proposed by Leslie Lamport in 1990. It solves the problem of reaching an agreement in a distributed system, so as to ensure that the consistency of the resolution will not be destroyed no matter any of the above exceptions occur. A typical scenario is that in a distributed database system, if the initial states of each node are consistent and each node performs the same sequence of operations, they can finally get a consistent state. In order to ensure that each node executes the same command sequence, a "consistency algorithm" needs to be executed on each instruction to ensure that the instructions seen by each node are consistent. 

In short, Paxos aims to make the nodes of the whole cluster agree on the change of a value. Paxos algorithm is basically a democratic election algorithm - most decisions will be unified decisions of the whole cluster. Any point can propose a proposal to modify some data. Whether the proposal is passed depends on whether more than half of the nodes in the cluster agree (so Paxos algorithm requires that the number of nodes in the cluster is singular). The algorithm has two stages (assuming that the algorithm has three nodes: A, B and C):
- Phase I: Prepare phase
    - A sends the prepare request to all nodes a, B and C. Note that the Paxos algorithm will have a sequence number (which is increasing and unique, that is, A and B cannot have the same sequence number). This sequence number will be sent together with the modification request. Any node will reject the request whose value is less than the current sequence number in the "prepare stage". Therefore, when applying for modification requests from all nodes, node A needs to bring a sequence number. The newer the proposal, the greater the sequence number.
    - If the sequence number received by the receiving node is greater than the sequence number sent by other nodes, this node will respond Yes (the latest approved proposal number on this node) and promise not to receive other proposals with sequence number below n. In this way, the node will always make a commitment to the latest proposal in the Prepare phase.
    - **Optimization:** in the above preparation process, if any node finds a proposal with a higher number, it needs to notify the proposer to remind him to interrupt the proposal.
- Phase II: Accept phase
    - If proposer A receives yes from more than half of the nodes, then he will issue an accept request to all nodes (similarly, the sequence number n needs to be brought). If there is no more than half, it will return failure.
    - After receiving the accept request, if n is the largest for the receiving node, it will modify this value. If it finds that it has a larger sequence number, the node will refuse to modify it.

## Reference

- [Distributed locks with Redis](https://redis.io/topics/distlock)
- [Distributed locks with Zookeeper](https://zookeeper.apache.org/doc/current/zookeeperOver.html)
- [NEAT ALGORITHMS - PAXOS](http://harry.me/blog/2014/12/27/neat-algorithms-paxos/)
