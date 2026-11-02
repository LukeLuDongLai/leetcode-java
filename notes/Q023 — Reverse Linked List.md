# Q023 — Reverse Linked List

**LeetCode:** [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)  
**Difficulty:** Easy  
**Pattern:** Linked List / Two Pointers / Recursion

## 1. Problem

Given the head of a singly linked list, reverse the list and return the new head.

**Example:**

```text
Input:  1 → 2 → 3 → 4 → 5 → null
Output: 5 → 4 → 3 → 2 → 1 → null
```

## 2. My Solution — Iterative (Three Pointers)

**Key Idea:**
- `previous`: Head of the reversed portion.
- `current`: Node being processed.
- `next`: Saves the next node before changing the link.
- Reverse each link, then advance the pointers.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode current = head;
        ListNode previous = null, next;

        while (current != null) {
            next = current.next;
            current.next = previous;

            previous = current;
            current = next;
        }

        return previous;
    }
}
```

**Complexity:**
- Time: **O(n)**
- Extra Space: **O(1)**

**Initial Mistake:** Returned `current` instead of `previous`. After the loop, `current == null`, while `previous` points to the new head.

## 3. Standard Solution — Recursive

**Key Idea:**
1. **Base Case:** An empty or single-node list is already reversed.
2. **Recursive Case:** Reverse the remaining list.
3. **Unwinding:** Attach the current node to the end of the reversed portion.
4. Return `newHead` through every recursive level.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode newHead = reverseList(head.next);

        head.next.next = head;
        head.next = null;

        return newHead;
    }
}
```

**Example:**

```text
Original: 1 → 2 → 3 → null

Recursive calls:
reverse(1) → reverse(2) → reverse(3)

Base Case:
reverse(3) returns Node 3

Unwinding:
3 → 2 → null
3 → 2 → 1 → null
```

**Important:**
- `newHead` always points to the original last node.
- `head.next.next = head` reverses the link.
- `head.next = null` removes the old forward link.
- Returning `newHead` preserves the new list head.

**Complexity:**
- Time: **O(n)**
- Extra Space: **O(n)** — Recursive call stack.

## 4. Keywords

- **Pattern:** Linked List Reversal
- **Iterative:** `previous`, `current`, `next`
- **Pointer Reversal:** `current.next = previous`
- **Recursive:** Base Case → Recursive Call → Unwinding
- **Recursive Link:** `head.next.next = head`
- **Return Value:** New head, not the original head
- **Space:** Iterative O(1) vs. Recursive O(n)

**Recall:** Iteration reverses links from head to tail. Recursion reaches the tail first, then reverses links while returning.