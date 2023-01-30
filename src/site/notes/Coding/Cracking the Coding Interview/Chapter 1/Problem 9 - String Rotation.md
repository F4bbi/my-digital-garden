---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1/problem-9-string-rotation/"}
---

# Chapter 1 - Problem 8 - String Rotation
## Problem
Assume you have a method `isSubstring` which checks if one word is a substring of another. Given two strings, `s1` and `s2`, write code to check if `s2` is a rotation of `s1` using only one call to `isSubstring` (e.g.,"waterbottle"is a rotation of "erbottlewat").

## Solution
The key here is that if `s2` is a rotation of `s1`, `s2` will always be a substring of `s1+s1`. We simply have to verify that.

**EXAMPLE:**
_s1:_ "waterbottle"
_s2:_ "erbottlewat"
_s1 + s1:_ "waterbottlewaterbottle"
✅"erbottlewat" found! It starts at position 3.
#### Solution in C++ using two arrays
```cpp
bool isSubstring(std::string s1, std::string s2) {
    if((s1.length() != s2.length()) || s1.length() == 0)
        return false;
    else
        return (s1+s1).find(s2) != std::string::npos;
}
```
- **Time complexity:** $O(s)$ (where _s_ is the size of the string)
- **Space complexity:** $O(1)$ 