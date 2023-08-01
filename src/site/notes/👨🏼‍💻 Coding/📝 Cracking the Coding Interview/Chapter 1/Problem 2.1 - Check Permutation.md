---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1/problem-2-1-check-permutation/","created":"2022-08-04T16:45:33.167+02:00","updated":"2023-07-24T15:38:36.084+02:00"}
---

# Chapter 1 - Problem 2.1 - Check Permutation
#leetcode/242
## Problem
Given two strings, write a method to decide if one is a permutation of the other.

## Solution
First we have to decide if the string is an ASCII string or a Unicode string. We'II assume for simplicity the character set is ASCII. See [[👨🏼‍💻 Coding/📝 Cracking the Coding Interview/Chapter 1/Problem 1 - IsUnique\|Problem 1 - IsUnique]] for more details.
In addition, we assume for this problem that the comparison is case sensitive and whitespace is significant.
Remember that strings of different lengths cannot be permutations of each other.
#### Solution in C++ using sort
lf two strings are permutations, then we know they have the same characters, but in different orders.
Therefore, first we sort the string and then check if they are equals.
```cpp
bool checkPermutation(std::string s1, std::string s2) {  
    if(s1.size() != s2.size()) return false;  
    std::sort(s1.begin(), s1.end());  
    std::sort(s2.begin(), s2.end());  
    return (s1 == s2);  
}
```
- **Time complexity:** $O(N\log N)$ (it depends on the sort algorithm)
- **Space complexity:** $O(1)$

#### Solution in C++ with array
In the first loop we simply count how many times each character appears in `s1` and amount it in an array. Each element in the array rapresents the corresponding value in the ASCII table `(char_set[97] = 'a', ...)`.
In the second loop, we do the same for `s2`, but we decrease the value, so if it goes below zero it means the frequency of the characters of the two strings are different.

```cpp
bool checkPermutation(std::string s1, std::string s2) {  
    if(s1.size() != s2.size()) return false;  
    std::array<int, 128> char_set = {};  
    for(char c : s1) {  
        int index = int(c);  
        char_set.at(index)++;  
    }  
    for(char c : s2) {  
        int index = int(c);  
        char_set.at(index)--;  
        if(char_set.at(index) < 0)  
            return false;  
    }  
    return true;  
}
```
- **Time complexity:** $O(N)$ (one pass over `s1`, one pass for `s2`)
- **Space complexity:** $O(1)$ (`char_set` will always have 128 elements)