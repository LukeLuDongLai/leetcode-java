# Q022 — Intersection of Two Linked Lists

**LeetCode:** [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)  
**Difficulty:** Easy  
**Pattern:** Linked List / Two Pointers

## 1. Problem

Given two singly linked lists, return their first intersecting node, or `null` if they do not intersect.

**Important:** Intersection means the **same node reference**, not the same value.

```text
List A: 4 → 1 ↘
              8 → 4 → 5
List B: 5 → 6 → 1 ↗

Output: Node 8
```

## 2. My Solution — Length Alignment

**Key Idea:**
1. Calculate both list lengths.
2. Advance the longer list by the length difference.
3. Move both pointers together until they meet.

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int countA = 0, countB = 0;
        ListNode pointerA = headA;
        ListNode pointerB = headB;

        while (pointerA != null) {
            countA++;
            pointerA = pointerA.next;
        }
        while (pointerB != null) {
            countB++;
            pointerB = pointerB.next;
        }

        pointerA = headA;
        pointerB = headB;

        for (int i = 0; i < Math.abs(countA - countB); i++) {
            if (countA > countB) {
                pointerA = pointerA.next;
            } else {
                pointerB = pointerB.next;
            }
        }

        while (pointerA != null) {
            if (pointerA == pointerB) return pointerA;
            pointerA = pointerA.next;
            pointerB = pointerB.next;
        }

        return null;
    }
}
```

**Complexity:**
- Time: **O(m + n)**
- Extra Space: **O(1)**

## 3. Official / Standard Solution — Head Switching

**Key Idea:** When a pointer reaches `null`, redirect it to the other list's head. Both pointers travel equal total distances, automatically compensating for different list lengths.

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA, b = headB;

        while (a != b) {
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }

        return a;
    }
}
```

**Why it works:**

```text
List A = a + common
List B = b + common

Pointer A: a + common + b
Pointer B: b + common + a
```

Both pointers travel the same distance before meeting at the intersection. If no intersection exists, both eventually become `null`.

**Complexity:** O(m + n) time / O(1) extra space.

## 4. Keywords

- **Pattern:** Linked List / Two Pointers
- **Node Identity:** `pointerA == pointerB`, not `.val`
- **Length Alignment:** Advance the longer list first.
- **Head Switching:** A → B, B → A
- **Termination:** Meet at the shared node or both reach `null`.
- **Complexity:** O(m + n) time / O(1) extra space

**Recall:** Either align both pointers by length difference or switch their heads to equalize the distance traveled.