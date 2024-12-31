---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-4/problem-2-minimal-tree/","created":"2023-01-30T22:00:44.880+01:00","updated":"2023-01-30T22:00:44.880+01:00"}
---

# Chapter 4 - Problem 2 - Minimal Tree
## Problem
Given a sorted (increasing order) array with unique integer elements, write an algorithm to create a binary search tree with minimal height.

#### Solution in C++
To create a tree of minimal height, we need to match the number of nodes in the left subtree to the number
of nodes in the right subtree as much as possible. This means that we want the root to be the middle of the
array, since this would mean that half the elements would be less than the root and half would be greater
than it. So, the middle of each subsection of the array becomes the root of the node. The left half of the array will become our left subtree, and the right half of the array will become the right subtree.

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
class BinarySearchTree {
    protected:
        Node<T>* root;
    public:
        BinaryTree() : root(NULL) {}
};

template<typename T>  
class Solution : public BinarySearchTree<T> {
    private:
        Node<T>* minimalTreeHelper(vector<T> array, int start, int end) {
            if(start > end) return NULL;
            int mid = (start + end) / 2;
            Node<T>* node = new Node(array.at(mid));
            node->left = minimalTreeHelper(array, start, mid - 1);
            node->right = minimalTreeHelper(array, mid + 1, end);
            return node;
        }
    public:
        void minimalTree(vector<T> array) {
            this->root = minimalTreeHelper(array, 0, array.size() - 1);
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the array)
- **Space complexity:** $O(1)$