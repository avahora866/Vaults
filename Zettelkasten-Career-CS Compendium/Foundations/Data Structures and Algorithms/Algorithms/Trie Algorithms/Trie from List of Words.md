---
tags:
  - Algorithm
---
Construct a Trie from a given list (or array) of words.
This operation is the first step in most Trie-based problems like:

* Dictionary matching
* Autocomplete
* Word filtering
* Prefix lookups

# Explanation
## TrieNode Structure

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}
```

Each node represents a character. Children are indexed from 0–25 for `'a'`–`'z'`.

## Algorithm Overview

To build the Trie:

1. Create a root node.
2. For each word in the list:
   * Start from the root.
   * For each character in the word:
     * If the corresponding child node is missing, create it.
     * Move to the child node.
   * After inserting all characters, mark the last node with `isEndOfWord = true`.

## Edge Cases

| Case                 | Behavior                       |
| -------------------- | ------------------------------ |
| Empty list           | Trie remains empty             |
| Duplicate words      | Safe: ends at same node        |
| Overlapping prefixes | Shared nodes reduce memory use |

## Time Complexity
* **Time**: O(N·L)
  * `N`: number of words
  * `L`: average word length
* **Space**: O(N·L)
  * For the total number of TrieNodes created

## State Walkthrough
#TODO

# Example code
```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void buildFromList(List<String> words) {
        for (String word : words) {
            insert(word);
        }
    }

    private void insert(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null)
                current.children[index] = new TrieNode();
            current = current.children[index];
        }
        current.isEndOfWord = true;
    }

    // Optional: Add search/suggest/etc. here
}
```
