---
tags:
  - DataStructure
---
The circular linked list is a linked list where all nodes are connected to form a circle. In a circular linked list, the first node and the last node are connected to each other which forms a circle. There is no NULL at the end.

There are generally two types of circular linked lists:
- Circular singly linked list: In a circular Singly linked list, the last node of the list contains a pointer to the first node of the list. We traverse the circular singly linked list until we reach the same node where we started. The circular singly linked list has no beginning or end. No null value is present in the next part of any of the nodes.
- Circular Doubly linked list: Circular Doubly Linked List has properties of both doubly linked list and circular linked list in which two consecutive elements are linked or connected by the previous and next pointer and the last node points to the first node by the next pointer and also the first node points to the last node by the previous pointer.

# Operations on the circular linked list:
We can do some operations on the circular linked list similar to the singly linked list which are:

# Insertion in the circular linked list:
A node can be added in three ways:

1. Insertion at the beginning of the list
2. Insertion at the end of the list
3. Insertion in between the nodes

# Deletion in a circular linked list:
1. Delete the node only if it is the only node in the circular linked list:

	- Free the node’s memory
	- The last value should be NULL A node always points to another node, so NULL assignment is not necessary.
	- Any node can be set as the starting point.
	- Nodes are traversed quickly from the first to the last.
2. Deletion of the last node:
	- Locate the node before the last node (let it be temp)
	- Keep the address of the node next to the last node in temp
	- Delete the last memory
	- Put temp at the end
3. Delete any node from the circular linked list: We will be given a node and our task is to delete that node from the circular linked list.

# Advantages of Circular Linked Lists: 
- Any node can be a starting point. We can traverse the whole list by starting from any point. We just need to stop when the first visited node is visited again. 
- Useful for implementation of a queue. Unlike this implementation, we don’t need to maintain two pointers for front and rear if we use a circular linked list. We can maintain a pointer to the last inserted node and the front can always be obtained as next of last.
- Circular lists are useful in applications to repeatedly go around the list. For example, when multiple applications are running on a PC, it is common for the operating system to put the running applications on a list and then cycle through them, giving each of them a slice of time to execute, and then making them wait while the CPU is given to another application. It is convenient for the operating system to use a circular list so that when it reaches the end of the list it can cycle around to the front of the list. 
- Circular Doubly Linked Lists are used for the implementation of advanced data structures like the Fibonacci Heap.
-  Implementing a circular linked list can be relatively easy compared to other more complex data structures like trees or graphs.

# Disadvantages of circular linked list:
- Compared to singly linked lists, circular lists are more complex.
- Reversing a circular list is more complicated than singly or doubly reversing a circular list.
- It is possible for the code to go into an infinite loop if it is not handled carefully.
- It is harder to find the end of the list and control the loop.
- Although circular linked lists can be efficient in certain applications, their performance can be slower than other data structures in certain cases, such as when the list needs to be sorted or searched.
- Circular linked lists don’t provide direct access to individual nodes

# Applications of circular linked lists:
- Multiplayer games use this to give each player a chance to play.
- A circular linked list can be used to organize multiple running applications on an operating system. These applications are iterated over by the OS.
- Circular linked lists can be used in resource allocation problems.
- Circular linked lists are commonly used to implement circular buffers,
- Circular linked lists can be used in simulation and gaming.

# Why circular linked list?
- A node always points to another node, so NULL assignment is not necessary.
- Any node can be set as the starting point.
- Nodes are traversed quickly from the first to the last.


