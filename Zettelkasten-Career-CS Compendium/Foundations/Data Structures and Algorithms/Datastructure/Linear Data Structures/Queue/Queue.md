---
tags:
  - DataStructure
---
Queue Data Structure is a linear data structure that follows FIFO (First In First Out) Principle, so the first element inserted is the first to be popped out. 

We define a queue to be a list in which all additions to the list are made at one end (back of the queue), and all deletions from the list are made at the other end(front of the queue). The element which is first pushed into the order, the delete operation is first performed on that.

# FIFO Principle of Queue Data Structure:
- A Queue is like a line waiting to purchase tickets, where the first person in line is the first person served. (i.e. First Come First Serve).
- Position of the entry in a queue ready to be served, that is, the first entry that will be removed from the queue, is called the front of the queue(sometimes, head of the queue). Similarly, the position of the last entry in the queue, that is, the one most recently added, is called the rear (or the tail) of the queue.

# Types of Queue Data Structure:
1.  Simple Queue: Simple Queue simply follows FIFO Structure. We can only insert the element at the back and remove the element from the front of the queue.
2.  Double-Ended Queue (Dequeue): In a double-ended queue the insertion and deletion operations, both can be performed from both ends. They are of two types:
    - Input Restricted Queue: This is a simple queue. In this type of queue, the input can be taken from only one end but deletion can be done from any of the ends.
    - Output Restricted Queue: This is also a simple queue. In this type of queue, the input can be taken from both ends but deletion can be done from only one end.
3.  Circular Queue: This is a special type of queue where the last position is connected back to the first position. Here also the operations are performed in FIFO order.
4.  Priority Queue: A priority queue is a special queue where the elements are accessed based on the priority assigned to them. They are of two types:
    - Ascending Priority Queue: In Ascending Priority Queue, the elements are arranged in increasing order of their priority values. Element with smallest priority value is popped first.
    - Descending Priority Queue: In Descending Priority Queue, the elements are arranged in decreasing order of their priority values. Element with largest priority is popper first

# Basic Operations in Queue Data Structure:

Some of the basic operations for Queue in Data Structure are:

1.  Enqueue: Adds (or stores) an element to the end of the queue..
2.  Dequeue: Removal of elements from the queue.
3.  Peek or front: Acquires the data element available at the front node of the queue without deleting it.
4.  rear: This operation returns the element at the rear end without removing it.
5.  isFull: Validates if the queue is full.
6.  isEmpty: Checks if the queue is empty.

# Applications of Queue Data Structure:

Application of queue is common. In a computer system, there may be queues of tasks waiting for the printer, for access to disk storage, or even in a time-sharing system, for use of the CPU. Within a single program, there may be multiple requests to be kept in a queue, or one task may create other tasks, which must be done in turn by keeping them in a queue.

- Queue can be used in job scheduling like Printer Spooling.
- Queue can be used where we have a single resource and multiple consumers.
- In a network, a queue is used in devices such as a router/switch and mail queue.
- Queue can be used in various algorithm techniques like Breadth First Search, [[Topological Sort|Topological Sort]], etc.

# Advantages of Queue Data Structure:

- A large amount of data can be managed efficiently with ease.
- Operations such as insertion and deletion can be performed with ease as it follows the first in first out rule.
- Queues are useful when a particular service is used by multiple consumers.
- Queues are fast in speed for data inter-process communication.
- Queues can be used in the implementation of other data structures.

# Disadvantages of Queue Data Structure:

- The operations such as insertion and deletion of elements from the middle are time consuming.
- Searching an element takes O(N) time.
- Maximum size of a queue must be defined prior in case of array implementation.