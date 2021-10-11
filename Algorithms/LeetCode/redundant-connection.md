## 684. Redundant Connection

### Descirption 
In this problem, a tree is an undirected graph that is connected and has no cycles.

You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph.

Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.

#### Constraints
- n == edges.length
- 3 <= n <= 1,000
- edges[i].length == 2
- 1 <= ai < bi <= edges.length
- ai != bi
- There are no repeated edges.
- The given graph is connected.

### Sol 1: Union-find Algorithm
The redundant edge will be the one that links together an already-linked graph. To determine whether already-seen segments of the graph have been connected, we can use a simple union-find (UF) implementation to keep track of the different segments. With UF, we must define two functions: union and find. The find function will recursively trace a node's lineage back to its ultimate parent and update its value in the parent array (par), providing a shortcut for the next link. The union function merges two segments by assigning the ultimate parent of one segment to another.

We can iterate through edges and find both vertices of the edge to see if they belong to the same segment. If so, we've located our redundant edge and can return it. If not, we should merge the two different segments with union.
- Time Complexity: O(N) where N is the length of edges
- Space Complexity: O(N) for par and the recursion stack


```C++
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        par.resize(edges.size()+1);
        for (int i = 0; i < par.size(); i++)
            par[i] = i;
        for (auto& e : edges)
            if (find(e[0]) == find(e[1])) return e;
            else uniun(e[0],e[1]);
        return edges[0];
    }
private:
    vector<int> par;
    int find(int x) {
        if (x != par[x]) par[x] = find(par[x]);
        return par[x];
    }
    void uniun(int x, int y) {
        par[find(y)] = find(x);
    }
};
```
Note:
- Runtime: 7 ms
- Memory Usage: 8.8 MB

### Sol 2: DFS
We need to find a cycle in the given graph and return an edge in that cycle that occurs last in input edges. We will  find the cycle and the edges involved in that cycle of the graph, and store these edges. For this we can use DFS traversal with small modification. We will start the dfs on the given graph from node 1 and maintain a vis array denoting the nodes that are visited. We will also keep a par variable to denote the parent of current node cur. In each dfs. we will iterate over the child nodes of cur. If we ever visit an already visited node, we know that we are in a cycle. So, we will mark this node as the cycleStart and return. We will start pushing all nodes from the recursion stack until we reach cycleStart node (its first visit recursive call).

We have the following cases:
1. child == par : The child node of cur is it's parent. So, skip it.
2. cycle.empty() == true: We haven't yet detected any cycle. So we will continue exploring the graph with recursive dfs calls.
3. cycleStart != -1: We have detected a cycle and marked the starting node of it. We will stop further dfs calls as cycle is found. We push all the nodes till we reach back to node numbered - cycleStart since they are all part of the cycle.
4. cur == cycleStart: We have reached back to the start of cycle. By now, we have pushed all nodes in the cycle. So, just mark cycleStart as -1 denoting we don't need to further push any nodes and return.

Finally, cycle contains all the nodes of cycle in the graph. We will iterate over the input edges in the reverse order to find the last edge in it that's part of cycle. If both node of an edge is in cycle, then we will return that edge.

```C++
class Solution {
public:
    unordered_set<int> cycle;   // will store all nodes of cycle
    int cycleStart = -1;        // used to mark start node of cycle
    vector<int> findRedundantConnection(vector<vector<int>>& e) {
        int n = size(e);
        vector<vector<int>> graph(n+1);
        vector<bool> vis(n+1);                
        // constructing the graph from the edges
        for(auto& edge : e) graph[edge[0]].push_back(edge[1]), graph[edge[1]].push_back(edge[0]);
        dfs(graph, vis, 1);   // dfs traveral to detect cycle and fill the those nodes in cycle set.
        for(int i = n-1; ~i; i--)
            if(cycle.count(e[i][0]) && cycle.count(e[i][1])) return e[i];  // last edge of input having both nodes in cycle
        return { };    // un-reachable
    }
    void dfs(vector<vector<int>>& graph, vector<bool>& vis, int cur, int par = -1) {
        if(vis[cur]) { cycleStart = cur; return; }              // reached an visited node - mark it as start of cycle and return
        vis[cur] = true;                                        // not visited earlier - mark it as visited
        for(auto child : graph[cur]) {                          // iterate over child / adjacents of current node
            if(child == par) continue;                          // dont visit parent again - avoids back-and-forth loop
            if(cycle.empty()) dfs(graph, vis, child, cur);      // cycle not yet detected - explore graph further with dfs
            if(cycleStart != -1) cycle.insert(cur);             // cycle detected - keep pushing nodes till we reach start of the cycle
            if(cur == cycleStart) { cycleStart = -1; return; }  // all nodes of cycle taken - now just return
        }
    }
};
```
Note:
- Runtime: 13 ms
- Memory Usage: 9.8 MB
