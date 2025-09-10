---
title: Binary Subarrays With Sum
description: leetcode No.930
date: 2025-03-24 21:46:00+0000
categories:
    - Development
tags:
---

leetcode 930: Binary Subarrays With Sum

Given a binary array nums and an integer goal, return the number of non-empty subarrays with a sum goal.

A subarray is a contiguous part of the array.


Example 1:

Input: nums = [1,0,1,0,1], goal = 2
Output: 4
Explanation: The 4 subarrays are bolded and underlined below:
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
Example 2:

Input: nums = [0,0,0,0,0], goal = 0
Output: 15

## 1. Forthright Solution

> Let me help you think about this differently. While recursion could be one approach, there might be a more efficient way.
> 
> Let's analyze your understanding:
> 1. You correctly identified that [1,0] and [0,1] are subarrays with sum = 1
> 2. You're suggesting to check all possible lengths. That's one way, but it might not be the most efficient.
> 
> Let me ask you: Instead of recursion, could we use a different technique? Consider these questions:
> 1. If we're looking for subarrays with a specific sum, how could we use prefix sums to help us?
> 2. In the example [1,0,1], what would the prefix sums array look like?
> 
> Would you like to explore this approach?

1. 
if cur sums = goal:
  all_sums = prefix_sums + 1
else:
  all_sums = prefix_sums

2. prefix sums array are [1,0] and [0,1]


> Let me help clarify prefix sums, as I think there might be some confusion.
> 
> For an array [1,0,1], the prefix sums would be:
> - After 1st element: 1
> - After 2nd element: 1+0 = 1
> - After 3rd element: 1+0+1 = 2
> 
> So prefix sums array would be [1,1,2]
> 
> Let's think about this:
> 1. If we want to find subarrays with a specific sum, how can we use these prefix sums?
> 2. For example, if our goal is 1, and at position i we have a prefix sum of 3, what prefix sum should we have seen before to make a valid subarray with sum 1?
> 
> Would you like to try working out this logic with the original example [1,0,1,0,1] with goal = 2?
> 
> the prefix sum : [1,1,2,2,3]
> 
> Good! You've correctly calculated the prefix sums for [1,0,1,0,1].
> 
> Now let's think about how to use these prefix sums to find subarrays with sum = 2:

> 1. If at position i, we have a prefix sum of say, 3, what prefix sum should we have seen before to make a subarray with sum = 2?
> 
> 2. For example:
>    - If current prefix sum is 3
>    - And we want a subarray sum of 2
>    - What previous prefix sum would we need to have seen? (Think: current_sum - goal = ?)
> 
> Can you try to work out this relationship? This will help us count the valid subarrays.

The previous prefix sum would <=3

> Not exactly. Let me help you think about this more precisely:
> 
> If at a position we have a prefix sum of 3, and we want a subarray with sum = 2:
> - Current prefix sum = 3
> - Goal sum = 2
> - We need to find previous prefix sums of: 3 - 2 = 1
> 
> This is because:
> - If we have a prefix sum of 3 at position i
> - And we find a previous position j with prefix sum 1
> - Then the subarray from (j to i) will have sum: 3 - 1 = 2 (our goal)
> 
> Let's look at your prefix sums array [1,1,2,2,3]:
> 1. When we're at prefix sum 2, what previous prefix sum would we need to find to make a subarray of sum 2?
> 2. When we're at prefix sum 3, what previous prefix sum would we need to find to make a subarray of sum 2?
> 
> Can you try answering these questions?

1. 2-2=0,so from the beginning to current prefix sum we nned find;
2. 3-2=1, so there are two type of prefix we should notice,first is prefix<=2, second is prefix from 1 to 3.

> You're getting closer, but let me help clarify:
> 
> 1. For prefix sum 2:
>    - We need to find previous prefix sums of: 2 - 2 = 0
>    - So we look for positions with prefix sum 0 before this position
>    
> 2. For prefix sum 3:
>    - We need to find previous prefix sums of: 3 - 2 = 1
>    - So we look for positions with prefix sum 1 before this position
> 
> The key pattern is:
> - At each position i, we look for previous positions with prefix sum = (current_prefix_sum - goal)
> 
> For your prefix sums [1,1,2,2,3]:
> - Can you count how many times 1 appears?
> - When we're at prefix sum 3, how many previous positions had prefix sum 1?
> - When we're at prefix sum 2, how many previous positions had prefix sum 0?
> 
> Would you like to try implementing this using a dictionary/hashmap to count prefix sums?

1. When we're at prefix sum 3, two position has prefix sum 1
2. When we're at prefix sum 2, 0 position has prefix sum 0

> Good! You're understanding it well now! 
> 
> Let's organize what we've learned:
> 1. We can use a hashmap to keep track of the count of each prefix sum
> 2. At each position, we look for (current_prefix_sum - goal) in our hashmap
> 
> Would you like to try writing the code? Here's a structure to start with:

```python
def numSubarrays(nums, goal):
    # Initialize variables
    # prefix_sum starts at 0
    # create a hashmap to store count of prefix sums
    
    # iterate through nums
        # update prefix_sum
        # if (prefix_sum - goal) exists in hashmap, add to result
        # update hashmap with current prefix_sum
```

> Would you like to try filling in this structure?


```py
def numSubarrays(nums, goal):
    # Initialize variables
    prefix_sum = []
    sum = 0
    # prefix_sum starts at 0
    for i in nums:
        sum+=i
        prefix_sum.append(sum)
    
    # create a hashmap to store count of prefix sums
    hash_map = {}
    for i in prefix_sum:
        if i not in hash_map:
            hash_map[i] = 1
        hash_map[i] += 1
    
    # iterate through nums
    result = 0
    for i in range(len(prefix_sum)-1, -1,-1):
        # update prefix_sum
        # if (prefix_sum - goal) exists in hashmap, add to result
        if (prefix_sum[i] - goal) in hash_map:
            result += hash_map[prefix_sum[i] - goal]
        # update hashmap with current prefix_sum
    return result
```

> You're on the right track, but there are a few issues to think about:
> 
> 1. In your current approach, you're creating the prefix sums first, then counting them in a hashmap, and then checking backward. Can this cause any problems? 
>    - What happens if we count all prefix sums first before checking?
>    - Could we double count some subarrays?
> 
> 2. Let's look at the case [1,0,1,0,1] with goal = 2:
>    - What should happen when we have prefix sum = 0?
>    - Should we initialize our hashmap with anything?
> 
> Let's modify the approach to count as we go. Here's a structure:

```python
def numSubarrays(nums, goal):
    curr_sum = 0
    count = {0: 1}  # Why do you think we initialize with {0: 1}?
    result = 0
    
    for num in nums:
        # Update curr_sum
        # Check if (curr_sum - goal) exists in count
        # Update count with current curr_sum
```

> Can you:
> 1. Explain why we initialize count with {0: 1}?
> 2. Try completing this modified version?
> 

Yes, the first index of the arr is 1, and index start from 0,
so we initialize with {0:1}

the modified code
```python
def numSubarrays(nums, goal):
    curr_sum = 0
    count = {0: 1}  # Why do you think we initialize with {0: 1}?
    result = 0
    
    for num in nums:
        # Update curr_sum
        curr_sum += num
        if curr_sum in count.keys():
            count[curr_sum] += 1
        else:
            count[curr_sum] = 1
        # Check if (curr_sum - goal) exists in count
        if (curr_sum - goal) in count.keys():
        # Update count with current curr_sum
            result +=  count[curr_sum]
```

> You're getting closer, but there's still a small but important error in your logic. Let's think about it:
> 
> 1. When checking for subarrays with sum = goal, at each position with curr_sum:
>    - We're looking for previous positions where: curr_sum - goal
>    - But in your code, you're adding count[curr_sum] to result
>    - Should it be count[curr_sum] or count[curr_sum - goal]?
> 
> 2. Let's walk through an example with [1,0,1] and goal = 1:
>    - Start: count = {0: 1}
>    - After 1: curr_sum = 1, count = {0: 1, 1: 1}
>    - After 0: curr_sum = 1, count = {0: 1, 1: 2}
>    - After 1: curr_sum = 2, count = {0: 1, 1: 2, 2: 1}
> 
> When curr_sum = 2, what previous prefix sum are we looking for to get a subarray of sum 1?
> 
> Would you like to modify your code based on this?

I noticed that, thanks.
```python
def numSubarrays(nums, goal):
    curr_sum = 0
    count = {0: 1}  # Why do you think we initialize with {0: 1}?
    result = 0
    
    for num in nums:
        # Update curr_sum
        curr_sum += num
        if curr_sum in count.keys():
            count[curr_sum] += 1
        else:
            count[curr_sum] = 1
        # Check if (curr_sum - goal) exists in count
        if (curr_sum - goal) in count.keys():
        # Update count with current curr_sum
            result += count[curr_sum - goal]
```


There is a situation not passed.
[0,0,0,0,0]
should return 15


> I see the issue in your code. The problem is in how you're updating the count dictionary and calculating the result. Let's fix it:
> 
> The main issue is that you're first updating the count for the current sum and then checking if (curr_sum - goal) exists, which means you're counting the current subarray twice when curr_sum - goal equals curr_sum (happens when goal = 0).

Here's the corrected code:

```python
def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
    curr_sum = 0
    count = {0: 1}  # Initialize with {0: 1} to count subarrays starting from index 0
    result = 0
    
    for num in nums:
        # Update curr_sum
        curr_sum += num
        
        # Check if (curr_sum - goal) exists in count
        # This finds all previous positions where a subarray of sum 'goal' ends at current position
        if (curr_sum - goal) in count:
            result += count[curr_sum - goal]
        
        # Update count with current curr_sum
        # This tracks the number of times we've seen each prefix sum
        count[curr_sum] = count.get(curr_sum, 0) + 1
    
    return result
```

The key changes are:
1. First check if (curr_sum - goal) exists before updating the count
2. Use count.get(curr_sum, 0) + 1 for cleaner code instead of checking if key exists
3. Removed redundant .keys() calls when checking dictionary keys

This approach correctly counts the number of subarrays with sum equal to the goal, including when the goal is 0 (as in the [0,0,0,0,0] test case).

For the [0,0,0,0,0] example with goal=0:
- We find all subarrays that have a sum of 0
- This includes subarrays of all possible lengths (1, 2, 3, 4, 5)
- Total: 5 + 4 + 3 + 2 + 1 = 15 subarrays

final:

```py
def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
    curr_sum = 0
    count = {0: 1}  # Initialize with {0: 1} to count subarrays starting from index 0
    result = 0
    
    for num in nums:
        # Update curr_sum
        curr_sum += num
        
        # Check if (curr_sum - goal) exists in count
        # This finds all previous positions where a subarray of sum 'goal' ends at current position
        if (curr_sum - goal) in count:
            result += count[curr_sum - goal]
        
        # Update count with current curr_sum
        # This tracks the number of times we've seen each prefix sum
        count[curr_sum] = count.get(curr_sum, 0) + 1
    
    return result
```
