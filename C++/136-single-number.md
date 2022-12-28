## 136. Single Number [#](https://leetcode.com/problems/single-number/)

### Approach #1 - Naive
For each number in the array, we do a linear search to find if there are two copies of that number until we find the unique element.


### Analysis
#### Time Complexity: O(n)
#### Space Complexity: O(1)

### Solution #1
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int num, seen;
        int n = nums.size();
        
        for (int i = 0; i < n; i++) {
            num = nums[i];
            seen = 0;
            for (int j = 0; j < n; j++) {
                if (num == nums[j]) seen++;
                // found two copies of num
                if (seen == 2) break;
            }

            if (seen == 1) return num;
        }
        // shouldn't happen
        return -1;
    }
};
```

### Approach #2 - XOR
This approach is not as straight-forward as the naive approach. </br>
Consider all numbers in the array written in binary and stacked on top of each other (as shown below). 
We know for a fact that XOR-ing even number of 1-bits gives us 0, so by taking the XOR of all digits of the same order of magnitude, 
the 1's from the duplicate elements "cancel out" and we are left with the digit of the unique element.
Thus, by XOR-ing the bits of all elements, we will end up with the value equal to the unique element.

```                               
   |------- 1 0 0 1 0 0  -> 36               
   |        0 1 1 1 0 1  -> 29               
 Input:     0 1 1 1 0 1  -> 29               
   |        1 0 0 1 0 0  -> 36               
   |_______ 0 0 0 1 1 0  ->  6    (XOR       
--------------------------------------       
Output: --- 0 0 0 1 1 0  ->  6              
```

### Analysis
#### Time Complexity: O(n)
#### Space Complexity: O(1)

### Solution #1
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        int result = 0;
        for (int i = 0; i < n; i++)
            result ^= nums[i];
        
        return result;
    }
};

```
