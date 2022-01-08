## 605. Can Place Flowers

### Descirption 
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule.

#### Constraints
- 1 <= flowerbed.length <= 2 * 10,000
- flowerbed[i] is 0 or 1.
- There are no two adjacent flowers in flowerbed.
- 0 <= n <= flowerbed.length

### Sol 1: 

```C++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int plantable = 0;
        for(int i = 0; i < flowerbed.size(); i++)
            if(flowerbed[i] == 0 && (i < 1 || flowerbed[i - 1] == 0) && (i > (int) flowerbed.size() - 2 || flowerbed[i + 1] == 0)) {
                flowerbed[i] = 1;
                plantable++;
            }
        
        return plantable >= n;
    }
};
```
Note:
- Runtime: 23 ms
- Memory Usage: 20.4 MB