---
{"dg-publish":true,"permalink":"/coding/data-structures/queues/"}
---

# Queues
A queue implements FIFO (first-in first-out) ordering. As in a line or queue at a ticket stand, items are removed from the data structure in the same order that they are added.

## Queue implementation
In the below implementation, we have a `Queue` class that contains the `Node` class. 
#### My Queue class implementation in C++
[Here](https://github.com/F4bbi/data-structures-implementation/tree/main/Queue) you can find the repository on GitHub with the complete code.
##### Queue.h
```cpp
#pragma once

#include <iostream> 

/* Forward declaration of the template, so that operator<< is regognized as a template */
template<class T>
class Queue;

template<class T>
std::ostream& operator<< (std::ostream& out, const Queue<T>& queue);

template<class T>    
class Queue {
    class Node {
        public:
            T data;
            Node* next;
            
            // default constructor
            Node() = default;
            // constructor with data
            Node(const T& value) : data(value), next(NULL) {}
    };
    protected:
        Node* first;
        Node* last;
    public:
        Queue() : first(NULL), last(NULL) {}
        Queue(const Queue& copyQueue);
        ~Queue();
        void initRandom(int size);
        T remove();
        void add(T value);
        T peek() const;
        bool isEmpty() const;
        void print() const;
        void clear();
        Queue<T>& operator= (const Queue<T>& queue);
        friend std::ostream& operator<< <> (std::ostream& out, const Queue<T>& queue);
};
```

##### Queue.cpp
```cpp
#include <iostream>

#include "Queue.h"

template<class T>
Queue<T>::Queue(const Queue& copyQueue) {
    Node* newFirst = this->first = new Node();
    
    for(auto tmp = copyQueue.first; tmp != NULL; tmp = tmp->next) {
        newFirst->next = new Node(tmp->data);
        newFirst = newFirst->next;
    }

    Node* tmp = this->first;
    this->first = this->first->next;
    delete tmp;
}

template<class T>
Queue<T>::~Queue() {
    this->clear();
}

template<class T>
void Queue<T>::initRandom(int size) {
    for(int i = 0; i < size; i++) {
            int random = rand() % 10;
            add(random);
    }
}

template<class T>
T Queue<T>::remove() {
    if(isEmpty())
        throw std::invalid_argument("The queue is empty!");
    Node *temp = this->first;
    this->first = this->first->next;
    int data = temp->data;
    delete temp;
    return data;
}

template<class T>
void Queue<T>::add(T value) {
    Node *node = new Node(value);
    if(isEmpty())
        this->first = this->last = node;
    else {
        this->last->next = node;
        this->last = this->last->next;
    }
}

template<class T>
T Queue<T>::peek() const {
    if(!isEmpty())
        return this->first->data;
}

template<class T>
bool Queue<T>::isEmpty() const {
    return this->first == NULL; 
}

template<class T>
void Queue<T>::clear() {
    while (!isEmpty())
        remove();
}

template<class T>
void Queue<T>::print() const {
    std::cout << *this; 
}
 
template<typename T>
Queue<T>& Queue<T>::operator= (const Queue<T>& copyQueue) {
    Queue<T> temp(copyQueue);
    std::swap(temp.top, this->top);
    return *this;
    /* Since I swapped the heads, now temp destructor will be called, but temp now is the 
        original queue */
}

template<class T>
std::ostream& operator<< (std::ostream& out, const Queue<T>& queue) {
    for(auto current = queue.first; current != NULL; current = current->next)
        out << current->data << " -> ";
    out << "NULL" << std::endl;
    return out;
}
```