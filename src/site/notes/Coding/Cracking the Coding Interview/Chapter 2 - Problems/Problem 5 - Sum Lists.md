---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-2-problems/problem-5-sum-lists/"}
---

# Chapter 2 - Problem 5 - Sum Lists
## Problem
You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1 's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

**EXAMPLE**
**First part**
_Input:_ (7-> 1 -> 6) + (5 -> 9 -> 2). That is, 617 + 295.
_Output:_ 2 - > 1 - > 9. That is, 912.
**Second part**
Suppose the digits are stored in forward order. Repeat the above problem.
_Input:_ (6 -> 1 -> 7) + (2 -> 9 -> 5). That is, 617 + 295.
_Output:_ 9 - > 1 -> 2. That is, 912.
		  
## Solution
### First part
#### Iterative solution in C++
```cpp
template<typename T>
class Node {
    public:
        Node* next;
        T data;

        Node();
        Node(T value);
};

template<typename T>
class LinkedList {
    protected:
        Node<T>* head;
    public:
	    LinkedList();
        ~LinkedList();
};

template<typename T>
class ImprovedLinkedList : public LinkedList<T> {
    public:
        void sumLists_iterative(const ImprovedLinkedList<T>& l1, 
						        const ImprovedLinkedList<T>& l2) {
            int carry = 0;
            Node<T>* l1Head = l1.head;
            Node<T>* l2Head = l2.head;
            Node<T>* current = this->head = new Node<T>();
            
			/* || carry because if l1 and l2 are the same size, like 9 + 1 */
            while(l1Head != NULL || l2Head != NULL || carry) { 
                int sum = (l1Head ? l1Head->data : 0) + (l2Head ? l2Head->data : 0) + carry;
                current->next = new Node<T>(sum % 10);
                carry = sum / 10;

                if(l1Head) l1Head = l1Head->next;
                if(l2Head) l2Head = l2Head->next;
                current = current->next;
            }

            Node<T> *tmp = this->head;
            this->head = this->head->next;
            delete tmp;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(1)$

#### Recursive solution in C++
```cpp
template<typename T>
class Node {
    public:
        Node* next;
        T data;

        Node();
        Node(T value);
};

template<typename T>
class LinkedList {
    protected:
        Node<T>* head;
    public:
	    LinkedList();
        ~LinkedList();
};

template<typename T>
class ImprovedLinkedList : public LinkedList<T> {
    public:
        void sumLists_recursive(const ImprovedLinkedList<T>& l1, const ImprovedLinkedList<T>& l2) {
            Node<T>* l1Head = l1.head;
            Node<T>* l2Head = l2.head;
            sumLists_recursive_aux(l1Head, l2Head, this->head, 0);
        }
    private:
        void sumLists_recursive_aux(Node<T>*& l1, Node<T>*& l2, Node<T>*& current, int carry) {
            if(l1 == NULL && l2 == NULL && carry == 0)
                return;

            int sum = (l1 ? l1->data : 0) + (l2 ? l2->data : 0) + carry;
            current = new Node<T>(sum % 10);
            if(l1) l1 = l1->next;
            if(l2) l2 = l2->next;
            sumLists_recursive_aux(l1, l2, current->next, sum / 10);
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(N)$

### Second part
#### Recursive solution in C++
In this approach, we directly go at the end of the list so we can do the sum backwards. `carry` is passed by reference so it can change in every iteration.
```cpp
template<typename T>
class Node {
    public:
        Node* next;
        T data;

        Node();
        Node(T value);
};

template<typename T>
class LinkedList {
    protected:
        Node<T>* head;
    public:
	    LinkedList();
        ~LinkedList();
        void insertFirst(T value);
};

template<typename T>
class ImprovedLinkedList : public LinkedList<T> {
    public:
        void reversedSumLists(ImprovedLinkedList<T>& l1, ImprovedLinkedList<T>& l2) {
            int carry = 0;

            if(l1.length() > l2.length())
                this->addZeros(l2, l1.length() - l2.length());
            else
                this->addZeros(l1, l2.length() - l1.length());
            
            reversedSumLists_aux(l1.head, l2.head, carry);

            if(carry != 0)
                this->insertFirst(carry);
        }
    private:
        void addZeros(ImprovedLinkedList<T>& list, int zeros) {
            for(int i = 0; i < zeros; i++) {
                list.insertFirst(0);
            }
        }

        void reversedSumLists_aux(Node<T>*& l1, Node<T>*& l2, int & carry) {
            /* l1 and l2 have the same length now*/
            if(l1 == NULL)
                return;

            reversedSumLists_aux(l1->next, l2->next, carry);
            
            int sum = l1->data + l2->data + carry;
            this->insertFirst(sum % 10);
            carry = sum / 10;            
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(N)$