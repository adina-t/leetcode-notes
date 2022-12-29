## 75. Sort Colors [#](https://leetcode.com/problems/sort-colors/)

### Intuition
We can solve this problem with the **counting sort** algorithm.

### Analysis
#### Time Complexity: O(n)
One pass through the array getting the counts for each color, which take _O(n)_, and another pass reordering the colors, which also takes _O(n)_.
#### Space Complexity: O(1)
We have only three colors to sort, which is constant, so the array created to store occurrence of colors takes up constant space.

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int color[3] = {};

        // count the occurence of each color
        for (int i = 0; i < nums.size(); i++) 
            color[nums[i]]++;
        
        // reassign elements based on counts of each color
        for (int i = 0; i < nums.size(); i++)
            if (color[0]-- > 0)
                nums[i] = 0;
            else if (color[1]-- > 0)
                nums[i] = 1;
            else nums[i] = 2;

    }
};
```
