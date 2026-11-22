---
tags:
  - Algorithm
---
Inserting a word into a **Trie** builds or reuses a path from the **root node** through a series of child nodes, one for each character in the word. If a node for a character doesn't exist, it's created. When the final character is added, the corresponding node is marked to indicate the **end of a valid word**.

# Explanation
## Assumptions
* Input: a lowercase English word (`a`–`z`)
* The Trie uses:
  * A root node that doesn't store any character
  * A fixed-size array of 26 children per node
  * A boolean `isEndOfWord` flag per node

## Trie Structure
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26]; // For 'a' to 'z'
    boolean isEndOfWord = false;
}
```

```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
}
```

## Algorithm Steps
1. Start from the `root` node.
2. For each character `ch` in the word:

   * Calculate the index: `int index = ch - 'a'`
   * If `current.children[index] == null`, create a new `TrieNode`
   * Move to `current.children[index]`
3. After the loop, mark the final node with `isEndOfWord = true`

## Edge Cases
* **Empty string**: No characters inserted; you may or may not mark the root as `isEndOfWord`
* **Duplicate insertion**: Has no effect; it's idempotent
* **Uppercase letters**: Should be lowercased or rejected (not handled by this code)
* **Non-alphabet characters**: Must be validated or rejected

## Alternatives
* Use `Map<Character, TrieNode>` if:
  * The alphabet is not fixed
  * Memory is a concern
  * Unicode or special characters are involved



## Time Complexity
* **Time Complexity**: O(`L`) where `L` is the length of the word
* **Space Complexity**: O(`L`) for creating new nodes (worst case, if no prefix matches)


## State Walkthrough
#TODO

# Example code
```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Insert a word into the trie
    public void insert(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                current.children[index] = new TrieNode(); // Create new node
            }
            current = current.children[index]; // Move to child
        }
        current.isEndOfWord = true; // Mark end of word
    }
}
```
