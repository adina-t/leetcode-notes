## 1962. Remove Stones to Minimize the Total [#](https://leetcode.com/problems/remove-stones-to-minimize-the-total/)

### Additional Cases
<pre>
<b>Case 1:</b>
<b>Input:</b> piles = [1,1,1,1], k = 5
<b>Output:</b> 4
</br>
<b>Case 2:</b>
<b>Input:</b> piles = [2,4,4], k = 10
<b>Output:</b> 3
</pre>

### Intuition
From the cases above and given the constraint that every pile has a positive number of stones, we can observe that the minimum possible sum of stones is the number of piles for an imaginary k of "infinity". Thus, the minimum possible sum of stones is either the number of piles or the remaining number of stones after _greedily_ removing the maximum possible of stones each time for all _k_ operations.

### Proof of Correctness
<!-- (Greedy stays ahead) </br>
Supose, for the sake of contradiction, B<sub>G</sub>, the collection of bags found by our algorithm isn't the maximum number of bags that be filled. Let B' be the true maximum collection of bags.
</br></br>
Let _b_ be the  -->

### Analysis
#### Time Complexity: O(n + klogn)
O(n) for turning the vector into a heap, O(logn) for each heap insertion and removal, and O(n) to compute the remaining sum of stones.
#### Space Complexity: O(1)

```cpp
#include <algorithm>

class Solution {
public:
    int minStoneSum(vector<int>& piles, int k) {
        int sum = 0;
    
        std::make_heap(piles.begin(), piles.end());

        while (k-- > 0) {
            // every pile has exactly 1 stone, we have reached the minimum
            if (piles.front() == 1) break;

            // take the max pile off heap
            std::pop_heap(piles.begin(), piles.end());

            // remove half of the stones from the max pile
            piles.back() -= piles.back() / 2;

            // insert the modified pile back into heap
            std::push_heap(piles.begin(), piles.end());
        }

        // get the sum of stones
        for (int i = 0; i < piles.size(); i++)
            sum += piles[i];

        return sum;
    }
};
```
