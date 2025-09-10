---
title: Search Insert Position
description: leetcode No.35
date: 2025-03-20 16:46:00+0000
categories:
    - Development
tags:
---

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
You must write an algorithm with O(log n) runtime complexity.

## 1. Forthright Solution


> I'll help guide you through solving this problem! Since we need O(log n) runtime complexity, this might suggest a particular approach. What algorithm do you think would be most appropriate for searching in a sorted array with logarithmic time complexity?
> 
> Let's break this down step by step. First, can you tell me what algorithm you think might be suitable here?
> 
> The Binary search method.
> 
> That's exactly right! Binary search is perfect here since we have a sorted array and need O(log n) complexity.
> 
> Before we start coding, let's think about how binary search would work in this case. What makes this problem slightly different from a regular binary search where we're just looking for a target?
> 
> Can you describe what should happen if the target is NOT found in the array? How would we determine where it should be inserted?

The difference:
1. Binary search: Find the index, if not return false.
2. This problem: Find the index,if not, compared to the value in final loop,
if the key is smaller, return p-1, if is larger, return p+1.

> I see your thinking, but let me ask you a question about your approach. Let's consider this example:
> nums = [1,3,5,6] and target = 2
> 
> If we do a binary search and the target isn't found, do we really need to check if the key is smaller/larger and do p-1 or p+1? 
> 
> Think about where our left pointer ends up in binary search when an element isn't found. Could we use this information directly? What does the position of the left pointer tell us about where the element should be inserted?

I get your point, maybe we don't compare, just return the final left point +1 when the loop ended?

> You're getting closer! But let's think about this carefully.
> 
> When the binary search loop ends, why would we need to return left +1? Could we just return left?
> 
> Let's look at a specific example:
> nums = [1,3,5,6], target = 2
> 
> Can you walk me through what happens to the left and right pointers during the binary search, and where they end up? This will help us understand exactly what we should return.

here is the walk through
1. left=0, right=3,compare 2 to numd[(right-left)//2] 2 is smaller=> right-= (right-left)//2
2. left=0, right=2,compare 2 to nums[(right-left)//2] 2 is smaller ==> right-= (right-left)//2
3. left=0, right=1,compare 2 to nums[(right-left)//2],2 is bigger,left+=(right-left)//2
final left=1

> Excellent walkthrough! Now let's analyze what we discovered:
> 
> In your example with target = 2, the left pointer ended at 1, which is exactly where 2 should be inserted between 1 and 3. 
> 
> So what does this tell us about our original question - do we need to add 1 to left? Looking at your walkthrough, it seems like we can just return 'left' directly.
> 
> Would you like to try writing the code for this solution? Remember we need:
> 1. Binary search loop
> 2. Return logic when we find the target
> 3. Return logic when we don't find the target
> 
> Can you give it a try?

```py
def searchInsert(self, nums: List[int], target: int) -> int:
    n = len(nums)
    p,q = 0,n-1
    while p<q:
        m = (q-p)//2
        if target < m:
            q-=m
        elif target > m:
            p+=m
        else:
            return m
    return p+1
```

> Let's analyze your code. There are a few issues we should think about:
> 
> 1. In your code, you're comparing target with 'm' (which is just the middle index). Should we be comparing target with the middle index, or the value at the middle index (nums[m])?
> 
> 2. Your calculation of the middle point 'm = (q-p)//2' might not give us the actual middle. What's the standard way to calculate the middle point between two indices?
> 
> 3. The condition 'p < q' might cause us to miss checking some elements. Should it be 'p <= q'?
> 
> Let's test your code with a simple example:
> nums = [1,3,5,6], target = 5
> 
> Can you walk through how your current code would handle this case? Can you spot any issues and suggest improvements?

sure, I'll fix that.
```py
def searchInsert(self, nums: List[int], target: int) -> int:
    n = len(nums)
    p,q = 0,n-1
    while p<=q:  # avoid miss checking,p <=q
        m = p+ (q-p)//2 # the middle index
        if target < nums[m]:
            q-=m
        elif target > nums[m]:
            p+=m
        else:
            return m
    return p+1
```

> You're getting closer, but there are still a few issues to fix. Let's think about your binary search updates:
> 
> 1. When you do `q-=m` and `p+=m`, you're subtracting/adding the middle index. Is this correct? 
>    - What should happen when target is smaller than nums[m]?
>    - What should happen when target is larger than nums[m]?
> 
> Let's test with an example:
> nums = [1,3,5,6], target = 2
> 
> First iteration:
> - p = 0, q = 3
> - m = 0 + (3-0)//2 = 1
> - nums[m] = 3
> - target(2) < nums[m](3)
> - Your code does: q -= m (q = 3-1 = 2)
> 
> Is this the correct way to update the search space? How should we modify the pointers to properly narrow down our search?

I noticed that, thanks
```py
def searchInsert(self, nums: List[int], target: int) -> int:
    n = len(nums)
    p,q = 0,n-1
    while p<=q:  # avoid miss checking,p <=q
        m = p+ (q-p)//2 # the middle index
        if target < nums[m]:
            q=m
        elif target > nums[m]:
            p=m
        else:
            return m
    return p+1
```

> You're very close! But there's still one small but important issue that could cause an infinite loop. Let's analyze:
> 
> When you update the pointers:
> - If target < nums[m], you do q = m
> - If target > nums[m], you do p = m
> 
> Consider this case:
> nums = [1,3], target = 2
> - First iteration: p = 0, q = 1, m = 0
> - nums[m] = 1
> - target(2) > nums[m](1)
> - p = m (p stays at 0)
> 
> Do you see what might happen here? How can we modify the pointer updates to ensure the search space actually shrinks each time?

I got that, maybe the update for q&p should change to m+1 and m-1,
here is code
```py
def searchInsert(self, nums: List[int], target: int) -> int:
    n = len(nums)
    p,q = 0,n-1
    while p<=q:  # avoid miss checking,p <=q
        m = p+ (q-p)//2 # the middle index
        if target < nums[m]:
            q=m-1
        elif target > nums[m]:
            p=m+1
        else:
            return m
    return p+1
```

> Perfect! Let's verify why this works better:
> 
> 1. When target < nums[m], we move q to m-1 because we know target can't be at m or after it
> 2. When target > nums[m], we move p to m+1 because we know target can't be at m or before it
> 3. The search space reduces by at least one element in each iteration
> 4. When p and q cross, p will end up at the correct insertion point
> 
> Let's verify with a few test cases:
> 1. nums = [1,3,5,6], target = 5 (target exists)
> 2. nums = [1,3,5,6], target = 2 (target doesn't exist)
> 3. nums = [1], target = 0 (target should go at beginning)
> 
> Would you like to walk through any of these test cases to verify the solution works correctly? This would help ensure you fully understand how the algorithm handles different scenarios.

I noticed taht if the target was not found ,no need return p+1 finally,the p already equal m+1 in each loop.
here is the final
```py
def searchInsert(nums: List[int], target: int) -> int:
    n = len(nums)
    p,q = 0,n-1
    while p<=q:  # avoid miss checking,p <=q
        m = p+ (q-p)//2 # the middle index
        if target < nums[m]:
            q=m-1
        elif target > nums[m]:
            p=m+1
        else:
            return m
    return p  # not p+1 since the final step always p=m+1
```

## 2. Research

### Tips for Binary Search

1. **sorted** : Ensure the array is sorted.
2. **Edge Cases**: Consider edge cases such as an empty array, an array with one element, or the target value being at the beginning or end of the array.
3. **Iterative vs. Recursive**: Binary search can be implemented iteratively or recursively. Choose the one that fits your needs. Iterative is generally preferred for its simplicity and to avoid potential stack overflow issues with recursion.
4. **Indices Management**: Be careful with the calculation of the middle index to avoid overflow.
5. **Return Values**: Decide what to return if the target is not found. Typically, you return `-1` or `None`.
6. **Performance**: Binary search has a time complexity of O(log n), making it very efficient for large datasets.


#### Iterative Binary Search

```python
def binary_search_iterative(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1  # Target not found

# Example usage
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
target = 5
result = binary_search_iterative(arr, target)
print(f"Element found at index: {result}")
```

#### Recursive Binary Search

```python
def binary_search_recursive(arr, target, left, right):
    if left > right:
        return -1  # Target not found
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)

# Example usage
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
target = 5
result = binary_search_recursive(arr, target, 0, len(arr) - 1)
print(f"Element found at index: {result}")
```
