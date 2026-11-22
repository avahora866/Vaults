---
tags:
  - DataStructure
---
# Under the hood

A bucket is a term to represent an index in the internal array that is the underlying structure creating the hash map data structure.

The internal array is of type linked list in java 1.8. In Java 8 it is still a linked list but gets turned into a tree once a certain threshold is reached

When inserting something into a map, if we iterate over the map to check which bucket is free, it will increase the complexity of the insert operation.   
Instead we hash the key and then mod the value to the array length to pick the bucket (index) we will store the value in. Effectively we are guessing on the probability that the bucket we are choosing is empty.

In an ideal world every time we insert something we get an empty bucket but if a collision appears then we add the values as another node to the linked list

```
Index | Bucket (Linked List)
--------------------------------
  0   | [Key A]  
  1   | [Key B]  
  2   | [Key C]  
  3   | [Key D]  
  4   | [Key E]  
  5   | [Key F]  
  6   | [Key G]  
  7   | [Key H]  

```

During the process of calculating the bucket we hash the key, we do this to help distribute the values uniformly in the internal array to reduce the amount of collisions that occur. (Less collisions means less list nodes added to the linked list resulting in a shorted list to traverse when getting the value)

Once the array has been filled by values any further inserts will all be treated as collisions and result in the increase of linked list, however the hashing function will make sure that collisions don't all occur on one bucket (array index)

During the get operation we hash the key and mod the array length to get the bucket. Once we have the bucket we traverse the linked list to find the key we are looking for

```
Index | Bucket (Linked List)
--------------------------------
  0   | null
  1   | null
  2   | null
  3   | [("Apple", "Red")] → [("Cherry", "Red")]  
  4   | null
  5   | [("Banana", "Yellow")] → [("Berry", "Blue")]
  6   | null
  7   | [("Mango", "Orange")]

```

## Red-Black Tree

After java 8 once a buckets linked list reaches a length of 8 or more it gets converted into a Red-Black Tree  
This is done to improve performance

* Linked List: O(n) lookup  
* Red-Black Tree: O(log n)

The Red-Black Tree will revert back into a linked list of the size of the bucket falls below 6 entries

From:

```
Index | Bucket (Linked List)
--------------------------------------
  0   | null
  1   | null
  2   | null
  3   | [("Apple", "Red")] → [("Banana", "Yellow")] → [("Cherry", "Red")] → [("Date", "Brown")] → [("Elderberry", "Purple")] → [("Fig", "Purple")] → [("Grape", "Green")] → [("Honeydew", "Green")]
  4   | null
  5   | null
  6   | null
  7   | null
```

To

```
Index | Bucket (Red-Black Tree)
--------------------------------------
  0   | null
  1   | null
  2   | null
  3   | [Tree: ("Date", "Brown") root]  
  4   | null
  5   | null
  6   | null
  7   | null



                ("Date", "Brown")
               /                  \
    ("Banana", "Yellow")    ("Honeydew", "Green")
         /        \              /               \
("Apple", "Red")   ("Cherry", "Red")  ("Fig", "Purple") ("Grape", "Green")
                                               \
                                        ("Elderberry", "Purple")

```

