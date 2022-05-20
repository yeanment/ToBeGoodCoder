## 75. Sort Colors

### Descirption 
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

#### Constraints
- n == nums.length
- 1 <= n <= 300
- nums[i] is either 0, 1, or 2.

### Sol 1: Two-pointer

```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int i = 0, ii = 0, ij = nums.size() - 1;
        while(i <= ij)
        {
            switch (nums[i])
            {
                case 0:
                    swap(nums[i++], nums[ii++]); break;
                case 1:
                    i++; break;
                case 2:
                    swap(nums[i], nums[ij--]); break;
            }
        }
        return;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 8.3 MB

### Sol 2:
```C++


class Solution {
public:
	void sortColors(vector<int>& arr) {
        int c0=0, c1=0, c2=0;
        for(int i=0; i<arr.size(); i++){
            if(arr[i]==0)   c0++;
            if(arr[i]==1)   c1++;
            if(arr[i]==2)   c2++;
        }
        
        if(c0){
            for(int i=0; i<c0; i++) arr[i]=0;
        }
        if(c1){
            for(int i=c0; i<c0+c1; i++) arr[i]=1;
        }
        if(c2){
            for(int i=c0+c1; i<arr.size(); i++) arr[i]=2;
        }
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 8.1 MB