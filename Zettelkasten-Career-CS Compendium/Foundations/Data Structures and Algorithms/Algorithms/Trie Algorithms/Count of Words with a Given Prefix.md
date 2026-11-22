---
tags:
  - FORMAT
  - Algorithm
---
Given a prefix, return the number of words in the Trie that start with that prefix.

Unlike autocomplete, which returns the actual matching words, this operation only counts how many words match.

# Explanation
## Used in:

* Auto-suggestion systems
* Analytics (e.g., how many queries started with "re")
* Word frequency tracking

## TrieNode Modification

To efficiently count matching words, each `TrieNode` must store an additional field:

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
    int prefixCount = 0; // Number of words passing through this node
}
```

* `prefixCount++` during insertion for every node visited.
* Decrement during deletion if implemented.

## Algorithm Steps

### During Insert
* For each character:
  * If child is `null`, create a new `TrieNode`
  * Move to the child
  * Increment `prefixCount`

### During Count Query

1. Start at `root`
2. Traverse down the prefix:
   * If any node is missing, return 0
   * Move to next child
3. After reaching last character in prefix, return its `prefixCount`

## Time Complexity
* **Insert**: O(`L`) where `L` is length of word
* **Query**: O(`P`) where `P` is length of prefix
* **Space**: Extra `int` per node

## State Walkthrough
#TODO

# Example code
```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null)
                current.children[index] = new TrieNode();
            current = current.children[index];
            current.prefixCount++;
        }
        current.isEndOfWord = true;
    }

    public int countWordsStartingWith(String prefix) {
        TrieNode current = root;
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null)
                return 0;
            current = current.children[index];
        }
        return current.prefixCount;
    }
}
```
