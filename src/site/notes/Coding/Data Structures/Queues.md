---
{"dg-publish":true,"permalink":"/coding/data-structures/queues/"}
---

# Queues
A queue implements FIFO (first-in first-out) ordering. As in a line or queue at a ticket stand, items are removed from the data structure in the same order that they are added.

## Queue implementation
In the below implementation, we have a `Queue` class that contains the `Node` class. 
#### My Queue class implementation in C++
##### Queue.h
```cpp
#pragma once

#include <iostream>

/* Forward declaration of the template, so that operator<< is regognized as a template */
template<typename T>
class Queue;

template<typename T>
std::ostream& operator<< (std::ostream& out, const Queue<T>& queue);

template<typename T>
class Queue {
    private:
        template<typename U>
        class Node {
            public:
                U data;
                Node* next;
                
                // default constructor
                Node() = default;

                // constructor with data
                Node(const U& value) : data(value), next(NULL) {}
            };
    protected:
        Node<T>* first;
        Node<T>* last;
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

template <typename T>
Queue<T>::Queue(const Queue& copyQueue) {
    Node<T>* newFirst = this->first = new Node<T>();
    
    for(auto tmp = copyQueue.first; tmp != NULL; tmp = tmp->next) {
        newFirst->next = new Node<T>(tmp->data);
        newFirst = newFirst->next;
    }

    Node<T>* tmp = this->first;
    this->first = this->first->next;
    delete tmp;
}

template <typename T>
Queue<T>::~Queue() {
    this->clear();
}

template <typename T>
void Queue<T>::initRandom(int size) {
    for(int i = 0; i < size; i++) {
            int random = rand() % 10;
            add(random);
    }
}

template <typename T>
T Queue<T>::remove() {
    if(isEmpty())
        throw std::invalid_argument("The queue is empty!");
    Node<T> *temp = this->first;
    this->first = this->first->next;
    int data = temp->data;
    delete temp;
    return data;
}

template <typename T>
void Queue<T>::add(T value) {
    Node<T> *node = new Node<T>(value);
    if(isEmpty())
        this->first = this->last = node;
    else {
        this->last->next = node;
        this->last = this->last->next;
    }
}

template <typename T>
T Queue<T>::peek() const {
    if(!isEmpty())
        return this->first->data;
}

template <typename T>
bool Queue<T>::isEmpty() const {
    return this->first == NULL; 
}

template <typename T>
void Queue<T>::clear() {
    while (!isEmpty())
        remove();
}

template <typename T>
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

template <typename T>
std::ostream& operator<< (std::ostream& out, const Queue<T>& queue) {
    for(auto current = queue.first; current != NULL; current = current->next)
        out << current->data << " -> ";
    out << "NULL" << std::endl;
    return out;
}
```