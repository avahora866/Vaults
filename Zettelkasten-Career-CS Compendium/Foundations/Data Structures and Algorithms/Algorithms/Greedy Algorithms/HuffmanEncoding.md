---
tags:
  - Algorithm
  - ALGO_STUDY
---
**Huffman Encoding**, a classic **Greedy Algorithm** used for **data compression**.

---

**Huffman Encoding** is an algorithm used to **compress data** by assigning **shorter binary codes to more frequent characters**, and longer codes to less frequent characters.

It generates a **prefix-free binary tree** so that no code is a prefix of another, enabling unambiguous decoding.

---

# Why is it Greedy?

Because at every step, it **greedily combines the two least frequent characters/nodes**, assuming this choice leads to the optimal tree.

---

# Steps of Huffman Encoding

1. Count frequency of each character.

2. Create a **min-heap (priority queue)** of nodes where each node represents a character and its frequency.

3. While there is more than one node in the heap:

   * Extract the two nodes with the **smallest frequencies**.

   * Create a new node with frequency \= sum of the two.

   * Set the two nodes as the **left and right children** of the new node.

   * Push the new node back into the heap.

4. The remaining node is the **root of the Huffman Tree**.

5. Traverse the tree to assign **binary codes** (left \= `0`, right \= `1`).

---

# Example Input:

```
Characters: a, b, c, d, e, f  
Frequencies: 5, 9, 12, 13, 16, 45
```

---

# Java Implementation

```java
import java.util.*;

class Node {
    char ch;
    int freq;
    Node left, right;

    Node(char ch, int freq) {
        this.ch = ch;
        this.freq = freq;
    }

    Node(int freq, Node left, Node right) {
        this.ch = '\0';
        this.freq = freq;
        this.left = left;
        this.right = right;
    }
}

class Huffman {

    public static void buildHuffmanTree(Map<Character, Integer> freqMap) {
        PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingInt(n -> n.freq));

        // Step 1: Create leaf nodes and add to min-heap
        for (Map.Entry<Character, Integer> entry : freqMap.entrySet()) {
            pq.offer(new Node(entry.getKey(), entry.getValue()));
        }

        // Step 2: Build the tree
        while (pq.size() > 1) {
            Node left = pq.poll();
            Node right = pq.poll();
            Node merged = new Node(left.freq + right.freq, left, right);
            pq.offer(merged);
        }

        Node root = pq.poll();
        printCodes(root, "");
    }

    // Step 3: Traverse tree and print codes
    private static void printCodes(Node root, String code) {
        if (root == null) return;

        if (root.ch != '\0') {
            System.out.println(root.ch + ": " + code);
        }

        printCodes(root.left, code + "0");
        printCodes(root.right, code + "1");
    }

    public static void main(String[] args) {
        Map<Character, Integer> freqMap = new HashMap<>();
        freqMap.put('a', 5);
        freqMap.put('b', 9);
        freqMap.put('c', 12);
        freqMap.put('d', 13);
        freqMap.put('e', 16);
        freqMap.put('f', 45);

        buildHuffmanTree(freqMap);
    }
}
```

---

# Output (Example)

The output might look like:

```
f: 0  
c: 100  
d: 101  
a: 1100  
b: 1101  
e: 111  
```

---

# Time and Space Complexity

* **Time**: O(n log n), where n \= number of unique characters (due to heap operations)

* **Space**: O(n), for the tree and code map

---

# Applications

* File compression (ZIP, GZIP)

* JPEG, PNG

* Real-time communication compression (Morse Code logic is similar\!)

---


