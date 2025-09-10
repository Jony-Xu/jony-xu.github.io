---
title: Generate Parentheses
description: leetcode No.22
date: 2025-03-26
categories:
    - Development
tags:
---

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Example 1:
Input: n = 3
Output: `["((()))","(()())","(())()","()(())","()()()"]`
Example 2:
Input: n = 1
Output: `["()"]`


## 1. Forthright Solution

ideas:
for each exists composition,
eg. when n = 2, all availble strings are ["()()", "(())"]
There are 5 position to insert a new bracket, index from 0 - 5.
1. ["()(())","(()())","((()))","(()())","(())()"],
2. "()(())","(()())","((()))","(()())","(())()"
finally remove the duplicate strings we will get the answer.

```py
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        # check combine
        def process(sub_comb):
            res = {}
            for item in sub_comb:
                for index in range(len(item)+1):
                    if index == 0:
                        temp = ("()"+ item)
                    elif index == len(item):
                        temp = (item + "()")
                    else:
                        temp = (item[0:index] + "()" + item[index:])
                    if temp not in res:
                        res[temp] = 1
            return list(res.keys())

        if n == 1:
            return ["()"]
        sub_comb = self.generateParenthesis(n-1)
        return process(sub_comb)
```


## Research

### Rules for Valid Parentheses:
1. At any point, the number of closing parentheses cannot exceed the number of opening ones.
2. The total number of opening parentheses cannot exceed n.
3. The total number of closing parentheses cannot exceed n.

## Code Solution with Backtracking

```python
def generateParenthesis(n):
    """
    Generate all combinations of well-formed parentheses given n pairs.
    
    Args:
        n: Number of pairs of parentheses
        
    Returns:
        List of all valid combinations
    """
    result = []
    
    def backtrack(s, open_count, close_count):
        """
        Recursive backtracking function to build valid parentheses strings.
        
        Args:
            s: Current string being built
            open_count: Number of opening parentheses used so far
            close_count: Number of closing parentheses used so far
        """
        # Base case: if we've used all n pairs, we have a complete valid combination
        if len(s) == 2 * n:
            result.append(s)
            return
        
        # We can add an opening parenthesis if we haven't used all n
        if open_count < n:
            backtrack(s + "(", open_count + 1, close_count)
        
        # We can add a closing parenthesis if it doesn't exceed opening count
        # This ensures validity - we never close more than we've opened
        if close_count < open_count:
            backtrack(s + ")", open_count, close_count + 1)
    
    # Start backtracking with empty string and no parentheses used yet
    backtrack("", 0, 0)
    return result
```

## Time and Space Complexity
- **Time Complexity**: O(4^n / √n) - This is the nth Catalan number, which is the number of valid combinations.
- **Space Complexity**: O(n) for the recursion stack, not counting the output which is O(4^n / √n) * n.

## Key Insights
1. We maintain validity by adding a closing parenthesis only if there are more opening ones.
2. The backtracking approach builds all valid combinations systematically.
3. The state is tracked by counting opening and closing parentheses used so far.

This solution ensures we generate all possible valid combinations without duplicates.
