## 290. Word Pattern [#](https://leetcode.com/problems/word-pattern/)

### Additional Cases
<pre>
<b>Case 1:</b>
<b>Input:</b> pattern = "aab", s = "dog dog cat cat"
<b>Output:</b> false (more words in s than characters in pattern)
</br>
<b>Case 2:</b>
<b>Input:</b> pattern = "aab", s = "dog dog"
<b>Output:</b> false (fewer words in s than characters in pattern)
</pre>

### Approach #1
This question is similar to [#205](205-isomorphic-strings.md). We can create two maps to keep track of the mapping from characters
in _pattern_ to words in _s_ and words in _s_ to characters in _pattern_ and check the mappings are one-to-one in both direction.

### Approach #2
Another approach is a modified version of the third approach in [#205](205-isomorphic-strings.md). We take the same idea 
to track the **index_last_seen**, the index where a character or word is last seen in _pattern_ or _s_, as we progress through 
_pattern_ and _s_. There is a valid bijection between letters in _pattern_ and words in _s_ if 
each i-th letter in _pattern_ and i-th word in _s_ has the same **index_last_seen** for all i.

### Analysis
#### Time Complexity: O(n)
**n** is the size of the _pattern_ string
#### Space Complexity: O(1)
There are only costant number of lowercase English characters (26 of them), so the character map (_char_last_seen_) takes up constant space. 
</br>
The word map (_word_last_seen_) will also take up at most a constant amount of space, since each unique character in _pattern_ is map to one word exactly. 
More specifically, we will never process more than (26 + 1) = 27 unique words in our algorithm, because once we see the 27-th unique word, it must be that some character in _pattern_ 
is attempting to be mapped to at least two words, which would cause the algorithm to return at this point (if not earlier).


```cpp
#include <string>
#include <cstring>  // for std::strtok, std::strcpy
#include <unordered_map>

const int LOWERCASE_CHARS_COUNT = 26;

class Solution {
public:
    bool wordPattern(string pattern, string s) {
        // two maps that map char in pattern and word in string
        // to the index they were last mapped
        unordered_map<char, int> char_last_seen;
        unordered_map<string, int> word_last_seen;

        char* s_cpy = new char [s.size() + 1];  // need a separate c-string copy of s
        std::strcpy (s_cpy, s.c_str());         // since std::strtok takes non-constant c-strings 
        char* word = std::strtok (s_cpy, " ");

        int i;
        for (i = 0; i < pattern.size(); i++) {
            // pattern longer than number of words
            if (word == NULL) break;

            bool char_seen = char_last_seen.count(pattern[i]);
            bool word_seen = word_last_seen.count(word_cpy);
            string word_cpy (word);

            // char is mapped already but word isn't, or vice versa
            if (char_seen != word_seen)
                break;
            // char was last mapped to some other word, or vice versa
            else if (char_last_seen[pattern[i]] != word_last_seen[word_cpy])
                break;
            else {
                // update that char was last mapped to word at current index
                char_last_seen[pattern[i]] = i + 1;
                word_last_seen[word_cpy] = i + 1;
                word = strtok(NULL, " ");
            }
            
        }

        delete[] s_cpy;
        
        // 1) all characters in pattern have one-to-one mapping to words
        // 2) pattern has the same size as the number of words 
        return i == pattern.size() && word == NULL;
    }
};
```
