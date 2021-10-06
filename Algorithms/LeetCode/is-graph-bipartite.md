## 785. Is Graph Bipartite?

### Descirption 
There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v. The graph has the following properties:
- There are no self-edges (graph[u] does not contain u).
- There are no parallel edges (graph[u] does not contain duplicate values).
- If v is in graph[u], then u is in graph[v] (the graph is undirected).
- The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.

A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.

Return true if and only if it is bipartite.

#### Constraints
- graph.length == n
- 1 <= n <= 100
- 0 <= graph[u].length < n
- 0 <= graph[u][i] <= n - 1
- graph[u] does not contain u.
- All the values of graph[u] are unique.
- If graph[u] contains v, then graph[v] contains u.

### Sol 1: BFS 

```C++
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<bool> seen(n, false);
        vector<bool> color(n, false);
        for(int i = 0; i < n; i++)
        {
            if(seen[i])
                continue;
            seen[i] = true;
            queue<int> bfs;
            bfs.push(i);
            while(!bfs.empty())
            {
                int onNode = bfs.front();
                for(int j = 0; j < graph[onNode].size(); j++)
                {
                    if(!seen[graph[onNode][j]])
                    {
                        bfs.push(graph[onNode][j]);
                        color[graph[onNode][j]] = !(color[onNode]);
                        seen[graph[onNode][j]] = true;
                    }
                    else if(color[graph[onNode][j]] == (color[onNode]))
                        return false;
                }
                bfs.pop();
            }
        }
        return true;
    }
};
```
Note:
- Runtime: 34 ms
- Memory Usage: 13.7 MB

### Sol 2: DFS
```C++
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<bool> seen(n, false);
        vector<bool> color(n, false);
        for(int i = 0; i < n; i++)
        {
            if(seen[i])
                continue;
            seen[i] = true;
            stack<int> dfs;
            dfs.push(i);
            while(!dfs.empty())
            {
                int onNode = dfs.top();
                bool allSeen = true;
                for(int j = 0; j < graph[onNode].size(); j++)
                {
                    if(!seen[graph[onNode][j]])
                    {
                        dfs.push(graph[onNode][j]);
                        color[graph[onNode][j]] = !(color[onNode]);
                        seen[graph[onNode][j]] = true;
                        allSeen = false;
                        break;
                    }
                    else if(color[graph[onNode][j]] == (color[onNode]))
                        return false;
                }
                if(allSeen)
                    dfs.pop();
            }
        }
        return true;
    }
};
```
Note:
- Runtime: 44 ms
- Memory Usage: 13.7 MB