# Cluster

## Load Balance

The nodes in cluster are often designed  to be stateless, and users can request any node. The load balance algorithm will forward user requests to the appropriate nodes according to the load of each node in the cluster. It is designed to achieve high availability and scalability:
- **High availability:** when a node fails, the load balancer will forward the user request to another node, so as to ensure the continuous availability of all services;
- **Scalability:** nodes can be easily added or removed according to the overall load of the system.

### Load Balance Algorithms

#### 1. Round Robin
The algorithm sends each request to each node in turn. This algorithm is more suitable for scenarios where the performance of each node is similar. If there are differences in performance, the node with poor performance may not be able to bear excessive load. 

The Weighted Round Robin algorithms gives a certain weight to the node according to the performance difference of the node on the basis of polling, and the node with high performance assigns a higher weight.

#### 2. Least Connections
Because of the difference in connection time of each request, the polling or weighted polling algorithm may make the current connections of one node too large and the connections of the other node too small, resulting in load imbalance. The least connection algorithm is to send the request to the node with the least number of connections. 

The Weighted Least Connections algorithms assigns a a weight according to the performance of the node, and then the number of connections that each node, on the basis of minimum connections.

#### 3. Random
Similarly with the pooling algorithms, except that it sends the request to the node randomly. 

#### 4. IP Hash
Calculates the hash value of the client IP, and then takes the modulus of the number of nodes to obtain the serial number of the target node. It can ensure that the requests of clients with the same IP will be forwarded to the same node to realize session sticky session

### Redirection
Redirection can be considered as a set of techniques that help to find bes” distributed content. Most redirection deployments include some form of load balancing. Conversely, any form of load balancing involved redirection techniques. The goal of redirection is to send HTTP messages to available web nodes as quickly as possible. 

#### 1. HTTP Redirection
Basis for rerouting (many options). It can be slow since every transaction involves the extra redirect step. Also, the first node must be able to handle the request load

**Procedures**
- Alice sends HTTP request to node n0
- Node n0 returns 302 redirect to Alice
- browser resends HTTP request, this time to node n1
- Node n1 sends HTTP request to Alice
- Node n0 returns 302 redirect to Alice

**Cons**
- A significant amount of processing power is required from the original node. 
- User delays are increased, because two round trips are required to access pages. If the redirecting node is broken, the site will be broken.

#### 2. DNS Redirection
The DNS sercver decides whether to resolve to. DNS address rotation spreads the load around, because each DNS lookup to a node gets a different ordering of node addresses. However, this load balancing isn’t perfect, because the results of the DNS lookup may be memorized and reused by applications, operating systems, and some primitive child DNS nodes. Many web browsers perform a DNS lookup for a host but then use the same address over and over again, to eliminate the cost of DNS lookups and because some nodes prefer to keep talking to the same client. Furthermore, many OSs perform the DNS lookup automatically, and cache the result, but don’t rotate the addresses. Consequently, DNS round robin generally doesn’t balance the load of a single client – one client typically will be stuck to one node for a long time. However, it can spread the aggregate load of multiple clients. As long as there is a modestly large number of clients with similar demand, the load will be relatively well distributed across nodes.

**Procedures**
- Alice asks DNS for IP address of 
- DNS replies Alice with node n0
- Alice sends HTTP request to node n0
- Node n0 asks DNS for IP address of Alice 
- DNS replies with node n1
- Node n0 sends HTTP request to node n1

**Cons**
- Typically, the DNS server that runs sophisticated server-tracking algorithm is an authoritative server that is under the control of the content provider. Several distributed hosting services use this DNS redirection model. The authoritative DNS serer uses to make its decision is the IP address of the local DNS server, Not the IP address of the client.
- Because DNS has a multi-level structure, domain name records at each level may be cached. When a server offline needs to modify DNS records, it takes a long time to take effect. Large websites basically use DNS as the first level load balancing method, and then use other methods internally to do the second level load balancing. That is, the result of domain name resolution is the IP address of the internal load balancing server.


#### 3. Anycast Addressing Backbone Network

The backbone node is located in front of the source node. Users' requests need to pass through the backbone node before reaching the source node. Backbone node can be used for caching, logging, etc. at the same time, it can also be used as a load balancing node. It can be easily integrated with other functions, and the deployment is simple.

**Cons**
- All requests and responses need to go through the backbone node, which may become a performance bottleneck.

## Session Management
If a user's session information is stored on a node, when the load balancer forwards the user's next request to another node, because the node does not have the user's session information, the user needs to log in again. 

### Sticky Session
In the Sticky Sessions approach, we configure our load balancer to route all the requests from the same client to the same node. But the problem with this approach is if a node goes down then all the user sessions on that node is gone.

### Session Replication
In this model, the user session data will be replicated on all the nodes so that any request can be routed to any node. Even if one node goes down the client request can be served by another node. But the Session Replication requires better hardware support and involves some node specific configuration.

### Session Server
In this model, the user session data will not be held in node’s memory, instead, it will be persisted into a data store and associate it with SESSION_ID. This solution will be node independent but we may need to write custom code to transparently store the session data in a Persistent datastore whenever a user adds some information to his/her session.

## Reference
- [Comparing Load Balancing Algorithms](http://www.jscape.com/blog/load-balancing-algorithms)
- [Redirection and Load Balancing](http://slideplayer.com/slide/6599069/#)
- [Session Management using Spring Session with JDBC DataStore](https://sivalabs.in/2018/02/session-management-using-spring-session-jdbc-datastore/)

