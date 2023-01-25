---
{"dg-publish":true,"permalink":"/coding/data-structures/linked-lists/"}
---

# Linked Lists
A linked list is a data structure that represents a sequence of nodes. 
In a singly linked list, each node points to the next node in the linked list. 
A doubly linked list gives each node pointers to both the next node and the previous node.
Unlike an array, a linked list does not provide constant time access to a particular "index" within the list. This means that if you'd like to find the Kth element in the list, you will need to iterate through K elements.
The benefit of a linked list is that you can add and remove items from the beginning of the list in constant time.

## Linked List implementation
In the below implementation, we have a `Linked List` class that wraps the `Node` class. 
In fact, if we accessed the linked list through a reference to the head `Node` of the linked list, we should be a bit careful. What if multiple objects need a reference to the linked list, and then the head of the linked list changes? Some objects might still be pointing to the old head.

#### My Linked List class implementation in C++
[Here](https://github.com/F4bbi/data-structures-implementation/tree/main/Linked%20List) you can find the repository on GitHub with the complete code.
##### Node.cpp
```cpp
#pragma once

template<typename T>
class Node {
    public:
        T data;
        Node* next;
        
        // default constructor
        Node() = default;

        // constructor with data
        Node(const T& value) : data(value), next{NULL} {}
};
```

##### LinkedList.h
```cpp
#pragma once

#include "Node.cpp"

/* Forward declaration of the template, so that operator<< is regognized as a template */
template<typename T>
class LinkedList;

template<typename T>
std::ostream& operator<< (std::ostream& out, const LinkedList<T>& list);

template<typename T>
class LinkedList {
    protected:
        Node<T>* head;
    public:
        LinkedList() : head(NULL) {}
        LinkedList(const LinkedList& copyList);
        ~LinkedList();
        void insertLast(T value);
        void initRandom(int size);
        void insertFirst(T value);
        void insertOrder(T value);
        int length() const;
        void deleteElement(T value);
        void deleteFirst();
        void deleteLast();
        Node<T>* get(int position) const;
        void reverse();
        void print() const;
        void clear();
        LinkedList<T>& operator= (const LinkedList<T>& list);
        friend std::ostream& operator<< <> (std::ostream& out, const LinkedList<T>& list);
};
```

##### LinkedList.cpp
```cpp
#include <iostream>

#include "LinkedList.h"

template <typename T>
LinkedList<T>::LinkedList(const LinkedList& copyList) {
    Node<T>* newHead = this->head = new Node<T>();
    
    /* I could simply do this->insertlast() but it would be less efficient */
    for(auto tmp = copyList.head; tmp != NULL; tmp = tmp->next) {
        newHead->next = new Node<T>(tmp->data);
        newHead = newHead->next;
    }

    Node<T>* tmp = this->head;
    this->head = this->head->next;
    delete tmp;
}

template <typename T>
LinkedList<T>::~LinkedList() {
    this->clear();
}

template <typename T>
void LinkedList<T>::insertLast(T value) {
    Node<T>* newNode = new Node<T>(value);
    if(this->head == NULL)
        this->head = newNode;
    else {
        Node<T>* current = head;
        for(; current->next != NULL; current = current->next);
        current->next = newNode; 
    }
}

template <typename T>
void LinkedList<T>::initRandom(int size) {
    for(int i = 0; i < size; i++) {
        int random = rand() % 10;
        insertLast(random);
    }
}

template <typename T>
void LinkedList<T>::insertFirst(T value) {
    Node<T>* newHead = new Node<T>(value);
    newHead->next = this->head;
    this->head = newHead;
}

template <typename T>
void LinkedList<T>::insertOrder(T value) {
    if((this->head == NULL) || (value < this->head->data)){
        insertFirst(value);
    }
    else {
        Node<T>* temp = head;
        Node<T>* node = new Node<T>(value);
        for(;(temp->next != NULL) && (value > temp->next->data); temp = temp->next);
        node->next = temp->next;
        temp->next = node;
    }
}

template <typename T>
int LinkedList<T>::length() const {
    int i = 0;
    for (auto current = this->head; current != NULL; current = current->next, i++);
    return i;
}

template <typename T>
void LinkedList<T>::deleteFirst() {
    if(this->head == NULL)
        return;
    Node<T>* temp = this->head;
    this->head = this->head->next;
    delete temp;
}

template <typename T>
void LinkedList<T>::deleteLast() {
    if(this->head == NULL)
        return;
    if(this->head->next == NULL) {
        delete head;
        return;
    }
    
    Node<T>* current = this->head;
    for (; (current->next->next != NULL); current = current->next);
    Node<T>* temp = current->next;
    current->next = current->next->next;
    delete temp;
}

template <typename T>
void LinkedList<T>::deleteElement(T value) {
    if(this->head == NULL)
        return;
    
    Node<T>* current = this->head;

    if (current->data == value)
        deleteFirst();
    
    for (; (current->next != NULL) && (value != current->next->data); current = current->next);
    
    if(current->next == NULL)
        return; 
    
    Node<T>* temp = current->next;
    current->next = current->next->next;
    delete temp;
}

template <typename T>
Node<T>* LinkedList<T>::get(int position) const{
    if(position < 0)
        throw std::out_of_range("Out Of Range!");

    Node<T>* current = this->head;
    int i = 0;
    while(i < position && current != NULL) {
        current = current->next;
        ++i;
    }
    if(current == NULL) throw std::out_of_range("Out Of Range!");
    return current;
}

template <typename T>
void LinkedList<T>::reverse() {
    if(this->head == NULL)
        return;

    Node<T> * previous = NULL, * current = this->head, * next = NULL;

    while(current != NULL){
        next = current->next;
        current->next = previous;
        previous = current;
        current = next;
    }
    this->head = previous;    
}

template <typename T>
void LinkedList<T>::clear() {
    while (this->head != NULL)
        deleteFirst();
}

template <typename T>
void LinkedList<T>::print() const {
    std::cout << *this; 
}
 
template<typename T>
LinkedList<T>& LinkedList<T>::operator= (const LinkedList<T>& copyList) {
    LinkedList<T> temp(copyList);
    std::swap(temp.head, this->head);
    return *this;
    /* Since I swapped the heads, now temp destructor will be called, but temp now is the 
        original list */
}

template <typename T>
std::ostream& operator<< (std::ostream& out, const LinkedList<T>& list) {
    for(auto current = list.head; current != NULL; current = current->next)
        out << current->data << " -> ";
    out << "NULL" << std::endl;
    return out;
}
```