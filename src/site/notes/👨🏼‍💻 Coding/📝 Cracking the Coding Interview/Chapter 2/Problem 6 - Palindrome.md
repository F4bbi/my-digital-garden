---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-2/problem-6-palindrome/","created":"2023-08-10T11:43:45.254+02:00","updated":"2023-08-10T11:43:45.254+02:00"}
---

# Chapter 2 - Problem 6 - Palindrome
## Problem
Implement a function to check if a linked list is a palindrome.

#### Solution in C++ using Reverse and Compare
The solution is to reverse the linked list and compare the reversed list to the original list. If they're the same, the lists are identical.
Note that when we compare the linked list to the reversed list, we only actually need to compare the first half of the list. If the first half of the normal list matches the first half of the reversed list, then the second half of the normal list must match the second half of the reversed list.
```cpp
template<typename T>
class Node {
    public:
        T data;
        Node* next;
        
        Node() = default;
        Node(const T& value) : data(value), next(NULL) {}
};

template<typename T>
class LinkedList {
    protected:
        Node<T>* head;
};

template<typename T>
class Solution : public LinkedList<T> {
    private:
        bool isEqual(Node<T>* l1, Node<T>* l2) {
            while(l1 != NULL) {
                if(l1->data != l2->data)
                    return false;
                l1 = l1->next;
                l2 = l2->next;
            }
            return true;
        }

    public:
        bool isPalindrome() {
            if(this->head == NULL) return false;
            Node<T>* reversed = NULL, *slowRunner = this->head, *fastRunner = this->head;
            
            /* find middle of list */
            while (fastRunner != NULL && fastRunner->next != NULL) {
                slowRunner = slowRunner->next;
                fastRunner = fastRunner->next->next;
            }

            /* reverse second half of list */
            while (slowRunner != NULL) {
                Node<T>* next = slowRunner->next;
                slowRunner->next = reversed;
                reversed = slowRunner;
                slowRunner = next;
            }

            /* we don't have to care if the list has odd number of elements */
            return isEqual(reversed, this->head);
        }
}
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(1)$

> [!info] **How did we found the middle of the list?** 
> Since we don't know the size of the linked list, we can iterate over the linked list, using the fast runner/ slow runner technique. At each step in the loop, the slow runner is moving at 1x speed, while the fast runner is moving at 2x.  
> When the fast runner hits the end of the list, the slow runner will have reached the middle of the linked list.

We can also do the same but reversing the first half of the list. The function would be similar.
```cpp
bool isPalindrome() {
    if(this->head == NULL) return false;
    /* Reversing and cloning the linked list */
    Node<T>* prev = NULL, *slowRunner = this->head, *fastRunner = this->head;
    while(fastRunner != NULL && fastRunner->next != NULL) {
        Node<T> *reversedList = new Node(slowRunner->data);
        reversedList->next = prev;
        prev = reversedList;
        slowRunner = slowRunner->next;
        fastRunner = fastRunner->next->next;
    }
    /* The list has odd number of elements, so skip the middle element */
    if(fastRunner != NULL)
        slowRunner = slowRunner->next;
    return isEqual(prev, slowRunner);
}
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(1)$

#### Solution in C++ using Stack
The approach is the same as before but we avoid the part of reversing the first half of the list and we simply push it in a stack.

```cpp
template<typename T>
class Node {
    public:
        T data;
        Node* next;
        
        Node() = default;
        Node(const T& value) : data(value), next(NULL) {}
};

template<typename T>
class LinkedList {
    protected:
        Node<T>* head;
};

template<typename T>
class Solution : public LinkedList<T> {
    private:
        bool isEqual(Node<T>* l1, Node<T>* l2) {
            while(l1 != NULL) {
                if(l1->data != l2->data)
                    return false;
                l1 = l1->next;
                l2 = l2->next;
            }
            return true;
        }

    public:
        bool isPalindrome() {
            if(this->head == NULL) return false;
            
            Node<T> *slowRunner = this->head, *fastRunner = this->head;
            std::stack<T> stack; 

            /* find middle of list and push the first half in stack */
            while (fastRunner != NULL && fastRunner->next != NULL) {
                stack.push(slowRunner->data);
                slowRunner = slowRunner->next;
                fastRunner = fastRunner->next->next;
            }

            /* The list has odd number of elements, so skip the middle element */
            if(fastRunner != NULL)
                slowRunner = slowRunner->next;

            for(; slowRunner != NULL; slowRunner = slowRunner->next) {
                int top = stack.top();
                stack.pop();
                if(top != slowRunner->data)
                    return false;
            }
            return true;
        }
}
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(\frac N 2)$
