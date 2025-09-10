---
title: Egg Drop Problem
description: leetcode No.455
date: 2025-04-02
categories:
    - Development
tags:
---

Egg drop. Suppose that you have an n-story building (with floors 1 through m) and plenty 1 of eggs. An egg
breaks if itis dropped from floor T or higher and does not break otherwise. Your goalis to devise a strategy to
determine the value of T given the following limitations on the number of eggs and tosses

Version 0: 1 egg， ≤ T tosses.
Version 1:~ 1lg n eggs and  ~1 lg n tosses.
Version 2: ~lg T eggs and ~ 2lg T tosses.
Version 3: 2 eggs and 2*sqrt(n) tosses.
Version 4: 2 eggs and ≤ c*sqrt(T) tosses for some fixed constant c.

# Egg Drop Problem Solutions

This is a classic "egg dropping" problem. The problem is described as: There is an n-floor building (floor numbers from 1 to n) and several eggs. There is a critical floor T. Eggs dropped from floors T or higher will break, while eggs dropped from floors below T will not break. The goal is to find a strategy to determine the threshold value T while meeting the limitations of different versions on the number of eggs and the number of times eggs are thrown.

Let's analyze each version of the egg drop problem to determine strategies for finding the threshold floor T.

## Version 0: 1 egg, ≤ T tosses

**Strategy**: Linear search starting from floor 1.
- Start from floor 1 and move up one floor at a time
- Try floor 1, then floor 2, and so on until the egg breaks
- If the egg breaks at floor k, then T = k
- If we reach floor n without breaking, then T > n

**Analysis**:
- Worst case: T tosses (when T = n)
- This is optimal with 1 egg since we need to check every floor below T

## Version 1: ~lg n eggs and ~lg n tosses

**Strategy**: Binary search
- First try floor n/2
- If the egg breaks, search the lower half (floors 1 to n/2-1)
- If the egg doesn't break, search the upper half (floors n/2+1 to n)
- Continue this binary division until T is found

**Analysis**:
- Each toss eliminates half of the remaining floors
- Requires lg n tosses in the worst case
- Requires lg n eggs in the worst case (if we're unlucky and break an egg at each step)

## Version 2: ~lg T eggs and ~2lg T tosses

**Strategy**: Modified binary search that's more careful near T
- First use binary search to find a power of 2, say 2^k, such that the egg breaks at 2^k but not at 2^(k-1)
- This takes about lg T tosses and lg T eggs in the worst case
- Then binary search between 2^(k-1) and 2^k to find T exactly
- This takes another lg T tosses

**Analysis**:
- Total tosses: ~2lg T
- Total eggs needed: ~lg T

## Version 3: 2 eggs and 2*sqrt(n) tosses

**Strategy**: Two-phase approach with uniform jumps
1. With first egg, check floors sqrt(n), 2*sqrt(n), 3*sqrt(n), etc.
2. When the egg breaks at k*sqrt(n), use the second egg to check floors linearly from (k-1)*sqrt(n)+1 to k*sqrt(n)-1

**Analysis**:
- First phase takes at most sqrt(n) tosses
- Second phase takes at most sqrt(n) tosses
- Total tosses: ≤ 2*sqrt(n)

## Version 4: 2 eggs and ≤ c*sqrt(T) tosses

**Strategy**: Decreasing step sizes approach
- Start with steps of size x, then x-1, then x-2, and so on
- For this to cover n floors: x + (x-1) + (x-2) + ... + 1 ≥ n
- This sum equals x(x+1)/2, so we need x(x+1)/2 ≥ n
- Solving for x, we get x ≈ sqrt(2n)
- Use first egg to check floors x, x+(x-1), x+(x-1)+(x-2), etc.
- When it breaks, use second egg for linear search in the last interval

**Analysis**:
- If T is the threshold, the worst case happens when the first egg breaks at last possible drop
- This gives us approximately sqrt(2T) tosses in total
- So c ≈ sqrt(2) in this version

Each of these strategies is optimal for the given constraints on eggs and tosses.
