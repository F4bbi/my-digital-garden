---
{"dg-publish":true,"permalink":"/coding/data-structures/c-std-array/"}
---

# C++ std::array
Modern alternative to C-style arrays, it's available from C++11. 
It's a simple aggregate containing an array as its only member.

## Some random things
- When you want to initialize an array
	- ❌ `std::array<int, 10> array;`  $\rightarrow$ there will be garbage inside
	- ✅ `std::array<int, 10> array = {};` $\rightarrow$ every element will be 0