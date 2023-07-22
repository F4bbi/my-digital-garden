---
{"dg-publish":true,"permalink":"/coding/leet-code-problems/maximum-submatrix/","created":"2022-09-22T16:39:12.686+02:00","updated":"2023-07-22T15:12:52.339+02:00"}
---

# Maximum Submatrix
## Problem
Given an integer matrix, find the contiguous submatrix (containing at least one number) which has the largest sum and return _its sum_.

## Solution
### Brute Force Algorithm
You can see a good explanation [here](https://youtu.be/-FgseNO-6Gk?t=98)
#### Solution in C++
```cpp
int maximumSubmatrix(const vector<vector <int> > & matrix, const int N, const int M) {
    int maxSoFar = matrix[0][0], sum;
    for(int i = 0; i < N; ++i) {
        for(int j = 0; j < M; ++j) {
            for(int k = i; k < N; ++k) {
                for(int l = j; l < M; ++l) {
                    sum = 0;
                    /* Sum all the elements in the submatrix */
                    for(int o = i; o <= k; ++o) {
                        for(int p = j; p <= l; ++p) {
                            sum = sum + matrix[o][p];
                        }
                    }
                    /* Check if current sum it's higher */
                    maxSoFar = max(maxSoFar, sum);
                }
            }
        }
    }
    return maxSoFar;
}
```
- **Time complexity:** $O(N^3*M^3)$ (where _N_ are the rows and _M_ the columns)
- **Space complexity:** $O(1)$

### Optimal Algorithm
You can see a good explanation [here](https://youtu.be/-FgseNO-6Gk?t=583)
To understand this algorithm you should see [[👨🏼‍💻 Coding/🕵🏼‍♀️ LeetCode Problems/Maximum Subarray\|Maximum Subarray]] first.
#### Solution in C++ 
```cpp
int maximumSubmatrix(const vector<vector <int> > & matrix, const int N, const int M) {
    vector<int> auxArray (N, 0);
    int maxHere, maxSoFar;
    maxHere = maxSoFar = matrix[0][0];
    for (int i = 0; i < M; ++i) {
        for (int j = i; j < M; ++j) {
            for(int k = 0; k < N; ++k) {
                auxArray.at(k) += matrix[k][j];
            }
            /* Application of Kadane's algorithm */
            maxHere = auxArray.at(0);
            for(int k = 1; k < N; ++k) {
                maxHere = max(auxArray.at(k), maxHere + auxArray.at(k));
                maxSoFar = max(maxSoFar, maxHere);
            }
        }
        /* Clear the array */
        fill(auxArray.begin(), auxArray.end(), 0);
    }
    return maxSoFar;    
}
```
- **Time complexity:** $O(N*M^2)$ (where _N_ are the rows and _M_ the columns)
- **Space complexity:** $O(N)$