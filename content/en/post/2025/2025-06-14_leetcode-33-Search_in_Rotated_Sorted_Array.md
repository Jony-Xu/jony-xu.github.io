---
title: Candy
description: leetcode No.33
date: 2025-05-08
categories:
    - Development
tags:
---

## leetcode No.33

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Example 3:

Input: nums = [1], target = 0
Output: -1


I'd be happy to help you work through this problem! This is about searching in a rotated sorted array.

Let's think about this step by step. What is your first thought on how to approach a search in a rotated sorted array? 

The problem mentions we need O(log n) complexity, which usually points to some kind of binary search. Have you considered how binary search might need to be modified for this scenario?

My thought: find the highest value and  re-sort the array to a sorted list, then use the binary search.

That's a start! Let's think about this approach:

If we find the highest value and re-sort the array, that would work, but would take O(n log n) time for sorting, which doesn't meet the O(log n) requirement.

What if we didn't need to actually re-sort the array? Since the array was originally sorted and then rotated, it has a special property - it consists of two sorted segments.

Could we perhaps modify binary search to work directly on this rotated array? What would be different about applying binary search here compared to a regular sorted array?

please remind me  for more details

Of course! Let's dig deeper into this problem.

In a regular sorted array, binary search works because we can compare the middle element with our target and know which half to search next. If the target is smaller, we search the left half; if larger, the right half.

In a rotated sorted array like [4,5,6,7,0,1,2], we have two sorted segments:
1. [4,5,6,7] - first sorted segment
2. [0,1,2] - second sorted segment

The challenge is: when we look at the middle element, we can't immediately tell which half to search.

Let's think about how to determine which segment is sorted. If we look at the first and middle elements:
- If nums[left] ≤ nums[mid], then the left segment is properly sorted
- Otherwise, the right segment is properly sorted

Once we know which segment is sorted, we can check if our target is within that segment's range. If yes, search there; if not, search the other segment.

Does this give you any ideas on how to modify binary search for this problem?

Thanks, I've got the idea for solution

Great! Now that you have an idea, would you like to try implementing the solution? I'd be happy to discuss any specific parts you're thinking about or help review your approach as you go along.

Remember, you'll want to:
1. Use binary search (to meet the O(log n) requirement)
2. Determine which half of the array is sorted
3. Check if the target is in the sorted half's range
4. Search in the appropriate half

Let me know if you'd like to discuss any part of your implementation!

ok, here is the implement of the code.
```
def search(self, nums: List[int], target: int) -> int:
    length = len(nums)
    l, r = 0, length
    m = (l + r)//2
    while l < r:
        if nums[l] < nums[m]:
            # left is sorted
            if target < nums[m]:
                # binary search in left
                r = m
                m = (l+r)//2
            else:
                # search in right
                l = m
        else:
            # right is sorted
            if target < nums[m] or target > nums[r]:
                r = m
            else:
                # binary search in right
                l = m
                m = (l+r)//2
    return nums[r] == target
```


Let's review your code together. I notice a few things we should look at:

1. First, the return statement is comparing `nums[r] == target`, which returns a boolean (True/False) instead of the required index.

2. Let's check your binary search logic:
   - You're setting `r = length` initially (which would be out of bounds)
   - Your loop condition is `l < r`, which is good
   - The way you're updating `l` and `r` needs some refinement

3. When checking which half is sorted, you're comparing `nums[l] < nums[m]`, but there are some edge cases to consider.

Let's think about how to modify this code. What would happen in your current code if the target is in the right half when the left half is sorted? How would you adjust your logic?

Also, can you walk me through how your algorithm would handle Example 1: `nums = [4,5,6,7,0,1,2], target = 0`?

thanks,this is my update of the code
```
def search(self, nums: List[int], target: int) -> int:
    l, r = 0, len(nums)-1
    while l < r:
        m = (l + r)//2
        if nums[l] < nums[m]:
            # left is sorted
            if target < nums[m] and target > nums[l]:
                # target in left
                return self.binary_search(nums[0:m], target)
            else:
                # search in right
                l = m
        else:
            # right is sorted
            if target < nums[m] or target > nums[r]:
                r = m
            else:
                # binary search in right
                return self.binary_search(nums[m:r], target)
    return r

def binary_search(nums, target):
    l,r = 0, len(nums)
    while l < r:
        m = (l+r)//2
        if target == nums[m]:
            return m
        elif target < nums[m]:
            r = m
        else:
            l = m
    return -1
```


Good progress! You're getting closer. Let me help you spot a few issues by walking through your logic:

1. When you call `binary_search(nums[0:m], target)` or `binary_search(nums[m:r], target)`, what index does this return? Will it be the correct index for the original array?

2. In your condition `target < nums[m] and target > nums[l]`, what happens if `target == nums[l]`? Would this target be found?

3. At the end, you return `r` - but what if the target isn't found at all?

Let me ask you this: instead of creating separate subarrays and calling a separate binary search function, could you continue the binary search within the same function by just updating `l` and `r` pointers?

Can you walk me through what happens with your current code when `nums = [4,5,6,7,0,1,2]` and `target = 4`?

here is the update
```
def search(self, nums: List[int], target: int) -> int:
    l, r = 0, len(nums)-1
    while l < r:
        m = (l + r)//2
        if target == nums[m]:
            return m
        if nums[l] < nums[m]:
            # left is sorted
            if target <= nums[m] and target >= nums[l]:
                # target in left
                r = m-1
            else:
                # search in right
                l = m+1
        else:
            # right is sorted
            if target > nums[m] and target <= nums[len(nums)-1]:
                l = m+1
            else:
                # binary search in right
                r = m-1
    return -1
```

Good improvements! You're getting much closer. Let me help you spot a couple of issues:

1. In your right-sorted section, you're using `nums[len(nums)-1]` but you should be using `nums[r]` since `r` represents the current right boundary of your search.

2. There's a logic issue in your right-sorted condition. You're checking `if target > nums[m] and target <= nums[r]`, but what if the target is less than `nums[m]` in the right-sorted section?

3. Your while loop condition is `l < r`, but you return -1 at the end. What happens if the target exists but `l` becomes equal to `r`?

Let me ask: after your while loop ends, shouldn't you check if `nums[l]` equals the target?

Can you trace through your algorithm with `nums = [4,5,6,7,0,1,2]` and `target = 0`? What would happen step by step?

thanks, here is the latest update
```
def search(self, nums: List[int], target: int) -> int:
    if not nums: return -1
    l, r = 0, len(nums)-1
    while l <= r:
        m = (l + r)//2
        if target == nums[m]:
            return m
        if nums[l] <= nums[m]:
            # left is sorted
            if target <= nums[m] and target >= nums[l]:
                # target in left
                r = m-1
            else:
                # search in right
                l = m+1
        else:
            # right is sorted
            if target > nums[m] and target <= nums[r]:
                l = m+1
            else:
                # binary search in right
                r = m-1
    return -1
```


Excellent! Your code looks much better now. Let me check the logic carefully...

You've made great improvements:
- Changed to `l <= r` which is correct
- Added the edge case check for empty array
- Used `nums[l] <= nums[m]` to handle duplicates
- Fixed the right boundary to use `nums[r]`

I think your solution should work correctly now! Let me ask you to trace through one example to make sure you understand the flow:

Can you walk me through what happens with `nums = [4,5,6,7,0,1,2]` and `target = 0`? 

Starting with `l=0, r=6`:
- What would be the value of `m`?
- Which half would be considered sorted?
- Which direction would the search go?

This will help confirm your understanding of the algorithm!
