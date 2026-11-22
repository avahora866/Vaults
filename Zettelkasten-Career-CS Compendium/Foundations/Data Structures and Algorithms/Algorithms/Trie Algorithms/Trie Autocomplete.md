---
tags:
  - Algorithm
---
**Autocomplete** uses a Trie to return all words starting with a given prefix. It combines two operations:

1. Traverse the Trie to the end of the prefix.
2. From there, collect all suffixes that complete valid words.

This is useful for search engines, text prediction, and command prompts.

# Explanation
## Assumptions
- TrieNode structure:

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}
```
- Words consist of lowercase `a–z`

## Algorithm Steps

### Step 1: Traverse to Prefix Node
* Start at `root`
* For each character in `prefix`:
  * If corresponding child is `null`, return empty list
  * Move to the child

### Step 2: DFS from Prefix Node
* From the current node, recursively collect all paths that end in `isEndOfWord = true`
* Accumulate characters to build full words

## Edge Cases

| Case           | Result                             |
| -------------- | ---------------------------------- |
| Empty prefix   | All words in Trie                  |
| No match       | Empty list                         |
| Prefix is word | Returns prefix + extensions if any |
| Duplicates     | Avoided by standard insert logic   |


## Time Complexity
* **Time**: O(`P + K`)
  * `P` = length of prefix
  * `K` = total number of characters in matching words
* **Space**: O(`K`) for result list


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

    public List<String> autocomplete(String prefix) {
        List<String> results = new ArrayList<>();
        TrieNode current = root;

        // Step 1: Traverse to end of prefix
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null)
                return results; // No matches
            current = current.children[index];
        }

        // Step 2: DFS from that node
        dfs(current, new StringBuilder(prefix), results);
        return results;
    }

    private void dfs(TrieNode node, StringBuilder path, List<String> results) {
        if (node.isEndOfWord) {
            results.add(path.toString());
        }

        for (int i = 0; i < 26; i++) {
            if (node.children[i] != null) {
                path.append((char) ('a' + i));
                dfs(node.children[i], path, results);
                path.deleteCharAt(path.length() - 1);
            }
        }
    }
}
```
