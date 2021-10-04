## 16. 3Sum Closest

### Descirption 
Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

#### Constraints
- 3 <= nums.length <= 1000
- -1000 <= nums[i] <= 1000
- -10000 <= target <= 10000

### Sol 1: using the brute force search

```C++
class Solution {
    public:
    int threeSumClosest(vector<int>& nums, int target){

        int n = nums.size();
        int res = 1E+6, targ = 0;
        int sum = 0;
        for(int i = 0; i < n ; i++)
        {
            for(int j = i + 1; j < n; j++)
            {
                for(int k = j + 1; k < n; k++)
                {
                    sum = nums[i] + nums[j] + nums[k];
                    if(abs(sum - target) < res)
                    {
                        res = abs(sum - target);
                        if(res == 0)
                            return target;
                        targ = sum;
                    }
                }
            }
        }
        return targ;
    }
};
```
Note:
- Runtime: 40 ms
- Memory Usage: 10 MB
### Sol 2: using two pointer technique
```C++
class Solution {
    public:
    int threeSumClosest(vector<int>& nums, int target){

        int n = nums.size();
        int res = 1E+6, targ;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < n - 2 ; i++)
        {
            int j = i + 1, k = n - 1;
            while(j < k)
            {
                int sum = nums[i] + nums[j] + nums[k];
                if(abs(target - sum) < res)
                {
                    res = abs(target - sum);
                    targ = sum;
                }
                if(sum > target)
                    k--;
                else 
                    j++;

            }
        }
        return targ;
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 9.8 MB

### Sol 3: with binary search
**Optimized for runtime**
```C++
class Solution {
private:
    int binary_search(vector<int>& nums, int target) {
        int i = 0, n = nums.size(), j = n - 1;
        int middle = (i + j)/2;
        //cout << target << endl;
        while(i != j && j - i > 1 && middle >= 0 && middle < n) {
            //cout << i << ", " << j << ", " << middle << endl;
            if(nums[middle] == target) {
                return middle;
            } else if(nums[middle] < target) {
                i = middle;
            } else {
                j = middle;
            }
            middle = (i + j)/2;
        }
        return (j - i) == 1 ? (abs(nums[j] - target) > abs(nums[i] - target) ? i : j) : i;
    }
public:
    int threeSumClosest(vector<int>& nums, int target) {        
        sort(nums.begin(), nums.end());
        int i = 0, j = nums.size() - 1;
        int closest_diff = numeric_limits<int>::max();
        int closest = 0; 
        while(i < j) {
            int p_sum = nums[i] + nums[j];
            //cout << p_sum << ", ";
            int elem = binary_search(nums, target - p_sum);
            if(elem == i || elem == j) {
                //cout << "inside ";
                const int a1 = (i - 1) >= 0 ? abs((nums[i - 1] + p_sum) - target) : numeric_limits<int>::max();
                const int a2 = (i + 1) < nums.size() && i + 1 != j? abs((nums[i + 1] + p_sum) - target) : numeric_limits<int>::max();
                const int a3 = (j - 1) >= 0 && j - 1 != i ? abs((nums[j - 1] + p_sum) - target) : numeric_limits<int>::max();
                const int a4 = (j + 1) < nums.size() ? abs((nums[j + 1] + p_sum) - target) : numeric_limits<int>::max();
                
                int min_diff = min({a1, a2, a3, a4});
                if(min_diff == a1) {
                        elem = i - 1;
                } else if(min_diff == a2) {
                        elem = i + 1;
                } else if(min_diff == a3) {
                        elem = j - 1;
                } else if(min_diff == a4) {
                        elem = j + 1;                
                }
                
            }
            p_sum += nums[elem];

            if(abs(p_sum - target) < closest_diff) {
                closest_diff = abs(p_sum - target);
                closest = p_sum;
            }
            if(p_sum == target) {
                return target;
            } else if(p_sum > target) {
                j--;
            } else {
                i++;
            }
        }
        return closest;
    }
};

// Time complexity: O(NlogN)
// Space: O(1)
```
Note:
- Runtime: 4 ms
- Memory Usage: 9.9 MB