# Q025 & 026* — Linked List Cycle II

**LeetCode:** [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)  
**Difficulty:** Medium  
**Pattern:** Linked List / Fast & Slow Pointers / Floyd's Cycle Detection

## 1. Problem

Given the head of a linked list, return the node where the cycle begins. Return `null` if no cycle exists.

**Example:**

```text
3 → 2 → 0 → -4
    ↑        |
    └────────┘

Output: Node 2
```

**Constraint:** Do not modify the linked list.

## 2. My Solution — Floyd's Cycle Detection

**Key Idea:**
1. Use fast and slow pointers to detect a cycle.
2. Once they meet, start one pointer from `head` and another from the meeting point.
3. Move both one step at a time. Their next meeting point is the cycle entrance.

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;

        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;

            if (slow == fast) {
                ListNode p1 = head;
                ListNode p2 = slow;

                while (p1 != p2) {
                    p1 = p1.next;
                    p2 = p2.next;
                }

                return p1;
            }
        }

        return null;
    }
}
```

**Complexity:**
- Time: **O(n)**
- Extra Space: **O(1)**

## 3. Official / Standard Solution

My solution is the standard Floyd's Cycle Detection algorithm.

### Mathematical Proof (Important*)

Define:
- `a`: Distance from head to cycle entrance.
- `b`: Distance from entrance to meeting point.
- `c`: Distance from meeting point back to entrance.
- `L = b + c`: Cycle length.

Since fast moves twice as fast as slow:

```text
fast = 2 × slow
fast - slow = kL    (fast travels k extra cycles)

Therefore:
slow = kL
```

Slow may have completed `x` extra cycles:

```text
slow = a + b + xL

a + b + xL = kL
a = (k - x)L - b

Since b = L - c:

a = (k - x - 1)L + c
```

**Key conclusion:**

```text
a ≡ c (mod L)
```

This means `a` and `c` differ by a whole number of cycle lengths.

Therefore:
- A pointer starting at `head` reaches the entrance after `a` steps.
- A pointer starting at the meeting point also reaches the entrance after `a` steps (possibly completing extra cycles).

**No need to calculate `a`!** Move both pointers until they meet.

## 4. Keywords

- **Pattern:** Floyd's Cycle Detection / Fast & Slow Pointers
- **Phase 1:** Slow moves 1 step; fast moves 2 steps.
- **Phase 2:** Start from head and meeting point; move at equal speeds.
- **Cycle Entrance:** Second meeting point
- **Mathematical Key:** `a ≡ c (mod L)`
- **Node Comparison:** `slow == fast`, not `.val`
- **Edge Cases:** No cycle, self-loop, cycle starting at head
- **Complexity:** O(n) time / O(1) extra space

**Recall:** Fast and slow meet inside the cycle. Reset one pointer to head, move both one step at a time, and their next meeting is the cycle entrance.