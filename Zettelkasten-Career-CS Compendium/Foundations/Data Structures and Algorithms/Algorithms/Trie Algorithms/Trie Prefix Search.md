---
tags:
  - Algorithm
---
The **prefix search** operation checks whether any word in the Trie starts with a given prefix. Unlike full-word search, it doesn’t require the final node to be marked as the end of a word.

# Explanation
## Assumptions
* Trie nodes:
  * `TrieNode[] children = new TrieNode[26]`
  * `boolean isEndOfWord`
* Prefix characters are lowercase a–z

## Algorithm Steps

1. Start from the `root` node.
2. For each character `ch` in `prefix`:

   * Compute index: `int index = ch - 'a'`
   * If `current.children[index] == null`, return `false`
   * Move to `current.children[index]`
3. If all characters exist, return `true` (don’t check `isEndOfWord`)

## Edge Cases
| Case             | Result                                |
| ---------------- | ------------------------------------- |
| Empty prefix     | `true` (entire Trie is a valid match) |
| No matching path | `false`                               |
| Exact match      | `true`                                |
| Case sensitivity | Must preprocess or validate input     |

## Time Complexity
* **Time Complexity**: O(`P`) where `P` = length of the prefix
* **Space Complexity**: O(1)


## State Walkthrough
#TODO

# Example code
```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // startsWith function
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        return true;
    }
}
```
