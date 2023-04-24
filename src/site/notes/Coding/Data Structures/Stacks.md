---
{"dg-publish":true,"permalink":"/coding/data-structures/stacks/","created":"2022-09-22T19:32:14.342+02:00","updated":"2023-01-25T00:35:05.833+01:00"}
---

# Stacks
The stack data structure is simply a stack of data. It uses LIFO (last-in first-out) ordering. 

## Stack implementation
Unlike an array, a stack does not offer constant-time access to the ith item. However, it does allow constant time adds and removes, as it doesn't require shifting elements around.
#### My Stack class implementation in C++
[Here](https://github.com/F4bbi/data-structures-implementation/tree/main/Stack) you can find the repository on GitHub with the complete code.
##### Stack.h
```cpp
#pragma once

/* Forward declaration of the template, so that operator<< is regognized as a template */
template<class T>
class Stack;

template<class T>
std::ostream& operator<< (std::ostream& out, const Stack<T>& stack);

template<class T>
class Stack {
    class Node {
        public:
            T data;
            Node* next;
            
            // default constructor
            Node() = default;

            // constructor with data
            Node(const T& value) : data(value), next{NULL} {}
    };
    protected:
        Node* top;
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

template <class T>
Stack<T>::Stack(const Stack& copyStack) {
    Node* newTop = this->top = new Node();
    
    for(auto tmp = copyStack.top; tmp != NULL; tmp = tmp->next) {
        newTop->next = new Node(tmp->data);
        newTop = newTop->next;
    }

    Node* tmp = this->top;
    this->top = this->top->next;
    delete tmp;
}

template <class T>
Stack<T>::~Stack() {
    this->clear();
}

template <class T>
void Stack<T>::initRandom(int size) {
    for(int i = 0; i < size; i++) {
            int random = rand() % 10;
            push(random);
    }
}

template <class T>
T Stack<T>::pop() {
    if(isEmpty())
        throw std::invalid_argument("The stack is empty!");
    Node *temp = this->top;
    this->top = this->top->next;
    int data = temp->data;
    delete temp;
    return data;
}

template <class T>
void Stack<T>::push(T value) {
    Node *newTop = new Node(value);
    newTop->next = this->top;
    top = newTop;
}

template <class T>
T Stack<T>::peek() const {
    if(!isEmpty())
        return top->data;
}

template <class T>
bool Stack<T>::isEmpty() const {
    return top == NULL; 
}

template <class T>
void Stack<T>::clear() {
    while (!isEmpty())
        pop();
}

template <class T>
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

template <class T>
std::ostream& operator<< (std::ostream& out, const Stack<T>& stack) {
    for(auto current = stack.top; current != NULL; current = current->next)
        out << current->data << " -> ";
    out << "NULL" << std::endl;
    return out;
}
```