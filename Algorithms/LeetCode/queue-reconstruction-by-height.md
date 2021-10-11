## 406. Queue Reconstruction by Height

### Descirption 
You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

#### Constraints
- 1 <= people.length <= 2,000
- 0 <= hi <= 1,000,000
- 0 <= ki < people.length
- It is guaranteed that the queue can be reconstructed.

### Sol 1: Sort and Insert
We put people in an array of length n. The number k means that we should put this person in the kth empty position from the beginning. The empty position mean that there will be higher or equal height person coming in here, so leave these positions out first. For everyone, we should first insert the lower h person. For the person who has same h we should first insert the person has larger k value. For everyone to put in, it takes O(n) time to find kth empty position, so it will take the O(n^2) time for all people.

```C++
class Solution {
private:
    static bool cmpEnd(vector<int>& iti, vector<int>& itj)
    {
        return (iti[1] == itj[1]) ? (iti[0] < itj[0]): (iti[1] < itj[1]);
    }
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmpEnd);
        vector<vector<int>> ret;
        for(int i = 0; i < people.size(); i++)
        {
            vector<vector<int>>::iterator pos = ret.begin();
            for(int cnt = 0; pos != ret.end(); pos++)
            {
                if( (*pos)[0] >= people[i][0])
                {
                    if((cnt++) >= people[i][1])
                    {
                        break;
                    }
                }
            }
            ret.insert(pos, people[i]);
        }
        return ret;
    }
};
```
Note:
- Runtime: 124 ms
- Memory Usage: 11.9 MB


**Optimized in insertion**
```C++
class Solution {
    static bool cmpEnd(vector<int>& iti, vector<int>& itj)
    {
        return (iti[0] == itj[0]) ? (iti[1] < itj[1]): (iti[0] > itj[0]);
    }
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmpEnd);
        // after sort the example we get this vector:  [[7,0],[7,1],[6,1],[5,0],[5,2],[4,4]]
        vector<vector<int>> res;
        for(int i = 0; i< people.size(); i++)
        {
            // insert the biggest height people first and insert in the k-th position
            res.insert(res.begin()+people[i][1], people[i]); 
        }
        return res;
    }
};
```
Note:
- Runtime: 136 ms
- Memory Usage: 12 MB


To optimize the present method, the binary search is introduced to reduce the time into O(nlog n).