---
{"dg-publish":true,"permalink":"/coding/companies-problems/alice-and-bob-s-game/","created":"2023-09-14T22:57:40.621+02:00","updated":"2023-09-14T22:57:40.621+02:00"}
---

# Alice and Bob's Game
## Problem
The input is an array of positive numbers.
**Rules of the Game:**
- A player can choose a pair of similar consecutive characters and erase them.
- There are two players playing the game, the player who makes the last move wins.

The task is to find the winner if Alice goes first and both play optimally.
## Solution
We can use a stack to simplify the problem. Each time we encounter a character that is different from the one present in the top of the stack we add it to the stack. If the stack top and the next character match, we pop the character from the stack and increment the count.
At the end, we just need to see who wins by checking `count % 2`.

Below is the implementation of the above approach.

#### Solution in C++ using stack
```cpp
string findWinner(const vector<int>& nums) {
    int counter = 0;
    stack<int> stack;

    for(int num : nums) {
        if(!stack.empty() && stack.top() == num) {
            stack.pop();
            counter++;
        }
        else
            stack.push(num);
    }

    //if counter % 2 == 1 Alice has found a pair, otherwise Bob did
    return counter % 2 == 0 ? "Bob" : "Alice";
}
```
- **Time complexity:** $O(n)$ (where _n_ is the size of the vector)
- **Space complexity:** $O(n)$