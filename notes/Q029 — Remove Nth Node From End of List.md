# Q029 — Remove Nth Node From End of List

**LeetCode:** [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
**Difficulty:** Medium  
**Pattern:** Linked List / Two Pointers / Dummy Node

## 1. Problem

Remove the `n`th node from the end of a singly linked list and return the new head.

**Example:**

```text
Input:  1 → 2 → 3 → 4 → 5, n = 2
Output: 1 → 2 → 3 → 5
```

**Goal:** O(L) time and O(1) extra space, where L is the list length.

## 2. My Solution — Two Pointers

**Key Idea:**
1. Use a dummy node to handle head deletion.
2. Start `fast` at `head` and `slow` at `dummy`.
3. Move `fast` ahead by `n` steps.
4. Move both pointers until `fast == null`.
5. `slow` now points to the node before the target.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode slow = dummy;
        ListNode fast = head;

        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }

        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        slow.next = slow.next.next;

        return dummy.next;
    }
}
```

**Complexity:**
- Time: **O(L)**
- Extra Space: **O(1)**

**Improvement:** My initial code checked `if (slow.next != null)` before deletion. This is unnecessary because the problem guarantees `1 <= n <= L`.

## 3. Standard / Alternative Solution — Both Start at Dummy

Another common approach initializes both pointers at `dummy`.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode slow = dummy, fast = dummy;

        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }

        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }

        slow.next = slow.next.next;

        return dummy.next;
    }
}
```

**Complexity:** O(L) time / O(1) extra space.

**Important Difference:**

| Fast Starts At | Advance | Stop When |
|---|---|---|
| `head` | `n` steps | `fast == null` |
| `dummy` | `n` steps | `fast.next == null` |
| `dummy` | `n + 1` steps | `fast == null` |

All three approaches position `slow` immediately before the node to remove.

## 4. Keywords

- **Pattern:** Two Pointers / Fixed Gap
- **Dummy Node:** Handles head deletion without special cases.
- **Fast Pointer:** Moves ahead to create a fixed gap.
- **Slow Pointer:** Stops at the target's predecessor.
- **Deletion:** `slow.next = slow.next.next`
- **Loop Invariant:** The pointer gap remains constant during synchronized movement.
- **Edge Cases:** Single node, remove head, remove tail.
- **Complexity:** O(L) time / O(1) extra space.

**Recall:** Use a dummy node, move fast ahead by `n` steps, advance both pointers, then bypass the target node.

**Coding Lesson:** Choose pointer initialization and loop conditions carefully to eliminate unnecessary `null` checks.