---
tags:
  - DataStructure
---

A Linked List is a linear data structure, in which elements are not stored at a contiguous location, rather they are linked using pointers. Linked List forms a series of connected nodes, where each node stores the data and the address of the next node.

Unlike **arrays, linked lists** allow for efficient **insertion** or **removal** of elements from any position in the list, as the nodes are not stored contiguously in memory.

# Linked Lists vs Arrays
Linked List:
- Data Structure: Non-contiguous
- Memory Allocation: Dynamic
- Insertion/Deletion: Efficient
- Access: Sequential
Array:
- Data Structure: Contiguous
- Memory Allocation: Static
- Insertion/Deletion: Inefficient
- Access: Random
# Linked lists applications
- Implementing stacks and queues using linked lists.
- Using linked lists to handle collisions in hash tables.
- Representing graphs using linked lists.
- Allocating and deallocating memory dynamically.

# Terms
- Node Structure: A node in a linked list typically consists of two components:
- Data: It holds the actual value or data associated with the node.
- Next Pointer: It stores the memory address (reference) of the next node in the sequence.
- Head and Tail: The linked list is accessed through the head node, which points to the first node in the list. The last node in the list points to NULL or nullptr, indicating the end of the list. This node is known as the tail node.
# Advantages
- Dynamic Data structure: The size of memory can be allocated or de-allocated at run time based on the operation insertion or deletion.
- Ease of Insertion/Deletion: The insertion and deletion of elements are simpler than arrays since no elements need to be shifted after insertion and deletion, Just the address needed to be updated.
- Efficient Memory Utilization: As we know Linked List is a dynamic data structure the size increases or decreases as per the requirement so this avoids the wastage of memory. 
- Implementation: Various advanced data structures can be implemented using a linked list like a stack, queue, graph, hash maps, etc.
# Types
1. [[Singly Linked List|Singly Linked List]]
In a singly linked list, each node contains a reference to the next node in the sequence. Traversing a singly linked list is done in a forward direction.
2. [[Doubly Linked List|Doubly Linked List]]
In a doubly linked list, each node contains references to both the next and previous nodes. This allows for traversal in both forward and backward directions, but it requires additional memory for the backward reference.
3. [[Circular Linked List|Circular Linked List]]
In a circular linked list, the last node points back to the head node, creating a circular structure. It can be either singly or doubly linked.
# Operations on Linked Lists
- Insertion: Adding a new node to a linked list involves adjusting the pointers of the existing nodes to maintain the proper sequence. Insertion can be performed at the beginning, end, or any position within the list
- Deletion: Removing a node from a linked list requires adjusting the pointers of the neighboring nodes to bridge the gap left by the deleted node. Deletion can be performed at the beginning, end, or any position within the list.
- Searching: Searching for a specific value in a linked list involves traversing the list from the head node until the value is found or the end of the list is reached.
# Complexity Analysis of Linked List:
Time Complexity: **O(n)**
Auxiliary Space: **O(n)**

# Advantages of Linked Lists
- Dynamic Size: Linked lists can grow or shrink dynamically, as memory allocation is done at runtime.
- Insertion and Deletion: Adding or removing elements from a linked list is efficient, especially for large lists.
- Flexibility: Linked lists can be easily reorganized and modified without requiring a contiguous block of memory.
# Disadvantages of Linked Lists
- Random Access: Unlike arrays, linked lists do not allow direct access to elements by index. Traversal is required to reach a specific node.
- Extra Memory: Linked lists require additional memory for storing the pointers, compared to arrays.




