## 207. Course Schedule

### Descirption 
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai. For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return true if you can finish all courses. Otherwise, return false.

#### Constraints
- 1 <= numCourses <= 105
- 0 <= prerequisites.length <= 5000
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- All the pairs prerequisites[i] are unique.

### Sol 1: DFS
```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        // Essentially detect if there is a loop
        map<int, set<int>> preMap;
        for(int i = 0; i < prerequisites.size(); i++)
        {
            if(preMap.find(prerequisites[i][0]) == preMap.end())
            {
                // Not exist
                set<int> tmp = {prerequisites[i][1]};
                preMap.insert(make_pair(prerequisites[i][0], tmp));
            }
            else
            {
                preMap[prerequisites[i][0]].emplace(prerequisites[i][1]);
            }
        }
        set<int> seen;
        for(map<int, set<int>>::iterator it = preMap.begin(); it != preMap.end(); it++)
        {
            if(seen.find(it->first) != seen.end())
                continue;
            stack<int> dfs;
            dfs.push(it->first);
            set<int> dfspath = {it->first};
            
            while(!dfs.empty())
            {
                int onNode = dfs.top();
                bool allSeen = true;
                for(set<int>::iterator itj = preMap[onNode].begin(); itj != preMap[onNode].end(); itj++)
                {
                    if(preMap.find(*itj) == preMap.end())
                        continue;
                    if((seen.find(*itj) == seen.end()))
                    {
                        dfs.push(*itj);
                        if(!dfspath.emplace(*itj).second)
                            return false;
                        allSeen = false;
                        break;
                    }
                }
                if(allSeen)
                {
                    dfspath.erase(onNode);
                    dfs.pop();
                    seen.emplace(onNode);
                }
            }
            seen.emplace(it->first);
        }
        return true;
    }
};
```
Note:
- Runtime: 24 ms
- Memory Usage: 19.2 MB


### Sol 2: BFS
```C++
class Solution {//BEST2: BFS:Time/Space: O(N); O(N)
public:// prerequisites: {child, parent}
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> g(numCourses);
        vector<int> degrees(numCourses, 0);
        for(const auto& e: prerequisites){
            g[e[1]].emplace_back(e[0]);
            degrees[e[0]]++; //Note1
        }
        
        queue<int> q;
        for(int i = 0; i < numCourses; i++)
            if(degrees[i] == 0) q.push(i);  // Note2
        int todoCnt=numCourses;
        while(!q.empty()){
            auto cur = q.front(); q.pop(); todoCnt--;
            for(const auto& next: g[cur])
                if(--degrees[next]==0) q.push(next); //Note3
        }
        return todoCnt == 0;
    }
};
```
Note:
- Runtime: 20 ms
- Memory Usage: 13.4 MB
