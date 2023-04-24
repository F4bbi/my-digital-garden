---
{"dg-publish":true,"permalink":"/coding/cracking-the-coding-interview/chapter-2/problem-2-return-kth-to-last/","created":"2022-08-13T14:20:32.115+02:00","updated":"2023-01-25T15:41:29.244+01:00"}
---

# Chapter 2 - Problem 2 - Return Kth to Last
## Problem
Implement an algorithm to find the kth starting from the last element of a singly linked list.

## Solution
In the following implementations, `k=0` corresponds to the last element of the list.

#### Obvious solution in C++
(You can find the `get` function in [[Coding/Data Structures/Linked Lists#LinkedList cpp\|Linked Lists#LinkedList cpp]])
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
    public:
	    Node<T>* get(int position) const{
		    if(position < 0)
		        throw std::out_of_range("Out Of Range!");
		    Node<T>* current = this->head;
		    int i = 0;
		    while(i < position && current != NULL) {
		        current = current->next;
		        ++i;
		    }
		    if(current == NULL)
			    throw std::out_of_range("Out Of Range!");
		    return current;
		}
};

template<typename T>
class Solution : public LinkedList<T> {
    public:
        T returnKthToLast(int k) {
            return this->get(this->length() - 1 - k)->data;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(1)$

#### Recursive solution in C++ (in reality java)
If we used Java instead of C++, we could not pass back a node and a counter using normal return statements. 
One way to resolve this is to change the problem to simply printing the kth to last element. Then, we can pass
back the value of the counter simply through return values.
```cpp
template<typename T>
class Solution : public LinkedList<T> {
    public:
        T returnKthToLast(int k) {
            return returnKthToLast_aux(k, this->head);
        }
    private:
	    /* Helper function */
        T returnKthToLast_aux(int k, Node<T>* current) {
            if(current == NULL)
                return -1; //because I want that k = 0 means get the last element
            int index = returnKthToLast_aux(k, current->next) + 1;
            if(index == k)
                std::cout << current->data << std::endl;
            return index;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(N)$ (because of recursion)

#### Recursive solution in C++ passing values by reference
We are using C++ so we can pass values by reference. This allows us to return the node value, but also update the counter by passing a pointer to it.
```cpp
template<typename T>
class Solution : public LinkedList<T> {
    public:
        T returnKthToLast(int k) {
            int i = -1; //because I want that k = 0 means get the last element
            return returnKthToLast_aux(k, this->head, i)->data;
        }
    private:
        T returnKthToLast_aux(int k, Node<T>* current, int & index) {
            if(current == NULL)
                return NULL;
                
            Node<T>* result = returnKthToLast_aux(k, current->next, index);       
            index++;
            if(index == k)
                return current;
            return result;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(N)$

Actually, we can do this in Java too, wrapping the counter value with simple class (or even a single element array). In this way we can mimic passing by reference.
(The solution is simply replacing `int & index` with the below class).

```cpp
class Index {
	public int value = 0;
}
```

#### Iterative solution in C++
We place a pointer to the first element in the list and one to the Kth element. Then we slide them simultaneously across the list, until the second pointer will point to the last element. Magically, the first pointer will now point to the element at position `length - 1 - k`.
```cpp
template<typename T>
class Solution : public LinkedList<T> {
    public:
        T returnKthToLast(int k) {
            Node<T>* result = this->head;
            Node<T>* runner = this->get(k);

            while(runner->next != NULL) {
                runner = runner->next;
                result = result->next;
            }
            return result->data;
        }
};
```
- **Time complexity:** $O(N)$ (where _N_ is the size of the list)
- **Space complexity:** $O(1)$

