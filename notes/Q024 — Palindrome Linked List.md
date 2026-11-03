# Q024 — Palindrome Linked List

**LeetCode:** [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)  
**Difficulty:** Easy  
**Pattern:** Linked List / Fast & Slow Pointers / Reversal

## 1. Problem

Given the head of a singly linked list, determine whether it is a palindrome.

**Example:**

```text
Input:  1 → 2 → 2 → 1
Output: true

Input:  1 → 2 → 3
Output: false
```

## 2. My Solution — Reverse First Half While Traversing

**Key Idea:**
1. Move `fast` two steps and `slow` one step.
2. Simultaneously reverse the first half using `prev`.
3. If `fast != null`, skip the middle node (odd length).
4. Compare the reversed first half with the remaining second half.

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;
        ListNode prev = null;

        // Find middle and reverse first half simultaneously
        while (fast != null && fast.next != null) {
            fast = fast.next.next;

            ListNode next = slow.next;
            slow.next = prev;
            prev = slow;
            slow = next;
        }

        // Skip middle node for odd-length lists
        if (fast != null) {
            slow = slow.next;
        }

        // Compare two halves
        while (prev != null) {
            if (prev.val != slow.val) return false;
            prev = prev.next;
            slow = slow.next;
        }

        return true;
    }
}
```

**Complexity:**
- Time: **O(n)**
- Extra Space: **O(1)**

**Important:** Move `fast` before reversing `slow.next`, since both pointers initially reference the same node.

## 3. Standard Solution — Find Middle, Then Reverse

**Key Idea:**
1. Find the middle using fast and slow pointers.
2. Reverse the second half separately.
3. Compare both halves.

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;

        // Find middle
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Reverse second half
        ListNode prev = null;
        while (slow != null) {
            ListNode next = slow.next;
            slow.next = prev;
            prev = slow;
            slow = next;
        }

        // Compare
        ListNode left = head, right = prev;
        while (right != null) {
            if (left.val != right.val) return false;
            left = left.next;
            right = right.next;
        }

        return true;
    }
}
```

**Complexity:**
- Time: **O(n)**
- Extra Space: **O(1)**

Both solutions modify the original linked list. Restore the reversed portion if preserving the original structure is required.

## 4. Keywords

- **Pattern:** Fast & Slow Pointers / In-place Reversal
- **My Solution:** Reverse first half while finding the middle.
- **Standard Solution:** Find middle first, then reverse second half.
- **Even Length:** `fast == null`
- **Odd Length:** `fast != null` (skip middle in my solution)
- **Reversal:** Save `next` → reverse link → advance pointers.
- **Comparison:** Compare node values (`val`), not references.
- **Complexity:** O(n) time / O(1) extra space.

**Recall:**
- **My approach:** Move fast → reverse slow → skip middle if odd → compare.
- **Standard approach:** Find middle → reverse second half → compare.