# Q015 — Rotate Array

**LeetCode:** [189. Rotate Array](https://leetcode.com/problems/rotate-array/)  
**Difficulty:** Medium  
**Topics:** Array, Two Pointers, In-place Manipulation

---

## 1. Problem

Given an integer array `nums`, rotate the array to the right by `k` steps.

Modify the original array in place.

### Example 1

```text
Input:
nums = [1,2,3,4,5,6,7]
k = 3

Output:
[5,6,7,1,2,3,4]
```

Explanation:

```text
Original:   [1,2,3,4,5,6,7]
Rotate 1:   [7,1,2,3,4,5,6]
Rotate 2:   [6,7,1,2,3,4,5]
Rotate 3:   [5,6,7,1,2,3,4]
```

### Example 2

```text
Input:
nums = [-1,-100,3,99]
k = 2

Output:
[3,99,-1,-100]
```

---

## 2. Pattern

**Array Rotation / Two Pointers / In-place Reversal**

Key techniques:

- Array index manipulation
- Modulo operation
- Two-pointer reversal
- In-place array modification
- Time and space optimization

---

## 3. Approach 1 — Brute Force

### Key Idea

Rotate the entire array one position to the right, repeating the operation `k` times.

For each rotation:

1. Save the last element.
2. Shift all remaining elements one position to the right.
3. Place the saved element at index `0`.

### Java

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;

        for (int i = 0; i < k; i++) {
            int lastNum = nums[n - 1];

            for (int j = 0; j < n - 1; j++) {
                nums[n - j - 1] = nums[n - j - 2];
            }

            nums[0] = lastNum;
        }
    }
}
```

### Complexity

- **Time:** O(n × k)
- **Extra Space:** O(1)

Each rotation takes O(n) time, and there are `k` rotations.

### Improvement

A complete rotation of `n` steps returns the array to its original state.

Therefore:

```java
k %= n;
```

This avoids unnecessary rotations when `k >= n`.

However, even with this optimization, the worst-case time complexity remains O(n²).

---

## 4. Approach 2 — Three Reversals (Optimal)

### Key Idea

Instead of rotating the array one position at a time, rearrange the array using three reversals.

For a right rotation by `k`:

1. Reverse the entire array.
2. Reverse the first `k` elements.
3. Reverse the remaining `n - k` elements.

### Visualization

```text
nums = [1,2,3,4,5,6,7]
k = 3

Original:
[1,2,3,4,5,6,7]

Step 1: Reverse the entire array
[7,6,5,4,3,2,1]

Step 2: Reverse the first k elements
[5,6,7,4,3,2,1]

Step 3: Reverse the remaining elements
[5,6,7,1,2,3,4]
```

Final result:

```text
[5,6,7,1,2,3,4]
```

### Java — My Optimized Solution

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;

        k %= n;

        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;

            left++;
            right--;
        }
    }
}
```

### Why It Works

Split the original array into two parts:

```text
A = [1,2,3,4]
B = [5,6,7]
```

The desired result is:

```text
A + B  →  B + A
```

Reversing the entire array gives:

```text
reverse(B) + reverse(A)
```

Then reverse each part individually:

```text
reverse(reverse(B)) + reverse(reverse(A))
```

This becomes:

```text
B + A
```

Therefore, three reversals achieve the rotation without allocating another array.

---

## 5. Complexity Analysis

Let `n` be the number of elements.

### Brute Force

```text
Time:  O(n × k)
Space: O(1)
```

With `k %= n`, the worst-case time is O(n²).

### Three Reversals

```text
Reverse whole array:   O(n)
Reverse first k:       O(k)
Reverse remaining:     O(n-k)

Total: O(n) + O(k) + O(n-k)
     = O(n)
```

Only a few temporary variables are needed.

```text
Time:        O(n)
Extra Space: O(1)
```

**The three-reversal approach is asymptotically optimal.**

---

## 6. Important Java Concepts

### Modulo Operator

```java
k %= n;
```

Equivalent to:

```java
k = k % n;
```

Example:

```text
n = 7
k = 10

10 % 7 = 3
```

Rotating by 10 positions is equivalent to rotating by 3 positions.

### Two-Pointer Reversal

```java
private void reverse(int[] nums, int left, int right) {
    while (left < right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;

        left++;
        right--;
    }
}
```

This reverses an inclusive interval `[left, right]`.

Examples:

```text
reverse(nums, 0, n - 1)  → Whole array
reverse(nums, 0, k - 1)  → First k elements
reverse(nums, k, n - 1)  → Remaining elements
```

Time complexity for reversing an interval of length `m`:

```text
O(m)
```

Extra space:

```text
O(1)
```

---

## 7. Edge Cases

### k = 0

No rotation is needed.

The reverse operations ultimately leave the array unchanged.

### k = n

```java
k %= n;
```

Results in `k = 0`, so the array stays unchanged.

### k > n

Use modulo to avoid redundant rotations.

### n = 1

The array remains unchanged.

### Empty Array

LeetCode guarantees `nums.length >= 1`.

If empty arrays were allowed, check `n == 0` before executing `k %= n` to avoid division by zero.

---

## 8. My Learning Process

### First Attempt

I implemented rotation by shifting every element one position to the right.

The solution was correct, but repeated the entire scan `k` times.

```text
Time: O(n × k)
Space: O(1)
```

### Optimization Challenge

I wanted to achieve:

```text
Time: O(n)
Space: O(1)
```

Initially, I struggled with moving entire sections of an array without overwriting elements.

### Key Breakthrough

Instead of directly swapping two sections, I realized that reversing the whole array puts the two sections into their correct positions, but in reversed order.

Reversing those two sections again restores their internal order.

This led to the three-reversal solution.

---

## 9. Common Mistakes

- Forgetting to normalize `k` using `k %= n`.
- Shifting from left to right and overwriting elements before using them.
- Confusing right rotation with left rotation.
- Using incorrect inclusive indices in `reverse()`.
- Reversing the sections in an incorrect order.
- Creating an additional array when O(1) extra space is required.
- Assuming three reversals mean O(n²); they are three linear operations, not nested loops.

---

## 10. Recall Keywords

**Pattern:** Array Rotation + Two Pointers

**Brute Force:**

`save last → shift right → repeat k times`

**Optimal:**

`reverse whole → reverse first k → reverse remaining`

**Formula:**

```java
k %= n;

reverse(nums, 0, n - 1);
reverse(nums, 0, k - 1);
reverse(nums, k, n - 1);
```

**Complexity:**

```text
Time:  O(n)
Space: O(1)
```

**Core Insight:**

> Reversing the entire array swaps the positions of the two sections in reversed order. Reversing each section again restores their original internal ordering.

**Recognition Trigger:**

When a problem asks to rotate an array in place with O(1) extra space, consider the three-reversal technique.