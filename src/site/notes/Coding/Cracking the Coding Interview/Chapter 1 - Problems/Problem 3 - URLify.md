---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1-problems/problem-3-ur-lify/"}
---

# Chapter 1 - Problem 3 - URLify
## Problem
Write a method to replace all spaces in a string with '\%20'. You may assume that the string
has sufficient space at the end to hold the additional characters, and that you are given the "true"
length of the string.

## Solution
We edit the string starting from the end and working backwards. This is useful because we have an extra buffer at the end, which allows us to change characters without worrying about what we're overwriting.

#### Solution in c++
```cpp
int getNumberSpaces(const std::string & string, int length) {
    int numberSpaces = 0;
    for(int i = 0; i < length; i++)
        if(string[i] == ' ') numberSpaces++;
    return numberSpaces;
}

void URLify(std::string & string, int trueLength) {
    int newIndex = trueLength + getNumberSpaces(string, trueLength) * 2 - 1;
    
    if(newIndex + 1 == trueLength)
        return;

    for(int oldIndex = trueLength - 1; oldIndex >= 0; oldIndex--) {
        if(string.at(oldIndex) != ' ')
            string.at(newIndex) = string.at(oldIndex);
        else {
            string.at(newIndex) = '0';
            string.at(--newIndex) = '2';
            string.at(--newIndex) = '%';
        }
        newIndex--;
    }
}
```
- **Time complexity:** $O(N)$ (one pass over the string)
- **Space complexity:** $O(1)$ 