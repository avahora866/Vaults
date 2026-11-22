---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
A **Trie** (also known as a **Prefix Tree**) is a tree-like data structure used to store strings, where each node represents a character of the string. It is particularly useful for operations like autocomplete, dictionary searches, and prefix matching.

Here are the most common **Trie Algorithms** and their typical use cases:

# Insert a Word into the Trie

The basic operation in a Trie is inserting a word. Starting from the root, we insert each character of the word into the Trie, creating new nodes as necessary.
[[Trie Insert Algorithm]]

# Search for a Word

To search for a word, you follow the same procedure as for insertion. If all characters of the word exist in the Trie and the last character is marked as a terminal node, the word is present in the Trie.
[[Trie Word Search]]

# Prefix Search

In addition to searching for a complete word, we can also search for a prefix. This operation checks if a word (or prefix) exists in the Trie. 
[[Trie Prefix Search]]

# Delete a Word

To delete a word from the Trie, you traverse down to the end of the word and mark the terminal node as **not a terminal**. If the node has no other children, you delete the node. This requires a **recursive** approach.
[[Trie Delete Word]]

# Auto-Completion (or Suggest Words)

Given a prefix, this operation finds all the words in the Trie that start with the prefix. It uses depth-first search (DFS) to find all the words that are descendants of the node representing the last character of the prefix.
[[Trie Autocomplete]]

# Count of Words with a Given Prefix

This algorithm counts how many words in the Trie start with a given prefix. The approach is similar to **Prefix Search**, but instead of stopping at the end of the prefix, you continue to traverse all the nodes from the end of the prefix and count the terminal nodes.
[[Count of Words with a Given Prefix]]

# Longest Prefix Matching

This algorithm finds the longest prefix of a string that exists in the Trie. It works by traversing the Trie one character at a time and tracking the longest valid prefix.
[[Longest Prefix Matching]]

# Find All Words in the Trie

This operation retrieves all the words stored in the Trie, which requires performing a DFS or BFS traversal starting from the root node and collecting all the terminal nodes.
[[Find All Words in the Trie]]

# Word Suggestions with Prefix

This operation combines prefix search and auto-completion to suggest words that start with a given prefix. You start by finding the node corresponding to the prefix, and then perform a DFS to gather suggestions.
[[Word Suggestions with Prefix]]

# Trie from List of Words

This algorithm constructs a Trie from a list of words by repeatedly inserting each word into the Trie.
[[Trie from List of Words]]

# Summary Table:

| Algorithm | Time Complexity | Description |
| ----- | ----- | ----- |
| **Insert a Word** | O(m) | Inserts a word into the Trie. |
| **Search for a Word** | O(m) | Searches for a word in the Trie. |
| **Prefix Search** | O(m) | Searches for a prefix in the Trie. |
| **Delete a Word** | O(m) | Deletes a word from the Trie. |
| **Auto-Completion** | O(m \+ k) | Suggests words with a given prefix. |
| **Count of Words with Prefix** | O(m \+ k) | Counts the words starting with a given prefix. |
| **Longest Prefix Matching** | O(m) | Finds the longest prefix in the Trie. |
| **Find All Words** | O(N) | Finds all words in the Trie. |
| **Word Suggestions with Prefix** | O(m \+ k) | Provides suggestions for words with a prefix. |
| **Trie from List of Words** | O(N \* m) | Constructs a Trie from a list of words. |