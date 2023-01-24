---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-1-problems/problem-4-palindrome-permutation/"}
---

# Chapter 1 - Problem 4 - Palindrome Permutation
## Problem
Given a string, write a function to check if it is a permutation of a palindrome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. 
The palindrome does not need to be limited to just dictionary words.

**EXAMPLE**
_Input_: Tact Coa
_Output_: true (permutations: "taco cat", "atco cta", etc.)

## Solution
We assume the string contains lowercase characters (others will be discarded).
We don't have to check the palindrome property on every permutation!
To be a permutation of a palindrome, a string can't have more than one character that is odd (the middle one).

#### Solution in C++ with array
We simply count the frequency of each character and after we check that the string has no more than one character that is odd.
```cpp
int getIndex(char c)
{
  if (c >= 'a' && c <= 'z')
    return (int)(c - 'a');
  return -1;
}

std::array<int, 26> getCharFrequencyTable(std::string string) {
    std::array<int, 26> frequencyTable = {};
    for(char c : string) {
        int index = getIndex(c);
        if(index != -1)
            frequencyTable.at(index)++;
    }
    return frequencyTable;
}

bool palindromePermutation(std::string string) {
    std::array<int, 26> frequencyTable = getCharFrequencyTable(string);
    bool isOddNumber = false;
    for(int number : frequencyTable) {
        if((number % 2) != 0) {
            if(isOddNumber)
                return false;
            else
                isOddNumber = true;
        }
    }
    return true;
}
```
- **Time complexity:** $O(N)$ (one pass over the `string`, one pass for the `frequency table`)
- **Space complexity:** $O(1)$ (`frequencyTable` will always have 26 elements)

#### Solution in C++ with array with small optimization
We don't have to wait the end of the word to count the odd numbers, we can keep track of them while we read the string. (This is not necessarily more optimal, we have eliminated a final iteration through the array, but now we have to run a few extra lines of code for each character in the string)
```cpp
int getIndex(char c)
{
  if (c >= 'a' && c <= 'z')
    return (int)(c - 'a');
  return -1;
}

bool palindromePermutation(std::string string) {
    std::array<int, 26> frequencyTable = {};
    int countOdd = 0;
    for(char c : string) {
        int index = getIndex(c);
        if(index != -1) {
            frequencyTable.at(index)++;
            frequencyTable.at(index) % 2 != 0 ? countOdd++ : countOdd--;
        }
    }
    return countOdd <= 1;
}
```
- **Time complexity:** $O(N)$ (one pass over the `string`)
- **Space complexity:** $O(1)$ (`frequencyTable` will always have 26 elements)

#### Solution in C++ without data structures
Since we just have to know if the string has either 0 or 1 character that is odd, we can use use a single integer (as a bit vector), where we map a letter to an integer between 0 and 26 (similarly to [[Coding/Cracking the Coding Interview/Chapter 1 - Problems/Problem 1 - IsUnique#Solution in c without data structures\|Problem 1 - IsUnique]]). 
If the frequency of the character is even, the corresponding bit will be 0, if odd it'll be 1.

```cpp
int getIndex(char c)
{
  if (c >= 'a' && c <= 'z')
    return (int)(c - 'a');
  return -1;
}

bool palindromePermutation(std::string string) {
    int bitVector = 0;

    for(char c : string) {
        int index = getIndex(c);
        if(index != -1) {
            bitVector ^= 1 << index; //logic xor, frequency of the character even -> 0
                                                //frequency of the character odd -> 1   
        }
    }
    return (bitVector & (bitVector - 1)) == 0; //bit manipulation
}
```
- **Time complexity:** $O(N)$ (one pass over the `string`)
- **Space complexity:** $O(1)$