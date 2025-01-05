---
layout: post
title:  "LeetCode Hard Problem: Word Search II"
date:   2025-01-05 12:04:00 -0500
---
## Problem Statement
Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

## Examples
# Example 1
![character board](/static/img/word_search_ii_example_1.png)

Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], 
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]

# Example 2
![character board](/static/img/word_search_ii_example_2.png)

Input: board = [["a","b"],["c","d"]], 
words = ["abcb"]

Output: []

# Constraints

m == board.length
n == board[i].length
1 <= m, n <= 12
board[i][j] is a lowercase English letter.
1 <= words.length <= 3 * 104
1 <= words[i].length <= 10
words[i] consists of lowercase English letters.
All the strings of words are unique.


## Solution

```python
class TrieNode:

    def __init__(self):
        self.children = {}
        self.isWord = False
    
    def addWord(self, word):
        curr = self

        for char in word:
            if char not in curr.children: # if current character not in children dictionary keys
                curr.children[char] = TrieNode() # add a trie node

            curr = curr.children[char] # grab the key
        
        # after adding all character, make it a word
        curr.isWord = True

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:

        ROWS = len(board)
        COLS = len(board[0])
        result = set()
        visited = set()

        root = TrieNode()

        for word in words:
            root.addWord(word)

        def dfs(r, c, node, word):

            if (r < 0 or c < 0 or r == ROWS or c == COLS) or (r, c) in visited or board[r][c] not in node.children:
                return
            
            visited.add((r, c))
            word += board[r][c]

            node = node.children[board[r][c]] # get the key

            if node.isWord: # if we find a full word, add it to result
                result.add(word)
            
            # otherwise traverse
            dfs(r + 1, c, node, word)
            dfs(r - 1, c, node, word)
            dfs(r, c + 1, node, word)
            dfs(r, c - 1, node, word)

            visited.remove((r, c)) # backtracking
        
        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, root, "")
        
        return list(result)

```

## Some Good Explanations & References

[NeedCode Explanation](https://www.youtube.com/watch?v=asbcE9mZz_U)
[Leetcode Solution](https://leetcode.com/problems/word-search-ii/solutions/)