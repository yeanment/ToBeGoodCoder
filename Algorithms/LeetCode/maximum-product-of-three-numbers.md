## 628. Maximum Product of Three Numbers

### Descirption 
Given an integer array nums, find three numbers whose product is maximum and return the maximum product.

#### Constraints
- 3 <= nums.length <= 10,000
- -1,000 <= nums[i] <= 1,000

### Sol 1:

```C++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        if(nums.size() == 3)
            return nums[0]*nums[1]*nums[2];
        sort(nums.begin(), nums.end());
        int ret = numeric_limits<int>::min();
        ret = max(nums[nums.size()-3]*nums[nums.size()-2]*nums[nums.size()-1], nums[0]*nums[1]*nums[nums.size()-1]);
        return ret;
    }
};
```
Note:
- Runtime: 44 ms
- Memory Usage: 27.8 MB

### Sol 2:
```C++
class Solution {
public:
	int maximumProduct(vector<int>& nums) {
		int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN;
		int min1 = INT_MAX, min2 = INT_MAX, min3 = INT_MAX;
		for(int i = 0; i < nums.size(); i++){
			if(nums[i] <= min1){
				min2 = min1;
				min1 = nums[i];
			} 
			else if(nums[i] <= min2){
				min2 = nums[i];
			}
			if(nums[i] >= max1){ 
				max3 = max2;
				max2 = max1;
				max1 = nums[i];
			} 
			else if(nums[i] >= max2){
				max3 = max2;
				max2 = nums[i];
			} 
			else if(nums[i] >= max3){
				max3 = nums[i];
			}
		}
		return max(min1 * min2 * max1, max1 * max2 * max3);
	}
};
```
Note:
- Runtime: 32 ms
- Memory Usage: 27.8 MB