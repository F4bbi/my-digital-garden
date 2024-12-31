---
{"dg-publish":true,"permalink":"/coding/companies-problems/minimum-deletion/","created":"2023-10-29T11:44:59.502+01:00","updated":"2023-10-29T11:44:59.502+01:00"}
---

# Minimum Deletion
## Problem
You are given a sequence $A$ of length $N$ consisting only positive integers $(A_1, A_2, ..., A_N)$. You have to delete the minimum number of elements from the sequence $A$ to make it a good sequence.

A sequence $B$ is said to be good if the occurrence of each element $x$ of the sequence $B$ is divisible of a common positive number $v$ ($v > 1$).

For example, (1, 1, 2, 2, 2, 2, 4, 4) is a good sequence because the occurrence of each element is divisible by 2. While sequence (1, 1, 3, 3, 3) is not a good sequence because there are no common positive integers that divide the occurrence of each element.

**Input:** First-line consists of an integer $N$ denoting the size of the sequence $A$. Second-line will contain $N$ space-separated integers $(A_1, A_2, ..., A_N)$.

**Output:** The value denoting the minimum number of elements that we have to remove to make the sequence $A$ a good sequence.

**Constraints:**
$1 \leq T \leq 10$
$1 \leq N \leq 10^3$
$1 \leq A_i \leq 10^9$

### Example 1)
```bash
Input:
5
1 1 3 3 3
Output:
1
```

### Example 2)
```bash
Input:
9
6 1 1 7 6 3 6 6 5
Output:
3
```

## Solution
#### Solution in C++ using mod with increasing numbers
First, we have to calculate the occurrences for each element of the input, using a hashmap. Then, we store in _max\_frequence_ the value with the highest frequency. Now, foreach number $\leq$ max\_frequence, we compute the number of deletions we would have using that number as GCD. Finally, we return the round in which we made the fewest deletions.

```cpp
int minimumDeletion(vector<int>& array) {
    unordered_map<int, int> map;
    int max_frequence = 0;
    int result = INT_MAX;
    for(int val : array)
        map[val]++;
    //get the value with the highest frequency in the map
    max_frequence =  max_element(
        map.begin(),
        map.end(),
        [](const pair<int, int>& p1, const pair<int, int>& p2) {
            return p1.second < p2.second;
        }
    )->second;

    for(int i = 2; i <= max_frequence; i++) {
        int num_deletions = 0;
        for(auto val : map) {
            num_deletions += val.second % i;
        }
        result = min(result, num_deletions);
    }

    return result;
}
```
- **Time complexity:** $O(N \cdot K)$ (where $K$ is the value with the highest frequency in the array)
- **Space complexity:** $O(N)$ (the hashmap)

#### Solution in C++ using mod with prime numbers
The solution is the same as above, but if you are smart enough you can note that there is no need to cycle for all numbers $\leq$ max_frequence. We can simply use all the prime numbers $\leq$ max_frequence.

```cpp
//Since max occurence is <= 1000 we can use a fixed array of prime numbers
const vector<int> primeNumbers = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997 };

int minimumDeletion(vector<int>& array) {
    unordered_map<int, int> map;
    int max_frequence = 0;
    int result = INT_MAX;
    for(int val : array)
        map[val]++;
    //get the value with the highest frequency in the map
    max_frequence =  max_element(
                                    map.begin(),
                                    map.end(),
                                    [](const pair<int, int>& p1, const pair<int, int>& p2) {
                                        return p1.second < p2.second;
                                    }
                                )->second;

    for(int i = 0; primeNumbers.at(i) <= max_frequence; i++) {
        int num_deletions = 0;
        for(auto val : map) {
            num_deletions += val.second % primeNumbers.at(i);
        }
        result = std::min(result, num_deletions);
    }

    return result;
}
```
- **Time complexity:** $O(N \cdot \frac {K} {\log K})$ (where $K$ is the value with the highest frequency in the array)
- **Space complexity:** $O(N + K)$ (hashmap + fixed array of prime numbers)

Using a fixed array with all the prime number $\leq 1000$ lowers the time complexity to $O(N \cdot \frac {K} {\log K})$. Alternatively, we can obtain all the prime numbers $\leq\ K$ using the [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) algorithm, leading the time complexity to $O(N \cdot \frac {K} {\log K} + K \log \log K)$.