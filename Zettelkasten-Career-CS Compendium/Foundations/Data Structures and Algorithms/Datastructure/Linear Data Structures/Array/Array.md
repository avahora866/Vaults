---
tags:
  - DataStructure
---
Arrays are a collection of items of the same type of variable that are stored in memory next to each other. Arrays are a fixed size.

**They are best at accessing an element at O(1)**
# Types of Arrays

Arrays can be classified in 2 ways:

- On the basis of Memory allocation
- On the basis of dimensions

## Memory Allocation

1.  Static Arrays:  
    In this type of array **memory is allocated at compile time** having a fixes size. Here only a fixed size of memory will be allocated for storage.  
    In case we don't know the size of the array and simply set the size of array to a really high number (4000), will lead to more memory being used then needed, this is called a **memory leek**.
2.  Dynamic Arrays:  
    In this type of array **memory is allocated at run time** but **not having a fixed size**. These are created during the background of an **Array List**.

## Dimensions

1.  1-D Array:  
    Elements are store one after another  
2.  Multi-dimensional Array:  
    These are arrays with more than one dimension. We can have 2-D Arrays, 3-D Arrays or 4-D Arrays
- 2-D arrays can be considered an arrays of arrays or as a matrix consisting of rows and columns  
	- 3-D Array contains 3 dimensions, so it can be considered an array of 2-D Array.  


# Operations

## Time Complexity

![946dd6f430fff40d4d5b197dfed31d74.png](946dd6f430fff40d4d5b197dfed31d74.png)

## Space Complexity

![9fc7ebec3b968845a32c4dd7ed20523a.png](9fc7ebec3b968845a32c4dd7ed20523a.png)

## Traversal

This is simply looping through the array

## Insertion

Insertion is a very expensive computational operation as when a item is inserted all the indexes followed by said insert is moved up one.  
**Note: if you insert into a full array it will remove the last element**

## Deletion

Deletion is also a very expensive operation for the same reason as above  
**Note: If you delete in the middle of an array the following indexes will fall back down**

# Advantages

- Arrays allow **random access** to elements. This makes accessing elements by position faster.
- Arrays have better ==cache locality== which makes a pretty big difference in performance.
- Arrays represent multiple data items of the same type using a single name.
- Arrays are used to **implement the other data structures** like linked lists, stacks, queues, trees, graphs, etc.

# Disadvantages

- As arrays have a **fixed size**, once the memory is allocated to them, it cannot be increased or decreased, making it impossible to store extra data if required. An array of fixed size is referred to as a **static array**.
- Allocating **less memory** than required to an array leads to **loss of data**.
- An array is ==homogeneous== in nature so, a single array cannot store values of different data types.
- Arrays store data in **contiguous** memory locations, which makes **deletion and insertion very difficult to implement**. This problem is overcome by implementing **linked lists**, which allow elements to be accessed sequentially.

# Applications

- They are used in the implementation of **other data structures** such as array lists, heaps, hash tables, vectors, and matrices.
- **Database records** are usually implemented as arrays.
- It is used in **lookup tables** by computer.