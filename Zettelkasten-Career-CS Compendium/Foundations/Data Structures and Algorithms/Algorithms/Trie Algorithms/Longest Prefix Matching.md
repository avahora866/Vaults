---
tags:
  - Algorithm
---
Given a query string, find the **longest prefix** in the Trie that matches the **start** of the query.
This is often used in:

* IP routing (longest prefix match)
* Autocomplete for longest valid root/prefix
* Dictionary-based replacements

# Explanation
## TrieNode Structure

You can use the standard `TrieNode`:

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}
```

## Algorithm Steps

1. Start from the root node.
2. Iterate over characters in the query.
3. For each character:

   * If child node exists, move to it and append character to result.
   * If `isEndOfWord == true`, store current position as latest valid match.
   * If child doesn't exist, break early.
4. Return the longest valid prefix (up to latest `isEndOfWord`).

## Edge Cases

| Case                      | Behavior                               |
| ------------------------- | -------------------------------------- |
| Query shorter than prefix | Returns "" if no `isEndOfWord` reached |
| Query equals a word       | Returns full word                      |
| Query not in Trie         | Returns `""`                           |


## Time Complexity
* **Time**: O(`L`), where `L` is length of query string
* **Space**: O(`1`) if result stored in primitive form (or O(L) for result string)


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
        }
        current.isEndOfWord = true;
    }

    public String longestPrefixMatch(String query) {
        TrieNode current = root;
        StringBuilder matched = new StringBuilder();
        int lastMatchedLength = 0;

        for (int i = 0; i < query.length(); i++) {
            char ch = query.charAt(i);
            int index = ch - 'a';
            if (current.children[index] == null)
                break;
            matched.append(ch);
            current = current.children[index];
            if (current.isEndOfWord) {
                lastMatchedLength = matched.length();
            }
        }

        return matched.substring(0, lastMatchedLength);
    }
}
```
