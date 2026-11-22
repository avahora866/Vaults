---
tags:
  - Algorithm
---
Return all complete words stored in a Trie.
This is a **full traversal** operation, used for:

* Debugging contents of a Trie
* Exporting dictionary contents
* Implementing autocomplete with empty prefix

# Explanation
## TrieNode Structure

You can use a standard `TrieNode`:

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}
```

## Algorithm Overview

Use **DFS traversal** from the root node:

1. Initialize an empty result list.
2. Start recursive DFS with an empty `StringBuilder` to track the current path.
3. If `isEndOfWord == true`, add current path to result.
4. Recurse through each non-null child.
5. Return result after traversal.

## Edge Cases

| Case                     | Result               |
| ------------------------ | -------------------- |
| Empty Trie               | Empty list           |
| One word                 | Singleton list       |
| Words with shared prefix | All listed correctly |


## Time Complexity
* **Time**: O(N·L)
  * `N` = number of words
  * `L` = average length of word
* **Space**: O(N·L) for result list


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

    public List<String> getAllWords() {
        List<String> result = new ArrayList<>();
        dfs(root, new StringBuilder(), result);
        return result;
    }

    private void dfs(TrieNode node, StringBuilder path, List<String> result) {
        if (node.isEndOfWord) {
            result.add(path.toString());
        }

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
