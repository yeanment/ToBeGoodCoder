## 169. Majority Element

### Descirption 
Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

#### Constraints
- n == nums.length
- 1 <= nums.length <= 50,000
- -2^31 <= nums[i] <= 2^31 - 1

### Sol 1:

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        map<int, int> cntMap;
        for(int i = 0; i < nums.size(); i++)
        {
            pair<map<int,int>::iterator, bool> itpair = cntMap.emplace(nums[i], 1);
            if(!itpair.second)
                itpair.first->second +=1;
        }
        int nMax = 0, ret;
        for(map<int,int>::iterator it=cntMap.begin(); it!=cntMap.end(); ++it)
        {
            if(it->second > nMax)
            {
                nMax = it->second;
                ret = it->first;
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 40 ms
- Memory Usage: 3.1 MB

### Sol 2:

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size()/2];
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 19.6 MB

### Sol 3: Boyer-Moore Majority Vote Algorithm
When count != 0 , it means nums[1...i] has a majority,which is major in the solution.

When count == 0 , it means nums[1...i] doesn't have a majority, so nums[1...i] will not help nums[1...n].And then we have a subproblem of nums[i+1...n].

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int major=nums[0], count = 1;
        for(int i=1; i<nums.size();i++){
            if(count==0){
                count++;
                major=nums[i];
            }else if(major==nums[i]){
                count++;
            }else count--;
        }
        return major;
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 19.7 MB

