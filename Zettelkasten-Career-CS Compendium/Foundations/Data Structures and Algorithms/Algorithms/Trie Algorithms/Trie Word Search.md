---
tags:
  - Algorithm
---
The goal of the **search algorithm** is to determine if a given word exists in a Trie. This requires following a path from the **root** through the **nodes representing each character** in the word. If the full path exists and the final node is marked as the **end of a word**, then the word is present.

# Explanation
## Assumptions
* Trie nodes contain:
  * An array of size 26 (`a`–`z`)
  * A boolean `isEndOfWord` flag
* Characters are assumed to be lowercase `a` to `z`

## Algorithm Steps
1. Start from the `root` node.
2. For each character `ch` in the word:
   * Calculate index: `int index = ch - 'a'`
   * If `current.children[index] == null`, the word doesn't exist → return `false`
   * Move to `current.children[index]`
3. After all characters, return `current.isEndOfWord`

## Edge Cases
| Case             | Behavior                                           |
| ---------------- | -------------------------------------------------- |
| Empty string     | Depends on implementation; usually returns `false` |
| Prefix only      | Returns `false` unless marked `isEndOfWord`        |
| Word not in Trie | Returns `false`                                    |
| Case sensitivity | Must lowercase input or validate                   |

## Time Complexity
* **Time Complexity**: O(`L`) where `L` = length of the word
* **Space Complexity**: O(1) (no extra space used beyond pointers)


## State Walkthrough
#TODO

# Example code
```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Insert function here if needed

    // Search for a full word in the trie
    public boolean search(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return false; // Character path doesn't exist
            }
            current = current.children[index];
        }
        return current.isEndOfWord; // Only return true if it's the end of a valid word
    }
}
```
