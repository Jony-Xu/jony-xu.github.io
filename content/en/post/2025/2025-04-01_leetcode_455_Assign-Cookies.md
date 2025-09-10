---
title: Assign Cookies
description: leetcode No.455
date: 2025-04-01
categories:
    - Development
tags:
---

## leetcode No.455

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

## Forthright Solution

```py
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        children = sorted(g)
        cookies = sorted(s)
        index_c = len(children)-1
        res = 0
        for c in cookies[::-1]:
            print(c)
            while index_c >= 0:
                print(children[index_c])
                if children[index_c] > c:
                    index_c -=1
                else:
                    res +=1
                    index_c -=1
            break
        return res
```

failed situation:
g = [1,2,3], s=[3]


Fix:

```py
def findContentChildren(self, g: List[int], s: List[int]) -> int:
    children = sorted(g)
    cookies = sorted(s)
    res = 0
    child = len(children)-1
    cookie = len(cookies)-1
    while child >= 0 and cookie>=0:
        if cookies[cookie] >= children[child]:
            res += 1
            cookie-=1
            child-=1
        else:
            child-=1
    return res
```

## Research
