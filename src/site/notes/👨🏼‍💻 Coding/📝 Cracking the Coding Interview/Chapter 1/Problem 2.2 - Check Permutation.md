---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1/problem-2-2-check-permutation/","created":"2022-08-04T17:01:51.885+02:00","updated":"2023-01-24T11:51:14.968+01:00"}
---

# Chapter 1 - Problem 2.2 - Check Permutation
## Problem
Given a smaller string `s1` and a bigger string `s2`, design an algorithm to find all permutations of the shorter string within the longer one. Return an array of all the start indices of  `s1`'s anagrams in `s2`.
#leetcode/438 #leetcode/567 
## Solution
The assumptions are the same of [[👨🏼‍💻 Coding/📝 Cracking the Coding Interview/Chapter 1/Problem 2.1 - Check Permutation\|Problem 2.1 - Check Permutation]].

#### Solution in C++ with array
This problem is similar to [[👨🏼‍💻 Coding/📝 Cracking the Coding Interview/Chapter 1/Problem 2.1 - Check Permutation\|Problem 2.1 - Check Permutation]], but here we have to find all the permutation occurences. To do that, we start sliding the window `[0..len(s1)]` over `s2`. Every time a letter gets out of the window, we decrease the corresponding counter in the array and when a letter gets in the window, we increase it. Now we just have to compare `s1` with the `sliding window`.
```cpp
std::vector<int> checkPermutation(std::string s1, std::string s2) {
    std::vector<int> answer;
    int s1_size = s1.size();
    int s2_size = s2.size();
    
    if(s1_size > s2_size) return {};

    std::array<int, 128> s1_charSet = {};
    std::array<int, 128> s2_charSet = {};

    //initialize s1 and s2 hash maps
    for(int i = 0; i < s1_size; i++) {
        s1_charSet.at(s1.at(i))++;
        s2_charSet.at(s2.at(i))++;
    }

    //first check
    if(s1_charSet == s2_charSet) answer.push_back(0);

    //it's a sliding window, each iteration the window slides at right 
    for(int i = 0; (s1_size + i) < s2_size; i++) { 
        int right_index = (int)s2.at(s1_size + i); //character to add in hash map
        int left_index = (int)s2.at(i); //character to delete in hash map
        s2_charSet.at(right_index)++;
        s2_charSet.at(left_index)--;
        if(s1_charSet == s2_charSet) answer.push_back(i+1);
    }
    return answer;
}
```
- **Time complexity:** $O(N)$ (one pass over `s1`, one pass for `s2`)
- **Space complexity:** $O(1)$ (`char_set` will always have 128 elements)

[^1]: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html