---
title: leetcode No.142 Linked List Cycle II
description: leetcode No.142
date: 2025-05-08
categories:
    - Development
tags:
---

## leetcode No.142


## Agent

I'd be happy to help you think through this LeetCode problem about finding the start of a cycle in a linked list! Let's break it down together.

First, let's make sure we understand the problem:
- We have a linked list
- There might be a cycle (where the tail's next pointer points back to some node in the list)
- We need to find where that cycle begins (or return null if there's no cycle)

What do you think might be a good approach to detect if there's a cycle in the linked list?

## User

1. how to detect the circle:use a slow pointer,with one step forward each time, and a fast pointer with 2 or more step each time, if the faster one catch up with the slower one, that means there is the circle.
2. how to find the location that circle starts with:No idea.

## Agent

Great! You've correctly identified a good approach for detecting a cycle using fast and slow pointers. That's often called Floyd's Cycle-Finding Algorithm or the "tortoise and hare" algorithm.

Now let's think about how to find the starting point of the cycle. This is where things get interesting!

Let's break this down step by step:

1. Once the fast and slow pointers meet inside the cycle, we know there's a cycle.
2. But how do we find where the cycle begins?

Let's think mathematically about this problem. Can you think about what might happen if we put one pointer back at the head of the list, and another at the meeting point, and then move them at the same speed (one step at a time)?

## User

currently I've got no idea,please give me some tips

## Agent

Let's think about this together. I'll give you some hints to guide your thinking.

When the fast and slow pointers meet inside the cycle, there's an interesting mathematical property we can use.

Let's define some variables:
- Let's say the distance from the head to the cycle start is 'x'
- The distance from the cycle start to the meeting point is 'y'
- The remaining distance to complete the cycle (from meeting point back to cycle start) is 'z'

When the pointers meet:
- The slow pointer has traveled: x + y
- The fast pointer has traveled: x + y + (some number of complete cycles) + y

Since the fast pointer travels twice as fast as the slow pointer, we know:
2(x + y) = x + y + (some number of complete cycles) + y

Can you try to simplify this equation? What does this tell us about 'x'?

## User

x = (some number of complete cycles)

## Agent

Close! Let's solve this step by step.

We have:
2(x + y) = x + y + (some number of complete cycles) + y

Let's denote the cycle length as (y + z), so one complete cycle is (y + z).
And let's say the fast pointer has gone around n complete cycles.

So our equation becomes:
2(x + y) = x + y + n(y + z) + y

Let's simplify:
2x + 2y = x + 2y + n(y + z)
2x = x + n(y + z)
x = n(y + z)

This means x (the distance from head to cycle start) equals some number of complete cycles.

But we really just need one specific insight: x equals some distance around the cycle starting from the meeting point.

So here's the key idea:
- If we put one pointer at the head
- And another pointer at the meeting point
- Then move both one step at a time
- They will meet at the cycle start!

Does this make sense? Want to try implementing this approach?

## User

I think there is another way to understand:
when 2 pointer meets, mark the fast point has run: f, the slow pointer has run: s, from head to cycle start is x, the length of circle is l.n equals the numbers that faster pointer run extra circles than slow point.
so f=2s, f-s=n*l,  so  s = n*l, so if s move another x distance, it will go to the cycle start.(n*l+x),so that we could put a new pointer at the start and make it with same speed to slow point, if they met , then the place is the cycle start.

## Agent

That's a great way to understand it! You've worked through the mathematical reasoning perfectly.

Let's summarize your approach:
1. When the fast and slow pointers meet, the slow pointer has traveled some distance s
2. We know that f = 2s (fast moved twice as fast)
3. The difference f - s = n*l (where l is the cycle length and n is the number of extra cycles)
4. So s = n*l (the slow pointer has traveled exactly n full cycles)
5. To reach the cycle start from the beginning, we need to travel x distance
6. To reach the cycle start from the meeting point, we need to travel some distance which ends up being x as well (because of the mathematical relationship)

Therefore, if we place one pointer at the head and another at the meeting point, and move them at the same speed, they'll meet at the cycle start!

Would you like to try implementing this algorithm now? Or would you like me to help guide you through writing the code?

## User

yes, please guide me to finish the code.
```py
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head:
            return head
        fast, slow, temp = head, head, head
        while fast.next!=slow.next:
            slow = slow.next
            fast = fast.next.next
        while temp.next!=slow.next:
            slow = slow.next
            temp = temp.next
        return temp

## Agent

Let's analyze your solution and make some corrections. There are a few issues we need to address:

1. In your first loop, we need to check if `fast` and `fast.next` exist before moving forward
2. Your loop condition should check when the pointers meet (not their next values)
3. The second part of the algorithm isn't quite right - after finding the meeting point, we need to reset one pointer to head and move both until they meet

Here's a corrected version:

```/dev/null/solution.py#L1-13
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return None
            
        # Step 1: Detect if there is a cycle
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:  # Found meeting point
                # Step 2: Find the start of the cycle
                slow2 = head
                while slow != slow2:
                    slow = slow.next
                    slow2 = slow2.next
                return slow
                
        return None  # No cycle found
```

Let me explain the changes:

1. We add a check at the beginning for edge cases (empty list or just one node)
2. In the first while loop, we check if `fast` and `fast.next` exist before moving forward
3. We check if the pointers meet after moving them (not before)
4. If they meet, we create a new pointer at the head and move both until they meet
5. If we exit the first loop without finding a meeting point, we return None (no cycle)

Do you understand why these changes make the algorithm work correctly? Would you like me to explain any part of it in more detail?


reference:
- https://ichbinhandsome.github.io/2020/11/06/FastAndSlow/
