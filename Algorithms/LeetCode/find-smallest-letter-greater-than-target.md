## 744. Find Smallest Letter Greater Than Target

### Descirption 
Given a characters array letters that is sorted in non-decreasing order and a character target, return the smallest character in the array that is larger than target.

Note that the letters wrap around. For example, if target == 'z' and letters == ['a', 'b'], the answer is 'a'.

#### Constraints
- 2 <= letters.length <= 104
- letters[i] is a lowercase English letter.
- letters is sorted in non-decreasing order.
- letters contains at least two different characters.
- target is a lowercase English letter.

### Sol 1:

```C++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int l = 0, h = letters.size();
        if(target < letters[l] || target >= letters[h - 1]) return letters[0];
        while(l < h)
        {
            int mid = l + (h - l)/2;
            if(letters[mid] <= target)
                l = mid + 1;
            else
                h = mid;
        }
        return letters[l];
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 15.9 MB
