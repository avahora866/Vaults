---
tags:
  - Algorithm
---
Given a prefix string, return all words in the Trie that start with that prefix.
This is the basis for **autocomplete** systems.
# Explanation
## TrieNode Structure
You can use the standard implementation:

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}
```

## Algorithm Overview
1. **Traverse the prefix**:
   * Start from `root`.
   * For each character in the prefix, move to the corresponding child.
   * If any child is missing → return empty list.
2. **From the last matched node**, run a DFS:
   * Build words using the given prefix.
   * Collect all completions by exploring down the tree from that node.

## Edge Cases

| Case               | Behavior                  |
| ------------------ | ------------------------- |
| Empty prefix       | Returns all words in Trie |
| Prefix not in Trie | Returns empty list        |
| Trie is empty      | Returns empty list        |


## Time Complexity
* **Time**:
  * Prefix traversal: O(P)
  * DFS to collect suggestions: O(N·L)
    * `N`: number of matching words
    * `L`: average word length
* **Space**:
  * O(N·L) for storing the suggestions

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

    public List<String> suggestWords(String prefix) {
        List<String> result = new ArrayList<>();
        TrieNode current = root;

        // Traverse the prefix
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null)
                return result; // No suggestions
            current = current.children[index];
        }

        // Run DFS from prefix node
        dfs(current, new StringBuilder(prefix), result);
        return result;
    }

    private void dfs(TrieNode node, StringBuilder path, List<String> result) {
        if (node.isEndOfWord)
            result.add(path.toString());

        for (int i = 0; i < 26; i++) {
            TrieNode child = node.children[i];
            if (child != null) {
                path.append((char) ('a' + i));
                dfs(child, path, result);
                path.deleteCharAt(path.length() - 1); // backtrack
            }
        }
    }
}
```
