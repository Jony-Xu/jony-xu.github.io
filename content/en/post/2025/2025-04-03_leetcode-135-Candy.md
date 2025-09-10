---
title: Candy
description: leetcode No.135
date: 2025-04-03
categories:
    - Development
tags:
---

## leetcode No.135

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
Return the minimum number of candies you need to have to distribute the candies to the children.

 

Example 1:

Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
Example 2:

Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

## Forthright Solution

```py
def candy(ratings: list[int]):
    length = len(ratings)
    res = [1] * length
    for i in range(1,length):
        if ratings[i] > ratings[i-1]:
            res[i] += 1
    for i in range(length-2,-1,-1):
        if ratings[i] > ratings[i+1]:
            res[i] += 1
    return sum(res)
```

wrong answer
[1,3,2,2,1]
answer: 8
expect: 7

Reason: while doing the right-to-left traversal,
if left already > right, then we don't need always change left value to right + 1

```py
def candy(ratings: list[int]):
    length = len(ratings)
    res = [1] * length
    for i in range(1,length):
        if ratings[i] > ratings[i-1]:
            res[i] += 1
    print(res)
    for i in range(length-1, 0,-1):
        if ratings[i] < ratings[i-1]:
            res[i-1] = max(res[i-1], res[i]+1)
    print(res)
    return sum(res)
```

wrong: candy([1,2,87,87,87,2,1])
output: [1, 2, 2, 1, 3, 2, 1]  12
expect: 13

Reason: while traversal left to right, if rate right > left, then res right = res[left]+1

```py
def candy(ratings: list[int]):
    length = len(ratings)
    res = [1] * length
    for i in range(1,length):
        if ratings[i] > ratings[i-1]:
            res[i] = res[i-1]+1
    print(res)
    for i in range(length-1, 0,-1):
        if ratings[i] < ratings[i-1]:
            res[i-1] = max(res[i-1], res[i]+1)
    print(res)
    return sum(res)
```

## Summary

Greedy strategies with comparative relationships do not necessarily require sorting.
