---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1/problem-1-is-unique/","created":"2023-01-24T11:50:52.904+01:00","updated":"2023-01-24T11:50:52.904+01:00"}
---

# Chapter 1 - Problem 1 - IsUnique
## Problem
Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

## Solution
First we have to decide if the string is an ASCII string or a Unicode string. We'II assume for simplicity the character set is ASCII.
>[!warning] Be careful!
> In C++, if you want to include extended ASCII strings or Unicode strings as well, simply increasing the storage size won't be enough. Remember that `char` in C++ ranges up to 127, so if you want to use extended ASCII strings you will have to use `unsigned char`, which goes from 0 - 255. Anyway, keep in mind that interpreting "higher" ASCII character values depends on your terminal, so it's possible you will see strange simbols if you try to print them. 
> This doesn't apply to Java, for example, where the `char` data type is a single 16-bit Unicode character[^1].

#### Solution in C++ with array
One solution is to create an array of boolean values, where the flag at index $i$ indicates whether character $i$ in the alphabet is contained in the string. The second time you see this character you can immediately return false.
```cpp
bool isUnique(std::string string) {  
    std::array<bool, 128> char_set = {};  
    for(char c : string) {  
        int index = int(c);  
        if(char_set.at(index) == true)  
            return false;  
        else  
            char_set.at(index) = true;  
    }  
    return true;  
}
```
- **Time complexity:** $O(N)$ (although it will be $O(1)$ on average)
- **Space complexity:** $O(1)$ (`char_set` will always have 128 elements)

#### Solution in C++ without data structures
We can reduce our space using an integer as a bit vector. However, the string will have to contain only 32 different characters, in our case the lowercase letters a through z.

```cpp
bool isUnique(std::string string) {  
    int bitVector = 0; //we use bitVector as an array, using and and or operators  
    for(char c : string) {  
        int character = (int)(c - 'a'); //a becomes 0, b becomes 1 and so on  
        if((bitVector & (1 << character)) > 0) //logic and between bitVector and
										        //left shift   
            return false;  
        else  
            bitVector |= (1 << character); //we add 1 one the relative position  
    }  
    return true;  
}
```
- **Time complexity:** $O(N)$ (although it will be $O(1)$ on average)
- **Space complexity:** $O(1)$ (`bitVector` has always the same size)

#### Other Solutions 
- Compare every character of the string to every other character of the string. This will take $O (N^2)$ time.
- Sort the string in $O(N \log(N))$ time and then linearly check the string for neighboring characters that are identical.


[^1]: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html