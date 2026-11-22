---
tags:
  - FORMAT
  - DataStructure
---
A **Trie** (short for *retrieval*) is a **tree-like data structure** used to store a **dynamic set of strings**, usually over an alphabet (e.g. English lowercase letters `a–z`).

* It's designed for **prefix-based retrieval**
* Every node represents a **prefix of one or more strings**
* Commonly used in **autocomplete**, **dictionary lookup**, and **spell checkers**

---

# Trie Structure

## Nodes

Each node typically stores:

* **Links to child nodes** (for each character)
* **Boolean flag** to mark the **end of a word**
* Optional: a character label (for clarity or debugging)

## Root

The root node represents the **empty prefix**.

---

# Example: Storing "cat", "car", and "dog"

```
Root
 ├─ c
 │   └─ a
 │       ├─ t (end)
 │       └─ r (end)
 └─ d
     └─ o
         └─ g (end)
```

Each path from root to a leaf forms a string in the trie.

---

# Node Design (Java)

## Fixed Alphabet (e.g. lowercase a–z)

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26]; // For 'a' to 'z'
    boolean isEndOfWord = false;
}
```

## Flexible Alphabet (e.g. unicode or variable symbols)

```java
import java.util.HashMap;
import java.util.Map;

class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord = false;
}
```

---

# Trie Class (Skeleton)

```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Add methods like insert, search, startsWith, etc.
}
```

---

# Trie Properties

| Property                    | Description                                   |
| --------------------------- | --------------------------------------------- |
| **Space Efficiency**        | Avoids storing duplicate prefixes             |
| **Time for Insert/Search**  | O(`L`) where `L` is word length               |
| **Supports Prefix Queries** | Fast startsWith/auto-complete logic           |
| **Not a BST**               | Nodes are not ordered or compared using keys  |
| **Not Balanced**            | Structure depends on the input set of strings |

---

# Trie Use Cases

| Use Case           | Description                            |
| ------------------ | -------------------------------------- |
| **Autocomplete**   | Suggest completions of a given prefix  |
| **Spell Checkers** | Validate and correct input strings     |
| **IP Routing**     | Prefix-matching for subnet routes      |
| **T9 Keypad**      | Word prediction in mobile keypads      |
| **DNA Search**     | Match DNA subsequences (A, C, G, T)    |
| **Word Games**     | Valid move checking in Boggle/Scrabble |

---

# Memory Tradeoff

Tries can consume more memory than necessary for small data sets, especially if:

* The strings have few shared prefixes
* The character set is large (e.g. full Unicode)

## Optimization Strategies

* Use `Map<Character, TrieNode>` instead of fixed arrays
* Use **compressed tries (radix trees)** to merge single-child paths
* Use **ternary search tries** to reduce space while supporting ordered traversal

---

# Summary

| Feature       | Trie                                      |
| ------------- | ----------------------------------------- |
| Structure     | Tree of characters                        |
| Lookup Time   | O(L) where L = string length              |
| Prefix Search | Fast                                      |
| Memory Usage  | High (can be optimized)                   |
| Key Strength  | Efficient prefix-based storage and search |

---
