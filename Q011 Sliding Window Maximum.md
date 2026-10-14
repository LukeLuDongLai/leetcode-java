# Q011 Sliding Window Maximum

## Problem

Given an integer array `nums` and a window size `k`, return the maximum value in every sliding window of size `k`.

### Example

```text
Input:
nums = [1,3,-1,-3,5,3,6,7]
k = 3

Output:
[3,3,5,5,6,7]
```

---

## Pattern

Monotonic Deque / Sliding Window

Alternative:

Max Heap + Lazy Deletion

---

## Brute Force

For every window, scan all `k` elements to find the maximum.

```text
Number of windows ≈ n
Work per window = O(k)
```

### Complexity

- Time: `O(n * k)`
- Space: `O(1)`

The key optimization question is:

**Can information from the previous window be reused?**

---

## Approach 1: Max Heap + Lazy Deletion

Store:

```text
[value, index]
```

in a Max Heap.

- `value` → determines the maximum
- `index` → determines whether the element has left the window

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    PriorityQueue<int[]> heap =
        new PriorityQueue<>(
            (a, b) -> Integer.compare(b[0], a[0])
        );

    int[] result = new int[nums.length - k + 1];

    for (int i = 0; i < k; i++) {
        heap.offer(new int[]{nums[i], i});
    }

    result[0] = heap.peek()[0];

    for (int i = 1; i < result.length; i++) {
        int newIndex = i + k - 1;

        heap.offer(new int[]{
            nums[newIndex],
            newIndex
        });

        while (heap.peek()[1] < i) {
            heap.poll();
        }

        result[i] = heap.peek()[0];
    }

    return result;
}
```

### Lazy Deletion

Expired elements do not need to be removed immediately.

Only remove them when they reach the top of the heap.

```text
heap.peek()[1] < windowLeft
→ maximum is expired
→ poll it
```

### Complexity

- Time: `O(n log n)`
- Space: `O(n)`

Heap is good at finding the maximum, but removing arbitrary expired elements is inconvenient.

---

## Approach 2: Monotonic Deque

The Deque stores **indices**, not values.

It maintains:

```text
nums[deque[0]]
>= nums[deque[1]]
>= nums[deque[2]]
...
```

So:

```java
nums[deque.peekFirst()]
```

is always the maximum candidate.

---

## Core Idea: Candidate Elimination

Suppose an older element is:

```text
3
```

and a newer element is:

```text
5
```

Since `5` is:

- larger than `3`
- newer than `3`
- therefore leaves the window later

then `3` can never become the maximum before `5` disappears.

So `3` can be permanently removed.

Therefore, when a new element enters:

```java
while (!deque.isEmpty()
        && nums[newIndex] >= nums[deque.peekLast()]) {
    deque.pollLast();
}
```

Remove weaker candidates from the back.

---

## Standard Solution

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> deque = new ArrayDeque<>();
    int[] result = new int[nums.length - k + 1];

    for (int i = 0; i < nums.length; i++) {

        // Remove expired indices from the front
        while (!deque.isEmpty()
                && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }

        // Remove weaker candidates from the back
        while (!deque.isEmpty()
                && nums[i] >= nums[deque.peekLast()]) {
            deque.pollLast();
        }

        deque.offerLast(i);

        // Record result once the first full window is formed
        if (i >= k - 1) {
            result[i - k + 1] =
                nums[deque.peekFirst()];
        }
    }

    return result;
}
```

---

## Why Store Indices?

An index gives both:

```java
nums[index]   // value
index         // position
```

The position is needed to detect expired elements.

For current window:

```text
[left, right]
```

an index is expired when:

```text
index < left
```

---

## Why Is the Deque Monotonic?

From front to back, values are decreasing:

```text
[9, 7, 5, 2]
```

The front is always the strongest candidate.

When a larger new value arrives:

```text
[9, 7, 5, 2] + 6
```

remove from the back:

```text
2 < 6 → remove
5 < 6 → remove
```

Result:

```text
[9, 7, 6]
```

Only elements that may still become a future maximum are kept.

---

## Complexity

### Monotonic Deque

- Time: `O(n)`
- Space: `O(k)`

Although there is a nested `while`, each index is:

```text
added at most once
removed at most once
```

So the total number of Deque operations is linear.

This is an example of:

**Amortized Analysis**

---

## Heap vs Deque

### Max Heap

```text
find maximum        easy
remove old elements inconvenient
Time                O(n log n)
Space               O(n)
```

### Monotonic Deque

```text
find maximum        O(1)
remove expired      O(1)
remove useless      O(1) amortized
Time                O(n)
Space               O(k)
```

---

## Common Mistakes

- Store indices rather than only values.
- Remove expired indices from the front.
- Remove weaker candidates from the back.
- The front of the Deque stores the current maximum.
- Nested `while` does not automatically mean `O(n²)`.
- Use `peek()` to inspect and `poll()` to remove.
- `>=` can remove older duplicate values because the newer equal value expires later.

---

## Recall Keywords

`Sliding Window`

`Monotonic Deque`

`store indices`

`front = maximum`

`remove expired from front`

`remove weaker candidates from back`

`candidate elimination`

`each index enters once / leaves once`

`O(n) / O(k)`