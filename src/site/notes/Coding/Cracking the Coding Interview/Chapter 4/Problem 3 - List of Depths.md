---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-4/problem-3-list-of-depths/"}
---

# Chapter 4 - Problem 3 - List of Depths
## Problem
Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists).

#### Solution in C++
To solve this problem we can traverse the graph any way that we'd like, provided we know which level we're on as we do so.
##### Solution using BFS
The idea is to keep track of the number of nodes in every level. Every time the counter goes to zero, the level is finished and we can push the list in the vector. When the list is empty, we will have traversed all the tree.
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
    public:
        vector<std::list<Node<T>*>> listOfDepths() {           
            list<Node<T>* > list;
            vector<std::list<Node<T>*>> result;
            /* The number of nodes in every level*/
            int count = 0;

            /* Base case */
            if (this->root == NULL)
                return result;

            /* Enqueue root */
            list.push_back(this->root);

            while(!list.empty()) {
                if(count == 0) {
                    result.push_back(list);
                    count = list.size();
                }
        
                Node<T>* node = list.front();
                list.pop_front();

                /* Enqueue left child */
                if(node->left != NULL)
                    list.push_back(node->left);

                /* Enqueue right child */
                if(node->right != NULL)
                        list.push_back(node->right);
                
                --count;
            }
            return result;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the binary tree)
- **Space complexity:** $O(1)$

##### Solution using DFS
We can implement a simple modification of the pre-order traversal algorithm, where we pass in `level + 1` to the next recursive call.
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
	public:
        vector<std::list<Node<T>*>> listOfDepths() {
            vector<list<Node<int>*>> result;
            preOrder(result, this->root, 0);
            return result;
        }
    private:
        void preOrder(vector<std::list<Node<T>*>>& result, Node<T>*& node, unsigned int level) {
            if(node == NULL) return;

            if(result.size() == level) {
                /* Levels are always traversed in order. So, if this is the first time we've
                 * visited level i, we must have seen levels 0 through i - 1. We can
                 * therefore safely add the level at the end */
                result.push_back(list<Node<T>*>());
            } 
            result.at(level).push_back(node);
            preOrder(result, node->left, level + 1);
            preOrder(result, node->right, level + 1);
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the binary tree)
- **Space complexity:** $O(N)$ ($O(\log N)$ if the tree is balanced)

>[!info] A comment on space complexity
> One might ask which of these solutions is more efficient. Both run in $O(N)$ time, but what about the space
efficiency? At first, we might want to claim that the second solution is more space efficient.
In a sense, that's correct. The first solution uses $O(\log N)$ recursive calls (in a balanced tree), each of which
adds a new level to the stack. The second solution, which is iterative, does not require this extra space.
However, both solutions require returning $O(N)$ data. The extra $O(\log N)$ space usage from the recursive
implementation is dwarfed by the $O(N)$ data that must be returned. So while the first solution may actually
use more data, they are equally efficient when it comes to "big O".