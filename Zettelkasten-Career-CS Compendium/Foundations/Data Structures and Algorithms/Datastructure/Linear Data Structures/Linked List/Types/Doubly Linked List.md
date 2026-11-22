---
tags:
  - DataStructure
---
Inserting a new node in a doubly linked list is very similar to inserting new node in linked list. There is a little extra work required to maintain the link of the previous node. A node can be inserted in a Doubly Linked List in four ways:

- At the front of the DLL. 
- In between two nodes
	- After a given node.
	- Before a given node.
- At the end of the DLL.
# Insertion at the beginning in Doubly Linked List
![c473a8e4dabcddcc4cffae2a7a24ccd9.png](c473a8e4dabcddcc4cffae2a7a24ccd9.png)
To insert a new node at the beginning of the doubly list, we can use the following steps:

- Allocate memory for a new node (say new_node) and assign the provided value to its data field.
- Set the previous pointer of the new_node to nullptr.
- If the list is empty:
	- Set the next pointer of the new_node to nullptr.
	- Update the head pointer to point to the new_node.
- If the list is not empty:
	- Set the next pointer of the new_node to the current head.
	- Update the previous pointer of the current head to point to the new_node.
	- Update the head pointer to point to the new_node.

**Time Complexity: O(1)
Auxiliary Space: O(1)**
# Insertion in between two nodes in Doubly Linked List
![b8907300ad62dc9f79b2cba1795dc6f6.png](b8907300ad62dc9f79b2cba1795dc6f6.png)
1. Add a node after a given node in a Doubly Linked List:
We are given a pointer to a node as prev_node, and the new node is inserted after the given node. This can be done using the following steps:

- Firstly create a new node (say new_node).
- Now insert the data in the new node.
- Point the next of new_node to the next of prev_node.
- Point the next of prev_node to new_node.
- Point the previous of new_node to prev_node.
- Point the previous of next of new_node to new_node.

**Time Complexity: O(1)
Auxiliary Space: O(1)**
# Insertion at the End in Doubly Linked List:
![bab43f120b7d8fdd83d2e8e689d382c9.png](bab43f120b7d8fdd83d2e8e689d382c9.png)
The new node is always added after the last node of the given Linked List. This can be done using the following steps:

- Create a new node (say new_node).
- Put the value in the new node.
- Make the next pointer of new_node as null.
- If the list is empty, make new_node as the head.
- Otherwise, travel to the end of the linked list.
- Now make the next pointer of last node point to new_node.
- Change the previous pointer of new_node to the last node of the list.

**Time Complexity: O(n)
Auxiliary Space: O(1)**


