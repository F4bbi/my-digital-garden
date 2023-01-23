---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1-problems/problem-5-one-away/"}
---

# Chapter 1 - Problem 5 - One Away
## Problem
There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.

**EXAMPLE**
_Input_: `s1`: "pale", `s2`: "ple"
_Output_: true

_Input_: `s1`: "pale", `s2`: "bae"
_Output_: false

## Solution
We can discern insertion, removal and replacement edits based on the lengths of the strings.
We have to check that no more that one character is different between the two strings.
#### Solution in c++ 
We may merge `characterReplaced` and `characterRemoved` into one method since it would be more compact and without duplicated code but i prefer this approach, as it is clearer and easier to follow.
```cpp
bool characterReplaced(std::string s1, std::string s2) {
    bool foundDifference = false;
    for(int i = 0; i < s1.length(); i++) {
        if(s1.at(i) != s2.at(i)) {
            if(foundDifference) //if we found a different character for the second time
                return false;
            else foundDifference = true;
        }
    }
    return true;
}

/*We pass over the two strings simultaneously, if we find a different character 
it is the one inserted, but if we find it another time the string has been edited 2 times.*/
bool characterRemoved(std::string s1, std::string s2) {
    int s1_index = 0, s2_index = 0;

    while(s1_index < s1.length() && s2_index < s2.length()) {
        if(s1.at(s1_index) != s2.at(s2_index)) {
            if (s1_index != s2_index) //if the indices are different, it's the second time
                return false;        // we found a different character
        }
        else
            s2_index++;
        
        s1_index++;
    }
    return true;
}

//insert and remove are the same, we just have to swap the strings
bool characterInserted(std::string s1, std::string s2) {
    return characterRemoved(s2, s1);  
}

bool oneAway(std::string s1, std::string s2) {
    int s1_length = s1.length();        
    int s2_length = s2.length();
    if(s1_length == s2_length) return characterReplaced(s1, s2);        
    else if(s1_length - 1 == s2_length) return characterRemoved(s1, s2);        
    else if(s1_length + 1 == s2_length) return characterInserted(s1, s2);
    return false; //two or more edits        
}
```
- **Time complexity:** $O(N)$ (we pass over the two strings simultaneously)
- **Space complexity:** $O(1)$