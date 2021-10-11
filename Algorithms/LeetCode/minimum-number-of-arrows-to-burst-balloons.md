## 452. Minimum Number of Arrows to Burst Balloons

### Descirption 
There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array points where points[i] = [xstart, xend] denotes a balloon whose horizontal diameter stretches between xstart and xend. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array points, return the minimum number of arrows that must be shot to burst all balloons.

#### Constraints
- 1 <= points.length <= 100,000
- points[i].length == 2
- -2^31 <= xstart < xend <= 2^31 - 1

### Sol 1: Binary Search

```C++
class Solution {
private:
    static bool cmpEnd(pair<int,int>& iti, pair<int, int>& itj)
    {
        return iti.first < itj.first;
    }
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        int n = points.size();
        if(n <= 1)
            return n;
        vector<pair<int, int>> pointsToSort(n);
        for(int i = 0; i < n; i++)
        {
            pointsToSort[i] = (pair<int, int>(points[i][0], points[i][1]));
        }
        sort(pointsToSort.begin(), pointsToSort.end(), cmpEnd);
        int ret = 1;
        for(int i = 1, shotX = pointsToSort[0].second; i < n; i++)
        {
            // cout << pointsToSort[i].first << " : " << pointsToSort[i].second　<< " : " << shotX << endl;
            if(pointsToSort[i].first > shotX)
            {
                ret++;
                shotX = pointsToSort[i].second;
            }
            else if(pointsToSort[i].second < shotX)
            {
                shotX = pointsToSort[i].second;
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 328 ms
- Memory Usage: 92.5 MB
