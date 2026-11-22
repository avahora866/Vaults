---
tags:
  - Java
---
# Implementation Details

Internally, PriorityQueue is implemented using a binary heap (a complete binary tree stored as an array). The heap ensures that:

- The smallest (or highest-priority) element is always at the root.
- Insertion (offer()) and removal (poll()) operations maintain heap order using heapify-up and heapify-down operations.

Heap Structure Example:

```java
PriorityQueue<Integer> pq = new PriorityQueue<>(); 
pq.offer(5); 
pq.offer(2); 
pq.offer(8); 
pq.offer(1);
```

Internally, this forms a min-heap like:
```
	1
   / \
  2   8
 /   
5
```


The smallest element (1) is always at the top, though the array representation may not always match this structure directly.

# Edge Cases & Considerations

## Null Elements Not Allowed

Unlike other collections like LinkedList, PriorityQueue does not allow null elements. Attempting to add null will throw a NullPointerException.

```java
PriorityQueue<Integer> pq = new PriorityQueue<>(); pq.offer(null); // Throws NullPointerException
```

## Thread Safety

PriorityQueue is not thread-safe. If used in a multi-threaded environment, you should wrap it using PriorityBlockingQueue from java.util.concurrent.

## Duplicate Elements Are Allowed

Unlike Set, PriorityQueue allows duplicate values, but they will be processed according to priority.

## Custom Ordering Using Comparator

You can use a custom Comparator to change the order, for example, creating a max-heap instead of a min-heap:

```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
```

# Java Code Example

```java
import java.util.PriorityQueue;
public class Main {     
	public static void main(String[] args) {
		PriorityQueue<Integer> pq = new PriorityQueue<>();         
		pq.offer(10);
		pq.offer(5);         
		pq.offer(20);         
		pq.offer(1);          
		System.out.println("Top element (peek): " + pq.peek()); 
		// 1        
		 while (!pq.isEmpty()) {       
		 // Prints in ascending order: 1, 5, 10, 20            
		 System.out.println(pq.poll()); 
		 }     
	} 
}
```

![[Priority Queue]]