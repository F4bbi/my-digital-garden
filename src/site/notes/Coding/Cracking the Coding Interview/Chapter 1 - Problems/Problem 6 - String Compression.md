---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1-problems/problem-6-string-compression/"}
---

# Chapter 1 - Problem 6 - String Compression
## Problem
Implement a method to perform basic string compression using the counts of repeated characters. For example, the string _aabcccccaaa_ would become _a2b1c5a3_. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

## Solution
We iterate through the string, copying characters to a new string and counting the repeats. 
At each iteration, check if the current character is the same as the next character. If not, add its compressed version to the result.
#### Solution in C++ 
```cpp
std::string stringCompression(std::string string) {
    std::string compressedString = "";
    int charCounter = 0;
    for(int i = 0; i < string.length(); i++) {
        charCounter++;

        /* If next character is different than current, append this char to result. */
        if(i + 1 >= string.length() || string.at(i) != string.at(i+1)) {
            /* First condition it's because at last iteration we would go out of bounds. */
            compressedString += string.at(i) + std::to_string(charCounter);
            charCounter = 0;
        }
    }
    return (compressedString.length() >= string.length()) ? string : compressedString;
}
```
- **Time complexity:** $O(s + c)$ (where _s_ is the size of the original string and _c_ is the number of 
 character sequences)
- **Space complexity:** $O(c)$ (where _c_ is the number of character sequences)

>[!warning] Be careful!
>In C++ strings are mutable, so the performance considerations of strings concatenation are less of a concern.
>However, in other languages like Java, strings are immutable so using the + operator will create a new instance of the string on each iteration. In this case, the time complexity would be $O(s + c^2)$. 
>For more details see [[Coding/Data Structures/StringBuilder\|StringBuilder]].

Last thing, we could optimize a bit the code above checking the condition in the return at the beginning of the function, avoiding us to create a string that we will never use. However, this approach would cause a second loop through the characters and also would add nearly duplicated code. For this reason, i will leave the function as it is.