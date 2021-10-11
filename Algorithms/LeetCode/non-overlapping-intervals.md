## 435. Non-overlapping Intervals

### Descirption 
Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

#### Constraints
- 1 <= intervals.length <= 100,000
- intervals[i].length == 2
- -50,000 <= starti < endi <= 50,000

### Sol 1: DFS

```C++
class Solution {
private:
    static bool cmpInterval(vector<int>& iti, vector<int>& itj)
    {
        return (iti[0] < itj[0]);
    }
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int n = intervals.size();
        sort(intervals.begin(), intervals.end(), cmpInterval);
        vector<int> ret(n, 0);
        for(int i = n - 2; i >= 0; i--)
        {
            int iiS = i + 1, intervalRight = intervals[i][1];
            while(iiS < n)
            {
                if(intervals[iiS][0] < intervalRight)
                    iiS++;
                else
                    break;
            }
            if(iiS - i > 1)
            {
                if(iiS < n)
                    ret[i] = min(1 + ret[i + 1], iiS - i - 1 + ret[ iiS]);
                else
                    ret[i] = min(1 + ret[i + 1], iiS - i - 1);
            }
            else
                ret[i] = ret[i + 1];
        }
        return ret[0];
    }
};
```
Note:
- Runtime: Time Limit Exceed
- Memory Usage: 

### Sol 2: Greedy Method
- {all intervals} - {max compatible intervals} = {minimum deleted intervals} Suppose interval A in the latter max compatible set B and A causes two other intervals be deleted. If we delete A instead and insert those two deleted intervals to B can obtain a larger set, then it contradicts B is the max compatible intervals.

The heuristic is: always pick the interval with the earliest end time. Then you can get the maximal number of non-overlapping intervals. (or minimal number to remove). This is because, the interval with the earliest end time produces the maximal capacity to hold rest intervals. E.g. Suppose current earliest end time of the rest intervals is x. Then available time slot left for other intervals is [x:]. If we choose another interval with end time y, then available time slot would be [y:]. Since x ≤ y, there is no way [y:] can hold more intervals then [x:]. Thus, the heuristic holds. Therefore, we can sort interval by ending time and key track of current earliest end time. Once next interval's start time is earlier than current end time, then we have to remove one interval. Otherwise, we update earliest end time.

```C++
class Solution {
private:
    static bool cmpInterval(vector<int>& iti, vector<int>& itj)
    {
        return iti[0] == itj[0] ? (iti[1] < itj[1]):(iti[0] < itj[0]);
    }
    
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int n = intervals.size();
        sort(intervals.begin(), intervals.end(), cmpInterval);
        int ret = 0;
        for(int i = n - 2, intervalLeft = intervals[n - 1][0]; i >= 0; i--)
        {
            if(intervalLeft < intervals[i][1] && intervalLeft >= intervals[i][0])
            {
                ret++;
            }
            else
            {
                intervalLeft = intervals[i][0];
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 448 ms
- Memory Usage: 89.7 MB

To optimize, we can change the type of intervals to `pair<int, int>` reducing the times for sorting.



