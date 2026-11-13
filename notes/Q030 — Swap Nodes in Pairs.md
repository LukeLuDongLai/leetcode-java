# Q030 — Swap Nodes in Pairs

**LeetCode:** [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)  
**Difficulty:** Medium  
**Pattern:** Linked List / Dummy Node / Pointer Manipulation

## 1. Problem

Given a singly linked list, swap every two adjacent nodes and return the new head.

**Important:** Swap the actual nodes, not their values.

**Example:**

```text
Input:  1 → 2 → 3 → 4 → 5
Output: 2 → 1 → 4 → 3 → 5
```

## 2. My Solution — Dummy Node + Iteration

**Key Idea:**
- Use a dummy node to avoid special handling when the head changes.
- Maintain `current` as the predecessor of the next pair.
- Reconnect three `next` references to swap two nodes.
- Move `current` to the end of the swapped pair.

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode current = dummy;

        while (current.next != null && current.next.next != null) {
            ListNode first = current.next;
            ListNode second = current.next.next;

            current.next = second;
            first.next = second.next;
            second.next = first;

            current = current.next.next;
        }

        return dummy.next;
    }
}
```

**Pointer Transformation:**

```text
Before:
current → first → second → next

After:
current → second → first → next
                    ↑
               new current
```

**Complexity:**
- Time: **O(n)** — Each node is processed at most once.
- Extra Space: **O(1)**

## 3. Official / Standard Solution

My solution follows the standard iterative approach using a dummy node and three pointer updates.

An alternative is recursion, but it uses O(n) call-stack space.

**Why the iterative approach works:**
- The loop condition guarantees two nodes exist before swapping.
- The dummy node handles changes to the head.
- The predecessor pointer advances to the original first node after each swap.
- An unmatched final node remains unchanged.

## 4. Keywords

- **Pattern:** Linked List / Dummy Node / Pointer Manipulation
- **Dummy Node:** Simplifies operations that may change the head.
- **Predecessor Pointer:** `current` points to the node before the next pair.
- **Three Connections:** `current.next`, `first.next`, `second.next`
- **Loop Invariant:** All pairs before `current` have been correctly swapped.
- **Null Safety:** Check both `current.next` and `current.next.next`.
- **Odd Length:** Leave the final unmatched node unchanged.
- **Complexity:** O(n) time / O(1) extra space.

**Recall:** Use a dummy node, swap the next two nodes by changing three links, then move the predecessor to the original first node.

**Coding Lesson:** When an operation may change the head or needs a predecessor, consider using a dummy node to eliminate special cases.