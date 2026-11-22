---
tags:
  - Algorithm
  - ALGO_STUDY
---
Fast and Slow pointers is a pointer approach that uses **two pointers moving at different speeds** through a sequence (usually [[Linked Lists|linked list]]), with the **fast pointer moving twice as fast as the slow pointer**.

# Explanation
__How it works, what concept it uses, any requirements of the input data__
The fast and slow pointers are used for the following situations:
- Detecting cycles in linked lists (as the fast pointer moves twice as fast eventually it will reach null (end of list) or the slow pointer (cycled list))
- Finding middle elements (Once the fast pointer reaches the end, the slow pointer at that point represents the middle element)
- Finding list lengths (includes a counter that increments ever time you update the fast pointer)

## Time Complexity
Time complexity is O(n) with O(1) space.

## State Walkthrough
### State Walkthrough - Cycle Detection
Consider a linked list with five nodes:

```
1 → 2 → 3 → 4 → 5
          ↑    ↓
          7 ←  6
```

The cycle starts at node 3 and loops back from node 7 to node 3.

#### Step 1: Initialize Pointers
slow starts at node 1.
fast starts at node 1.


```
slow → 1
fast → 1
```

#### Step 2: Move the Pointers
slow moves one step: now at node 2.
fast moves two steps: now at node 3.

```
1 → 2 → 3 → 4 → 5
          ↑    ↓
          7 ←  6
```

```
slow → 2
fast → 3
```
#### Step 3: Continue Moving
slow moves one step: now at node 3.
fast moves two steps: now at node 5.

```
1 → 2 → 3 → 4 → 5
          ↑    ↓
          7 ←  6
```

```
slow → 3
fast → 5
```

#### Step 4: Move Again
slow moves one step: now at node 4.
fast moves two steps: now at node 7.

```
1 → 2 → 3 → 4 → 5
          ↑    ↓
          7 ←  6
```

```
slow → 4
fast → 7
```
#### Step 5: Move Further
slow moves one step: now at node 5.
fast moves two steps: now at node 4 (since node 7 loops back to node 3).


```
1 → 2 → 3 → 4 → 5
          ↑    ↓
          7 ←  6
```

```
slow → 5
fast → 4
```
#### Step 6: Move Again
slow moves one step: now at node 6.
fast moves two steps: now at node 6.

```
1 → 2 → 3 → 4 → 5
          ↑    ↓
          7 ←  6
```

```
slow → 6
fast → 6
```

At this point, both pointers meet, confirming a cycle.

---

#### Summary of How It Works
1. The slow pointer moves one step at a time.
2. The fast pointer moves two steps at a time.
3. If there is no cycle, fast will reach the end (null).
4. If there is a cycle, fast and slow will eventually meet inside the cycle.

# Example code
```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
    }
}

class FastSlowPointers {
    public boolean hasCycle(ListNode head) {
        if (head == null) return false;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }

	public int findMiddle(ListNode head) {
        if (head == null) return false;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow.val;
    }
}
```