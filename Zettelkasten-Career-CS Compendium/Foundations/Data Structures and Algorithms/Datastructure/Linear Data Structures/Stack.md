---
tags:
  - DataStructure
---
A Stack is a linear data structure that follows a particular order in which the operations are performed. The order may be LIFO (Last In First Out) or FILO (First In Last Out). LIFO implies that the element that is inserted last, comes out first and FILO implies that the element that is inserted first, comes out last.

- Pushing an element onto the stack is like adding a new plate on top.
- Popping an element removes the top plate from the stack.

# Key Operations on Stack Data Structures

- Push: Adds an element to the top of the stack.
- Pop: Removes the top element from the stack.
- Peek: Returns the top element without removing it.
- IsEmpty: Checks if the stack is empty.
- IsFull: Checks if the stack is full (in case of fixed-size arrays).

# Applications of Stack Data Structures

- Recursion
- Expression Evaluation and Parsing
- Depth-First Search (DFS)
- Undo/Redo Operations
- Browser History
- Function Calls

# Types of Stack Data Structure:

- Fixed Size Stack : As the name suggests, a fixed size stack has a fixed size and cannot grow or shrink dynamically. If the stack is full and an attempt is made to add an element to it, an overflow error occurs. If the stack is empty and an attempt is made to remove an element from it, an underflow error occurs.
- Dynamic Size Stack : A dynamic size stack can grow or shrink dynamically. When the stack is full, it automatically increases its size to accommodate the new element, and when the stack is empty, it decreases its size. This type of stack is implemented using a linked list, as it allows for easy resizing of the stack.

# Operations
1. Push Operation: Adds an item to the stack. If the stack is full, then it is said to be an Overflow condition.
2. Pop Operation: Removes an item from the stack. The items are popped in the reversed order in which they are pushed. If the stack is empty, then it is said to be an Underflow condition.
3. Top or Peek Operation: Returns the top element of the stack.
4. isEmpty Operation: Returns true if the stack is empty, else false.
5. isFull Operation:Returns true if the stack is full, else false.

# Implementation of Stack Data Structure:

The basic operations that can be performed on a stack include push, pop, and peek. There are two ways to implement a stack –

- Using Array
- Using Linked List

In an array-based implementation, the push operation is implemented by incrementing the index of the top element and storing the new element at that index. The pop operation is implemented by returning the value stored at the top index and then decrementing the index of the top element.

In a linked list-based implementation, the push operation is implemented by creating a new node with the new element and setting the next pointer of the current top node to the new node. The pop operation is implemented by setting the next pointer of the current top node to the next node and returning the value of the current top node.

# Advantages of Stack Data Structure:

- Simplicity: Stacks are a simple and easy-to-understand data structure, making them suitable for a wide range of applications.
- Efficiency: Push and pop operations on a stack can be performed in constant time (O(1)) , providing efficient access to data.
- Last-in, First-out (LIFO): Stacks follow the LIFO principle, ensuring that the last element added to the stack is the first one removed. This behaviour is useful in many scenarios, such as function calls and expression evaluation.
- Limited memory usage: Stacks only need to store the elements that have been pushed onto them, making them memory-efficient compared to other data structures.

# Disadvantages of Stack Data Structure:

- Limited access: Elements in a stack can only be accessed from the top, making it difficult to retrieve or modify elements in the middle of the stack.
- Potential for overflow: If more elements are pushed onto a stack than it can hold, an overflow error will occur, resulting in a loss of data.
- Not suitable for random access: Stack s do not allow for random access to elements, making them unsuitable for applications where elements need to be accessed in a specific order.
- Limited capacity: Stacks have a fixed capacity, which can be a limitation if the number of elements that need to be stored is unknown or highly variable.

# Applications of Stack Data Structure:

- <ins>Infix to Postfix</ins> /Prefix conversion
- Redo-undo features at many places like editors, photoshop.
- Forward and backward features in web browsers
- In Memory management, any modern computer uses a stack as the primary management for a running purpose. Each program that is running in a computer system has its own memory allocations.
- Stack also helps in implementing function call in computers. The last called function is always completed first.