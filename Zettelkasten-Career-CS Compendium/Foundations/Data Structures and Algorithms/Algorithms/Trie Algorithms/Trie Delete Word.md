---
tags:
  - Algorithm
---
Deleting a word from a Trie removes its `isEndOfWord` marker. If nodes along its path are **not shared** with other words, they can also be **safely deleted** to free space. The deletion must be done **recursively**, working backward from the word’s last character to the root.

# Explanation
Given a Trie and a word `word`, remove the word from the Trie:

* If the word doesn’t exist → do nothing
* If it's a prefix for other words → only unset `isEndOfWord`
* If it's not a prefix → remove unused nodes

## Assumptions

* Lowercase a–z only
* Trie uses:

  ```java
  class TrieNode {
      TrieNode[] children = new TrieNode[26];
      boolean isEndOfWord = false;
  }
  ```

## Algorithm Steps

Recursive function `delete(TrieNode node, String word, int depth)`:

1. Base case:

   * If `node == null`, return `false`
   * If `depth == word.length()`:

     * If `isEndOfWord == false`, return `false` (word doesn't exist)
     * Set `isEndOfWord = false`
     * Return `true` if node has no children
2. Recurse:

   * Get index of current char
   * Recur to child node
   * After returning:

     * If child should be deleted → set `node.children[index] = null`
     * Return `true` if node has no children and is not `isEndOfWord`

## Edge Cases

| Case                   | Behavior                    |
| ---------------------- | --------------------------- |
| Word not in Trie       | No-op                       |
| Word is prefix         | Only unset `isEndOfWord`    |
| Word has no dependents | Clean up trailing nodes     |
| Empty string           | Usually rejected or ignored |

## Time Complexity
* **Time Complexity**: O(`L`), where `L` = length of word
* **Space Complexity**: O(`L`) call stack due to recursion

## State Walkthrough
#TODO

# Example code
```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void delete(String word) {
        delete(root, word, 0);
    }

    private boolean delete(TrieNode current, String word, int depth) {
        if (current == null) {
            return false;
        }

        if (depth == word.length()) {
            if (!current.isEndOfWord) {
                return false; // Word doesn't exist
            }
            current.isEndOfWord = false;

            // If node has no children, it can be deleted
            return isNodeEmpty(current);
        }

        int index = word.charAt(depth) - 'a';
        boolean shouldDeleteChild = delete(current.children[index], word, depth + 1);

        if (shouldDeleteChild) {
            current.children[index] = null;

            // If current is not end of another word and has no other children
            return !current.isEndOfWord && isNodeEmpty(current);
        }

        return false;
    }

    private boolean isNodeEmpty(TrieNode node) {
        for (TrieNode child : node.children) {
            if (child != null) return false;
        }
        return true;
    }
}
```
