## 1962. Remove Stones to Minimize the Total [#]([https://leetcode.com/problems/maximum-bags-with-full-capacity-of-rocks/](https://leetcode.com/problems/remove-stones-to-minimize-the-total/)

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

```cpp
class Solution {
public:
    int minStoneSum(vector<int>& piles, int k) {
        int sum = 0;
    
        std::make_heap(piles.begin(), piles.end());

        while (k-- > 0) {
            // every pile has exactly 1 stone, we have reached the minimum
            if (piles.front() == 1) break;

            // remove the max pile from heap
            std::pop_heap(piles.begin(), piles.end());

            // remove half of the stones from pile
            piles.back() -= piles.back() / 2;

            std::push_heap(piles.begin(), piles.end());
        }

        // get the sum of stones
        for (int i = 0; i < piles.size(); i++)
            sum += piles[i];

        return sum;
    }
};
```
