---
{"dg-publish":true,"permalink":"/coding/data-structures/hash-tables/"}
---

# Hash Tables
A hash table is a data structure that maps keys to values for highly efficient lookup, in fact we can store and retrieve objects in constant time $O(1)$ (on average[^1]), provided we know the key.
In a simple implementation, we use an array of linked lists and a hash code function.

[^1]: If the hash code function is very bad, all keys' hash code will point to the same index in the array and we will have to travel the whole list to get the desired element. 