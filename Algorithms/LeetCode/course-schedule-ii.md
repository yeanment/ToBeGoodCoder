## 210. Course Schedule II

### Descirption 
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai. For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

#### Constraints
- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= numCourses * (numCourses - 1)
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- ai != bi
- All the pairs [ai, bi] are distinct.

### Sol 1: BFS + Queue, Based on Kahn's Algorithm 
Time complexity:- O(V+E) where V is the number of vertices and E is the number of Edges.

Kahn's Algorithm is the algorithm to do topological sorting. In this algorithm we will follow three steps:
1. We will find indegree of every node means we will calculate the adjacent nodes of that vertice which are dependent on it.
2. We will push the node with 0 indegree into the queue.
3. Then we will remove all the nodes one by one from the queue and will reduce the indegree of their adjacent nodes which are prerequisites for it.

We will take a count variable to check if there is a deadlock(cycle) in the graph. Because if there is a cycle in the graph then it means we cant apply 
topological sort in it and there will be no way to complete all the courses.

So, for DAG(Directed acyclic graph) our count variable should be equal to the total no of nodes in the graph.

```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> ret;
        vector<vector<int>> g(numCourses);
        vector<int> degrees(numCourses, 0);
        for(const auto& e: prerequisites){
            g[e[1]].emplace_back(e[0]);
            degrees[e[0]]++; //Note1
        }
        
        queue<int> q;
        for(int i = 0; i < numCourses; i++)
            if(degrees[i] == 0)
                q.push(i);
        int todoCnt=numCourses;
        while(!q.empty()){
            auto cur = q.front(); q.pop(); todoCnt--; ret.push_back(cur);
            for(const auto& next: g[cur])
                if(--degrees[next]==0) q.push(next); //Note3
        }
        
        return todoCnt == 0? ret:vector<int>();
    }
};
```
Note:
- Runtime: 42 ms
- Memory Usage: 13.5 MB

### Sol 2: DFS

```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> g(numCourses);
        vector<int> degrees(numCourses, 0);
        for(const auto& e: prerequisites){
            g[e[0]].emplace_back(e[1]);
            degrees[e[1]]++; //Note1
        }
        
        vector<int> ret;
        vector<bool> seen(numCourses, false);
        stack<int> dfs;
        for(int i = 0; i < numCourses; i++)
            if(degrees[i] == 0)
                dfs.push(i);
        
        int todoCnt=numCourses;
        while(!dfs.empty()){
            // cout<< "sds" << endl;
            int cur = dfs.top();
            if(seen[cur])
            {
                dfs.pop();
                break;
            }
            bool allDone = true;
            for(int i = 0; i < g[cur].size(); i++)
            {
                if(!seen[g[cur][i]] && degrees[g[cur][i]] > 0)
                {
                    degrees[g[cur][i]]--;
                    dfs.push(g[cur][i]);
                    allDone = false;
                    break;
                }
            }
            if(allDone)
            {
                dfs.pop(); todoCnt--; ret.push_back(cur); seen[cur] = true; 
            }
        }
        return todoCnt == 0? ret:vector<int>();
    }
};
```
Note:
- Runtime: 20 ms
- Memory Usage: 13.6 MB


**Another version of DFS with topological sorting and cyclic detect**
```C++
class Solution {
    
     bool isCyclic(vector<vector<int>> &adj,vector<int>& visited,int curr){
        if(visited[curr]==2)
            return true;
        visited[curr]=2;
        for(int i=0; i<adj[curr].size(); i++)
            if(visited[adj[curr][i]]!=1)
                if(isCyclic(adj,visited,adj[curr][i])) return true;
        
        visited[curr]=1;
        return false;
    }
    
    void util(vector<bool> &visited,vector<int> adj[],stack<int> &st,int i){
        visited[i]=true;
        for(auto x: adj[i])
        {
            if(!visited[x]) util(visited,adj,st,x);
        }
        st.push(i);
    }
    
    vector<int> toposort(int V,vector<int> adj[]){
        vector<int> ans;
        stack<int> st;
        vector<bool> visited(V,false);
        for(int i=0; i<V; i++)
        {
            if(!visited[i]) util(visited,adj,st,i);
        }
        
        while(!st.empty())
        {
            ans.push_back(st.top());
            st.pop();
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
    
public:
    vector<int> findOrder(int n, vector<vector<int>>& graph) {
        vector<int> adj[n];
        vector<vector<int>> gg(n);
        for(int i=0; i<graph.size(); i++)
        {
            adj[graph[i][0]].push_back(graph[i][1]);
            gg[graph[i][0]].push_back(graph[i][1]);
        }
         vector<int> visited(n,0);
        for(int i=0; i<n; i++)
        {
            if(visited[i]==0) 
            {
                if(isCyclic(gg,visited,i)) return {};
            }
        }
        return toposort(n,adj);
    }
};
```
Note:
- Runtime: 20 ms
- Memory Usage: 14.6 MB
