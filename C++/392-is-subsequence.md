## 392. Is Subsequence [#](https://leetcode.com/problems/is-subsequence/)

### Intuition
To find whether string _s_ is a subsequence of string _t_, we go through each _s[i]_ in order and
check if _s[i]_ can be found in the substring of _t_ starting at _t[k+1]_, where _t[k]_ is matched to _s[i]_ and ending at index _t.size() - 1_.


### Analysis
#### Time Complexity: O(n)
#### Space Complexity: O(1)

```cpp
#include <string>

class Solution {
public:
    bool isSubsequence(string s, string t) {
        size_t pos = 0;
        
        for (int i = 0; i < s.size(); i++) {
            pos = t.find(s.at(i), pos);
            // current character not found in the remaining source string
            if (pos == std::string::npos)
                return false;
            // the remaining source string starts at one index after
            // the character that matches s[i]
            pos++;
        }
        return true;
    }
    
};
```
