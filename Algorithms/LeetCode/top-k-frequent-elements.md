## 347. Top K Frequent Elements

### Descirption 
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

#### Constraints
- 1 <= nums.length <= 100,000
- k is in the range [1, the number of unique elements in the array].
- It is guaranteed that the answer is unique.

### Sol 1: Binary Search

```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int, int> seen;
        vector<int> unique;
        for(int i = 0; i < nums.size(); i++)
        {
            // map<int,int>::iterator it = seen.find(nums[i]);
            // if (it == seen.end())
            //     seen.emplace(nums[i], 0);
            seen[nums[i]]++;
        }
        for (map<int, int>::iterator it=seen.begin(); it!=seen.end(); ++it)
            unique.push_back(it->first);
        
        vector<int> ret(k, 0);
        int idx = 0;
        map<int, int>::iterator it=seen.begin();
        ret[0] = it->first;
        it++;
        while(it != seen.end())
        {
            // binary search and insert
            if(seen[it->first] >= seen[ret[idx]] || idx < k - 1)
            {
               
                int ij = 0, ik = idx;    
                if(seen[ret[ij]] > seen[it->first])
                {
                    while(ij < ik )
                    {
                        int mid = ij + (ik - ij )/2;
                        if(seen[ret[mid]] > seen[it->first])
                        {
                            ij = mid + 1;
                        }
                        else if(seen[ret[mid]] == seen[it->first])
                        {
                            ij = mid;
                            break;
                        }
                        else
                            ik = mid;
                    }
                }

                if(idx < k -1 && seen[it->first] < seen[ret[idx]] )
                {
                    ret[++idx] = it->first;
                }
                else
                {
                    if(idx < k - 1)
                    {
                        ret[idx + 1] = ret[idx];
                        idx++;
                    }
                
                    for(int j = idx; j > ij && j > 0; j--)
                    {
                        ret[j] = ret[j-1];
                    }
                    ret[ij] = it->first;
                }
            }
            ++it;
        }
        return ret;
    }
};
```
Note:
- Runtime: 27 ms
- Memory Usage: 13.7 MB


### Sol 2: Priority Queue

```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> mp;
        priority_queue<pair<int,int>> pq;
        vector<int> ans;
        for(int i:nums)
            ++mp[i];
        for(auto it:mp)
            pq.push({it.second,it.first});
        while(k--)
        {
            ans.push_back(pq.top().second);
            pq.pop();
            if(!pq.empty())
            {
                continue;
            }
        }
        return ans;
    }
};
```
Note:
- Runtime: 11 ms
- Memory Usage: 13.8 MB
