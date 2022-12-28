## 205. Isomorphic Strings [#](https://leetcode.com/problems/isomorphic-strings/?envType=study-plan&id=level-1)

### Additional Cases
<pre>
<b>Case 1:</b>
<b>Input:</b> s = "ba", t = "cc"
<b>Output:</b> false
</pre>

### Intuition
Isomorphism stands for a one-to-one (correspondence) mapping between two sets. This means each character in _s_ maps 
to exactly one character in _t_, and each character in _t_ maps to exactly one character in _s_. This means we need two separate
data structures to keep track of the mapping of characters in both directions. 

### Approach #1
Create a hash map for mapping character in _t_ to _s_, _T_Map_, and another one from _s_ to _t_, _S_Map_. For i-th character in _s_ and in _t_, 
call them _s[i]_ and _t[i]_,
mapped _s[i]_ and _t[i]_ to each other in both maps if both are not mapped yet. If only _t[i]_ is mapped in _T_Map_ 
(or only _s[i]_ is mapped in _S_Map_), it must be that _t[i]_ isn't mapped to _s[i]_ in _T_Map (or _s[i]_ isn't mapped to _t[i]_ in _S_Map) so we return false.
If both _t[i]_ is mapped in _T_Map_ and _s[i]_ is mapped in _S_Map_, we check if _t[i]_ and _s[i]_ are mapped to each other in both maps and return false if not.

### Proof of Correctness
Our cases are exhaustive since we cover all three possible cases (if both _s[i]_ and _t[i]_ are mapped, if only one is mapped, and if none are mapped yet for each character in the string)
and check for consistency in the mapping.

### Analysis
#### Time Complexity: O(n)
#### Space Complexity: O(1)
The number of ASCII characters is constant, and so will the two maps created for store mapping between ASCII characters.

### Solution #1
```cpp
#include <string>
#include <unordered_map>

class Solution {
public:
    bool isIsomorphic(string s, string t) {
        std::unordered_map<char, char> st_map;       
        std::unordered_map<char, char> ts_map;

        for (int i = 0; i < s.size(); i++) {
            if (st_map.count(s.at(i)) == 0) {
                if (ts_map.count(t.at(i)) != 0) return false;
                st_map[s.at(i)] = t.at(i);
                ts_map[t.at(i)] = s.at(i);
            }
            
            else if (st_map[s.at(i)] != t.at(i) ||
                     ts_map[t.at(i)] != s.at(i))
                return false;
        }

        return true;


    }
};
```
### Approach #2
An optimized version of **Approach #1**. Since the number of ASCII is constant, we can use a array instead of std::unordered_map 
to store the mappings. Note that this optimization doesn't change the space complexity as the number of entries in **Approach #1** is constant as well.

### Analysis
#### Time Complexity: O(n)
#### Space Complexity: O(1)

### Solution #2
```cpp
#include <string>

const int NUM_OF_ASCII_CHARS = 256;

class Solution {
public:
    
    bool isIsomorphic(string s, string t) {
        int st_map[NUM_OF_ASCII_CHARS] = {};  // initialize elements to all zeros, which is find since 0 in ASCII       
        int ts_map[NUM_OF_ASCII_CHARS] = {};  // is a control code and won't collide with characters in s or t

        for (int i = 0; i < s.size(); i++) {
            if (st_map[s.at(i)] == 0) {
                if (ts_map[t.at(i)] != 0)
                    return false;
                st_map[s.at(i)] = t.at(i);
                ts_map[t.at(i)] = s.at(i);
                    
            } else if (st_map[s.at(i)] != t.at(i) ||
                     ts_map[t.at(i)] != s.at(i))
                return false;
        }

        return true;
    }
};
```

### Approach #3
This approach is not as straight-forward. The intuition is that instead of tracking the mapping of characters in _s_ to _t_ and vice versa, 
we allow the same character in _s_ or _t_ to be mapped over and over and store in the maps, the index in the string at which it was last mapped, or **last_mapped_at_index**.
After processing the entire string, we return true if both maps have the same **last_mapped_at_index** for each _s[i]_ and _t[i]_ pair, and false otherwise.



<!-- ### Proof of Correctness
For a isomorphic string, the characters in _s_ and _t_ have a one-to-one mapping, so _s[i]_ and _t[i]_ are always assigned to map to each other at the same time, 
so their **last_mapped_at_index** will always stay the same for all _i_'s. </br>
For a non-isomorphic string, there is at least one character s[k] in _s_ that is mapped to two distinct characters in _t_, call them _t[k]_ and _t[x]_, where _k_ < _x_.
We know that _s[k]_ in _s_map_ and _t[k]_ in _t_map_ will have the same **last_mapped_at_index**, which is _k_ + 1 (1-indexed), but when we get to _t[x]_, _s[k]_ (or equivalently _s[x]_)
in _s_map_ is updated to have the value _x_ + 1 while the value of _t[k]_ in _t_map_ stays the same.
A similar argument can be made for double mapping from _t_ to _s_, so we've proven the algorithm is corrrect. -->

### Analysis
#### Time Complexity: O(n)
#### Space Complexity: O(1)

### Solution #2
```cpp
#include <string>
#include <unordered_map>

const int NUM_OF_ASCII_CHARS = 256;

class Solution {
public:
    
    bool isIsomorphic(string s, string t) {
        int s_map[NUM_OF_ASCII_CHARS] = {};         
        int t_map[NUM_OF_ASCII_CHARS] = {};

        for (int i = 0; i < s.size(); i++) {
            // map s[i] and t[i] to each other
            // by updating the index s[i] was last mapped in s_map
            //                       t[i] was last mapped in t_map
            s_map[s[i]] = i + 1;           // 1-indexed since elements are 0-initialized
            t_map[t[i]] = i + 1;
        }

        for (int i = 0; i < s.size(); i++)
            // at least one is mapped to two or more characters
            if (s_map[s[i]] != t_map[t[i]])
                return false;

        return true;
    }
};
```
