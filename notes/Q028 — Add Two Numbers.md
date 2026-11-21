# Q028 — Add Two Numbers

**LeetCode:** [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)  
**Difficulty:** Medium  
**Pattern:** Linked List / Dummy Node / Carry

## 1. Problem

Given two linked lists representing non-negative integers in reverse order, return their sum as a linked list.

**Example:**

```text
Input:
l1: 2 → 4 → 3  (342)
l2: 5 → 6 → 4  (465)

Output:
    7 → 0 → 8  (807)
```

## 2. My Solution — Dummy Node + Carry

**Key Idea:**
- Use a dummy node to build the result.
- Traverse both lists simultaneously, treating missing digits as `0`.
- Use `% 10` for the current digit and `/ 10` for the carry.
- Append any remaining carry after the loop.

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        int carry = 0;

        while (l1 != null || l2 != null) {
            int num1 = 0, num2 = 0;

            if (l1 != null) {
                num1 = l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                num2 = l2.val;
                l2 = l2.next;
            }

            int sum = carry + num1 + num2;
            tail.next = new ListNode(sum % 10);
            tail = tail.next;
            carry = sum / 10;
        }

        if (carry != 0) {
            tail.next = new ListNode(carry);
        }

        return dummy.next;
    }
}
```

**Complexity:**
- Time: **O(max(m, n))**
- Auxiliary Space: **O(1)**, excluding the output list.

## 3. Standard Solution — Include Carry in Loop Condition

The same algorithm can be simplified by including `carry` in the loop condition.

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        int carry = 0;

        while (l1 != null || l2 != null || carry != 0) {
            int sum = carry;

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            tail.next = new ListNode(sum % 10);
            tail = tail.next;
            carry = sum / 10;
        }

        return dummy.next;
    }
}
```

**Improvement:** No separate `if` is needed for the final carry. Both approaches have identical complexity.

## 4. Keywords

- **Pattern:** Linked List / Simulation / Dummy Node
- **Dummy Node:** Simplifies result construction.
- **Carry:** `sum / 10`
- **Current Digit:** `sum % 10`
- **Null Handling:** Treat missing digits as `0`.
- **Loop Condition:** Continue while either list or carry remains.
- **Loop Invariant:** `tail` points to the last result node; `carry` stores the previous digit's carry.
- **Edge Cases:** Different lengths, `[0] + [0]`, final carry.
- **Complexity:** O(max(m, n)) time / O(1) auxiliary space.

**Recall:** Traverse both lists, add corresponding digits plus carry, append `sum % 10`, and update `carry = sum / 10`.

**Coding Lesson:** Use a dummy node and a unified loop condition to reduce special-case handling.