## 1192. Critical Connections in a Network

### Descirption 
There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.

#### Constraints
- graph.length == n
- 2 <= n <= 100,000
- n - 1 <= connections.length < 10,000
- 0 <= ai < bi <= n - 1
- ai != bi
- There are no repeated connections.

### Sol 1: DFS, Tarjan Alorithm
An edge is a critical connection, if and only if it is not in a cycle. So, if we know how to find cycles, and discard all edges in the cycles, then the remaining connections are a complete collection of critical connections.

We will use DFS algorithm to find cycles and decide whether or not an edge is in a cycle.

Define rank of a node: The depth of a node during a DFS. The starting node has a rank 0.

Only the nodes on the current DFS path have non-special ranks. In other words, only the nodes that we've started visiting, but haven't finished visiting, have ranks. So 0 <= rank < n.

(For coding purpose, if a node is not visited yet, it has a special rank -2; if we've fully completed the visit of a node, it has a special rank n.)

How can "rank" help us with removing cycles? Imagine you have a current path of length k during a DFS. The nodes on the path has increasing ranks from 0 to k and incrementing by 1. Surprisingly, your next visit finds a node that has a rank of p where 0 <= p < k. Why does it happen? Aha! You found a node that is on the current search path! That means, congratulations, you found a cycle!

But only the current level of search knows it finds a cycle. How does the upper level of search knows, if you backtrack? Let's make use of the return value of DFS: dfs function returns the minimum rank it finds. During a step of search from node u to its neighbor v, if dfs(v) returns something smaller than or equal to rank(u), then u knows its neighbor v helped it to find a cycle back to u or u's ancestor. So u knows it should discard the edge (u, v) which is in a cycle.

After doing dfs on all nodes, all edges in cycles are discarded. So the remaining edges are critical connections.

```C++
class Solution {
public:
    //use find the bridges in a graph approach to get all critical connections
    void dfs(int node, int parent, vector<int> &tin, vector<int> &low, vector<int> &vis, vector<int> graph[], vector<vector<int>> &ans, int &time)
    {
        //when we visite node first time, tin and low of the node are equal to the time
        tin[node]=low[node]=time++;
        vis[node] = 1; //mark node as visited
        
        for(auto it : graph[node])
        {
            if(it == parent) continue; //to avoid backtracking            
            //if node is not visited, call the dfs function
            if(!vis[it])
            {
                dfs(it, node, tin, low, vis, graph, ans, time);
                //when we return, low of the node is equal to the minimum of low of its child and low of itself
                low[node] = min(low[node], low[it]);
                //when we get low of child is greater than tin of the node
                //it means there is only path to cover child that's why {node, it} will be our critical connections or bridge 
                if(low[it] > tin[node])
                    ans.push_back({node, it});
            }
            
            //if node is already visited and tin of the child is less than the low of itself
            //we assign tin of the child to the low of the node
            else
                low[node] = min(low[node], tin[it]);
            
        }
    }
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) 
    {
        //tin - time of insertion at the node
        //low - lowest time of insertion at the node
        //vis - mark visited node 
        vector<int> tin(n, -1), low(n, -1), vis(n, 0);
        vector<vector<int>> ans;
        vector<int> graph[n]; //stores connections in the form of graph
        int time=0;
        
        for(auto it: connections)
        {
            graph[it[0]].push_back(it[1]);
            graph[it[1]].push_back(it[0]);
        }
        
        //call dfs function
        dfs(0, -1, tin, low, vis, graph, ans, time);
        return ans;
    }
};
```
Note:
- Runtime: 681 ms
- Memory Usage: 174.8 MB
