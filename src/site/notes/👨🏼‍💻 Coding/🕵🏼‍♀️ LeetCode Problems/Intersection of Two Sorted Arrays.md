---
{"dg-publish":true,"permalink":"/coding/leet-code-problems/intersection-of-two-sorted-arrays/","created":"2023-01-26T00:34:05.277+01:00","updated":"2023-01-26T00:34:05.277+01:00"}
---

# Intersection of Two Sorted Arrays
## Problem
Given two sorted arrays, find the number of elements in common. The arrays are the same length and each has all distinct elements.

## Solution
Let's start with a good example. We'll underline the elements in common.

A: $13$ $27$ $\underline{35}$ $\underline{40}$ $49$ $\underline{55}$ $59$
B: $17$ $\underline{35}$ $39$ $\underline{40}$ $\underline{55}$ $58$ $60$

### Brute Force Algorithm 
Start with each element in A and search for it in B.
- **Time complexity:** $O(N^2)$
- **Space complexity:** $O(N)$

#### Solution in C++
```cpp
vector<int> intersect(vector<int> A, vector<int> B){
    vector<int> result;
    for(int i = 0; i < A.size(); i++)
        for(int j = 0; j < B.size(); j++)
            if(A[i] == B[j])
                result.push_back(A[i]);
    return result;
}
```

### BCR
We have to look at each element at least once and there are 2N total elements. $\rightarrow \ O(N)$ 

We can improve our algorithm using binary search in B. $\rightarrow \ O(N \log N)$
Or we can precompute what's in B. In fact **any work you do that's less than or equal to the BCR is "free", it won't impact your runtime**.  So any precomputation that's $O(N)$ or less is "free". 

### Near-Optimal Algorithm
In this case, we can just throw everything in B into a [[👨🏼‍💻 Coding/🏗 Data Structures/Hash Tables\|hash table]].
- **Time complexity:** $O(N)$
- **Space complexity:** $O(N)$

At this point we **can't** do better in terms of [[👨🏼‍💻 Coding/📝 Cracking the Coding Interview/Concepts/1.1 Big O#⏱️ Time complexity\|runtime]], but we can optimize the [[👨🏼‍💻 Coding/📝 Cracking the Coding Interview/Concepts/1.1 Big O#💾 Space Complexity\|space complexity]].
In fact, we would have achieved the exact same runtime if the data wasn't sorted.

#### Solution in C++
```cpp
vector<int> intersect(vector<int>& A, vector<int>& B){
    vector<int> result;
    unordered_map<int, int> hash;
    
    for(int i = 0; i < B.size(); i++)
        hash[B[i]] = 1;

    for(int i = 0; i < A.size(); i++)
        if (hash.find(A[i]) != hash.end())
            result.push_back(A[i]);
            
    return result;
}
```

### Optimal Algorithm
- **Time complexity:** $O(N)$
- **Space complexity:** $O(1)$

Since the two arrays are sorted, we just do linear search but we start where the last one left off.

#### Solution in C++
```cpp
vector<int> intersect(vector<int>& A, vector<int>& B){
    vector<int> result;
    int j = 0;
    for(int i = 0; i < A.size(); i++) {
        for(; j < B.size(); j++) {
            if(A[i] < B[j])
                break;
            else if(A[i] == B[j])
                result.push_back(A[i]);
        }
    }       
    return result;
}
```