---
title: find-the-index-of-the-first-occurrence-in-a-string
description: leetcode No.28
date: 2025-03-19 14:33:00+0000
categories:
    - Development
tags:
---

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

## 1. Forthright Solution

use a simple example:  
haystack = "hello"  
needle = "ll"  

```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        p_stack = 0
        p_ndl = 0
        start = 0
        for i in range(len(haystack)):
            if haystack[i] == needle[p_ndl]:
                p_stack += 1
                p_ndl += 1
            else:
                start = i
            if p_ndl == len(needle):
                return start
        else:
            return -1
```

> assistant
> Let's analyze your approach. I notice a few things we should think about:
> 
> 1. In your current code, when characters don't match (`else` case), you're setting `start = i`, but what about `p_ndl`? Should it reset?
> 
> 2. Let's test this with a simple example:
> haystack = "hello"
> needle = "ll"
> 
> Can you walk me through how your pointers would move in this case? 
> 
> 3. There seems to be an issue with the `start` variable. When we find a match, we need to return the position where the match began, not where it ended. How might we modify this?
> 
> Would you like to try adjusting your code to address these points?


Sure ,here is the walk through:
1. "h": not match,start change to 1,p_ndl = 0
2. "e": not match,start change to 2,p_ndl = 0
2. "l": matched,start still 2,p_ndl = 1
3. "l": matched,start still 2,p_ndl = 2, return 2

this is code update:
```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        p_ndl = 0
        start = 0
        for i in range(len(haystack)):
            if haystack[i] == needle[p_ndl]:
                p_ndl += 1
            else:
                p_ndl = 0
                start = i+1
            if p_ndl == len(needle):
                return start
        else:
            return -1
```


> Let's test this updated code with a few cases. I notice something that might need attention:
> 
> 1. When we have a mismatch, you set `start = i+1`, but should we always move the start position forward? 
> 
> Let's test with this example:
> haystack = "mississippi"
> needle = "issi"
> 
> Can you walk through how your current code would handle this case? Pay special attention to:
> - What happens when we find the first 'i'?
> - If we encounter a mismatch after matching some characters, should we really jump to `i+1`, or should we try matching from an earlier position?
> 
> Would you like to try tracing through this example to see if you spot any potential issues?

I can see the issue not, when
haystack = "mississippi"
needle = "issip", current script will return -1.
So if there is a mismatch,I shouldn'd jump to `i+1`,I should try match from an earlier position(if it match the 1st character in needle).

```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        start = 0
        p_ndl = 0
        for i in range(len(haystack)):
            if haystack[i] != needle[p_ndl]:
                start += 1
            else:
                for j in range(len(needle)):
                    if needle[j] == haystack[j+start]:
                        continue
                    else:
                        start += 1
                        break
                else:
                    return start
        else:
            return -1
```

> I see you're trying to modify the approach by adding a nested loop to check for matches. Let's analyze this new approach:
> 
> 1. When you find a potential match, you're using a nested loop with `j+start`. Could this potentially cause any index out of range issues? When might this happen?
> 
> 2. Let's walk through your code with a simple example:
> haystack = "hello"
> needle = "ll"
> 
> Could you trace:
> - How does `start` change?
> - What happens when we find the first 'l'?
> - Is the nested loop handling the matching process efficiently?
> 
> 3. There might be a simpler way to handle this. Instead of using a nested loop, could we maintain our two-pointer approach but with better handling of mismatches?
> 
> Would you like to try refining your solution considering these points?

1. I see there's a potential problem when needle is longer than haystack.
here's the udpate
```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        start = 0
        p_ndl = 0
        for i in range(len(haystack)):
            if haystack[i] != needle[p_ndl]:
                start += 1
            else:
                for j in range(len(needle)):
                    if j+start > len(haystack)-1:
                        return -1
                    if needle[j] == haystack[j+start]:
                        continue
                    else:
                        start += 1
                        break
                else:
                    return start
        else:
            return -1
```

> Good catch on handling the index out of range issue! However, there are still some aspects of the solution that could be improved:
> 
> 1. Let's test with this example:
> haystack = "mississippi"
> needle = "issi"
> 
> When you increment `start` by 1 each time there's a mismatch, you might skip potential matches. For instance:
> - If you find 'i' but the subsequent characters don't match
> - Should you really move `start` by 1, or should you try matching from some other position?
> 
> 2. Also, consider the efficiency:
> - Right now, for each character match, you're checking the entire needle string again
> - Could we keep track of how many characters have matched so far instead?
> 
> Would you like to try modifying your approach to handle these cases more efficiently? Think about how you could use your original two-pointer approach but with better handling of mismatches.

## 2. Research

3 way for Pattern-Matching Algorithms
1. Brute Force
2. The Knuth-Morris-Pratt Algorithm
3. The Boyer-Moore Algorithm

### brute force

```py
def find brute(T, P):
    n, m = len(T), len(P) # introduce convenient notations
    for i in range(n−m+1): # try every potential starting index within T 
        k = 0 # an index into pattern P
        while k < m and T[i + k] == P[k]: # kth character of P matches
            k += 1
        if k == m: # if we reached the end of pattern,
            return i # substring T[i:i+m] matches P
    return −1 # failed to find a match starting with any i
```

worst case : O(nm)

### The Knuth-Morris-Pratt Algorithm

#### 1. the failture function

concept: the failure function `f(k)` is defined as the length of the longest prefix of P that is a suffix of P[1:k+1]
> note that we did not include P[0] here, since we will shift at least one unit
purpose: it tells us how many of the immediately preceding characters can be reused to restart the pattern.

||||||||||||||
|:----:|----|-|-|-|-|-|-|-|-|-|-|-|
|k    |0 | 1 |2| 3 |4 |5 |6 |7 |8| 9| 10| 11  |
|P[k] |a | m |a| l |g |a |m |a |t| i| o |n  |
|f(k) |0 | 0 |1| 0 |0 |1 |2 |3 |0| 0| 0 |0  |

implement:

```py
def compute_kmp_fail(P):
    m = len(P)
    fail = [0] * m
    j = 1
    k = 0
    while j < m:
        if P[j] == P[k]:
            fail[j] = k + 1
            j += 1
            k += 1
        elif k > 0:
            k = fail[k - 1]
        else:
            j += 1
    return fail
```

#### 2. the solution

```py
def find_kmp(T, P):
    """Return the lowest index of T at which substring P begins (or else -1)."""
    n, m = len(T), len(P)
    if m == 0:
        return 0
    fail = compute_kmp_fail(P)
    j, k = 0, 0  # index into text, pattern
    while j < n:
        if T[j] == P[k]:
            if k == m - 1:
                return j - m + 1
            j += 1
            k += 1
        elif k > 0:
            k = fail[k - 1]  # reuse suffix of P[0:k]
        else:
            j += 1
    return -1
```

time performance O(n + m)

#### 3. The Boyer-Moore Algorithm

1. Align the `String` with the `pattern` head and compare them starting at the end.

eg. string: "HERE IS A SIMPLE EXAMPLE",  pattern: "EXAMPLE".

> DuringthetestingofapossibleplacementofPwithin T, a mismatch of text character T[i]=c with the corresponding pattern char- acter P[k] is handled as follows. If c is not contained anywhere in P, then shift P completely past T[i] (for it cannot match any character in P). Other- wise, shift P until an occurrence of character c in P gets aligned with T[i].

reference：
  - Data Structures and Algorithms in Python,by Michael T. Goodrich (Author), Roberto Tamassia (Author)
  - https://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html
