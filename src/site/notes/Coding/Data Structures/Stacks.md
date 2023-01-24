---
{"dg-publish":true,"permalink":"/coding/data-structures/stacks/"}
---

# Stacks
The stack data structure is simply a stack of data. It uses LIFO (last-in first-out) ordering. 

## Stack implementation
Unlike an array, a stack does not offer constant-time access to the ith item. However, it does allow constant time adds and removes, as it doesn't require shifting elements around.
#### My Stack class implementation in C++
##### Stack.h
```cpp
#pragma once

/* Forward declaration of the template, so that operator<< is regognized as a template */
template<typename T>
class Stack;

template<typename T>
std::ostream& operator<< (std::ostream& out, const Stack<T>& stack);

template<typename T>
class Stack {
    private:
        template<typename U>
        class Node {
            public:
                U data;
                Node* next;
                
                // default constructor
                Node() = default;

                // constructor with data
                Node(const U& value) : data(value), next{NULL} {}
            };
    protected:
        Node<T>* top;
    public:
        Stack() : top(NULL) {}
        Stack(const Stack& copySist);
        ~Stack();
        void initRandom(int size);
        T pop();
        void push(T value);
        T peek() const;
        bool isEmpty() const;
        void print() const;
        void clear();
        Stack<T>& operator= (const Stack<T>& stack);
        friend std::ostream& operator<< <> (std::ostream& out, const Stack<T>& stack);
};
```

##### Stack.cpp
```cpp
#include <iostream>

#include "Stack.h"

template <typename T>
Stack<T>::Stack(const Stack& copyStack) {
    Node<T>* newTop = this->top = new Node<T>();
    
    for(auto tmp = copyStack.top; tmp != NULL; tmp = tmp->next) {
        newTop->next = new Node<T>(tmp->data);
        newTop = newTop->next;
    }

    Node<T>* tmp = this->top;
    this->top = this->top->next;
    delete tmp;
}

template <typename T>
Stack<T>::~Stack() {
    this->clear();
}

template <typename T>
void Stack<T>::initRandom(int size) {
    for(int i = 0; i < size; i++) {
            int random = rand() % 10;
            push(random);
    }
}

template <typename T>
T Stack<T>::pop() {
    if(isEmpty())
        throw std::invalid_argument("The stack is empty!");
    Node<T> *temp = this->top;
    this->top = this->top->next;
    int data = temp->data;
    delete temp;
    return data;
}

template <typename T>
void Stack<T>::push(T value) {
    Node<T> *newTop = new Node<T>(value);
    newTop->next = this->top;
    top = newTop;
}

template <typename T>
T Stack<T>::peek() const {
    if(!isEmpty())
        return top->data;
}

template <typename T>
bool Stack<T>::isEmpty() const {
    return top == NULL; 
}

template <typename T>
void Stack<T>::clear() {
    while (!isEmpty())
        pop();
}

template <typename T>
void Stack<T>::print() const {
    std::cout << *this; 
}
 
template<typename T>
Stack<T>& Stack<T>::operator= (const Stack<T>& copyStack) {
    Stack<T> temp(copyStack);
    std::swap(temp.top, this->top);
    return *this;
    /* Since I swapped the heads, now temp destructor will be called, but temp now is the 
        original stack */
}

template <typename T>
std::ostream& operator<< (std::ostream& out, const Stack<T>& stack) {
    for(auto current = stack.top; current != NULL; current = current->next)
        out << current->data << " -> ";
    out << "NULL" << std::endl;
    return out;
}
```