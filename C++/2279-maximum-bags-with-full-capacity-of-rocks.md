## 2279. Maximum Bags With Full Capacity of Rocks [#](https://leetcode.com/problems/maximum-bags-with-full-capacity-of-rocks/)

### Intuition
One _greedy_ approach is to can calculate the remaining capacity of each bag, order them in ascending order, and then fill the bags with the given additional rocks until we run out of it. 

### Proof of Correctness
<!-- (Exchange argument) </br>
Supose, for the sake of contradiction, B<sub>G</sub>, the collection of bags found by our algorithm isn't the maximum number of bags that be filled. Let B' be the true maximum collection of bags.
</br></br>
Let _b_ be the  -->

### Analysis
#### Time Complexity: O(nlogn)
O(n) for calculating the remaining capacity, O(nlogn) to sort the array, and O(n) to fill the bags to capacity. 
#### Space Complexity: O(1)
We can optimize the space complexity from linear to constant by storing the remaining capacity directly in the capacity array, since the original capacity is not relavant once we know the remaining capacity.

### Solution
```cpp
#include <algorithm>  // for sort()

class Solution {
public:
    int maximumBags(vector<int>& capacity, vector<int>& rocks, int additionalRocks) {
        // num of bags
        int n = capacity.size();

        // calculate the remaining capacity of each bag
        for (int i = 0; i < n; i++) 
            capacity[i] -= rocks[i];
        
        // sort the remaining capacity in ascending order
        std::sort (capacity.begin(), capacity.end());

        for (int i = 0; i < n; i++) {
            additionalRocks -= capacity[i];
            // fewer rocks than needed to fill current bag
            if (additionalRocks < 0) return i;
        }
        
        // all bags filled
        return n;
    }
};
```
