# Q027 — Merge Two Sorted Lists

**LeetCode:** [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)  
**Difficulty:** Easy  
**Pattern:** Linked List / Two Pointers / Dummy Node

## 1. Problem

Merge two sorted linked lists into one sorted list by reusing the existing nodes.

**Example:**

```text
List 1: 1 → 2 → 4
List 2: 1 → 3 → 4

Output: 1 → 1 → 2 → 3 → 4 → 4
```

## 2. My Solution — Alternating List Segments

**Key Idea:**
- Handle empty lists first.
- Ensure `list1` starts with the smaller value.
- Traverse consecutive sorted segments and connect the two lists alternately.

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list2 == null && list1 != null) return list1;
        if (list1 == null && list2 != null) return list2;
        if (list1 == null && list2 == null) return null;

        if (list1.val > list2.val) {
            ListNode temp = list1;
            list1 = list2;
            list2 = temp;
        }

        ListNode head = list1;

        while (list1 != null && list2 != null) {
            while (list1.next != null && list1.next.val <= list2.val) {
                list1 = list1.next;
            }

            ListNode current = list1;
            list1 = list1.next;
            current.next = list2;

            if (list1 == null) return head;

            while (list2.next != null && list2.next.val <= list1.val) {
                list2 = list2.next;
            }

            current = list2;
            list2 = list2.next;
            current.next = list1;
        }

        return head;
    }
}
```

**Complexity:**
- Time: **O(m + n)**
- Extra Space: **O(1)**

**Lesson Learned:** The algorithm was straightforward, but manually managing the head, node connections, and `null` cases made the implementation unnecessarily complicated.

## 3. Standard Solution — Dummy Node + Two Pointers

**Key Idea:**
1. Use a dummy node to avoid special handling of the head.
2. Always append the smaller current node.
3. When one list ends, attach the remaining list directly.

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                tail.next = list1;
                list1 = list1.next;
            } else {
                tail.next = list2;
                list2 = list2.next;
            }
            tail = tail.next;
        }

        tail.next = (list1 != null) ? list1 : list2;

        return dummy.next;
    }
}
```

**Complexity:**
- Time: **O(m + n)**
- Extra Space: **O(1)**

**Why Better:**
- Dummy node eliminates head-related edge cases.
- The `while` condition guarantees both pointers are non-null.
- Remaining nodes can be attached in one operation.
- Fewer variables and simpler pointer management.

## 4. Keywords

- **Pattern:** Linked List / Two Pointers / Dummy Node
- **Dummy Node:** Avoid special handling of the first node.
- **Tail Pointer:** Tracks the last node of the merged list.
- **Loop Invariant:** Both list pointers are non-null inside the loop; the merged prefix remains sorted.
- **Relinking:** `tail.next = list1` modifies the chain.
- **Moving:** `tail = tail.next` advances the pointer.
- **Remaining Nodes:** Attach the unfinished list directly.
- **Return:** `dummy.next`, not `dummy`.
- **Complexity:** O(m + n) time / O(1) extra space.

**Recall:** Use a dummy node, repeatedly append the smaller node, and attach the remaining list when one becomes null.

**Coding Lesson:** Reduce edge cases through better code structure rather than adding more conditional checks.