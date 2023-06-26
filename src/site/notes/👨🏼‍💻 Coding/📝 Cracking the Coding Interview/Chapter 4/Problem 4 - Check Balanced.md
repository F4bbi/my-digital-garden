---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-4/problem-4-check-balanced/","created":"2023-02-20T15:41:34.513+01:00","updated":"2023-02-21T09:51:37.981+01:00"}
---

# Chapter 4 - Problem 4 - Check Balanced
## Problem
Implement a function to check if a binary tree is balanced. For the purposes of  this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differ by more than one.

#### Solution in C++ using BFS
The idea is to check the height of each subtree as we recurse down from the root.
On each node, we recursively get the heights of the left and right subtrees through the `checkHeight()` method. If the subtree is balanced, then checkHeight will return the actual height of the subtree. If the subtree is not balanced, then checkHeight will return an error code. We will immediately break and return an error code from the current call.

```cpp
template<typename T>
class Node {
        public:
            T key;
            Node* left;
            Node* right;
            
            Node() = default;
            Node(const T& value) : key(value), left(NULL), right(NULL) {}
};

template<typename T>    
class BinaryTree {
    protected:
        Node<T>* root;
    public:
        BinaryTree() : root(NULL) {}
};

template<typename T>    
class Solution : public BinaryTree<T> {
    private:
        int checkHeight(Node<T>*& root) {
            if(root == NULL)
                return 0;
            
            int left = checkHeight(root->left);
            int right = checkHeight(root->right);

            if(abs(left - right) > 1 || left == -1 || right == -1)
                return -1;  // Error found -> pass it back
            else
                return 1 + max(left, right);
        }
    public:
        bool isBalanced() {
            if(this->root == NULL)
                return false;
            else
                return checkHeight(this->root) != -1;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the binary tree)
- **Space complexity:** $O(H)$ (where _H_ is the height of the binary tree)