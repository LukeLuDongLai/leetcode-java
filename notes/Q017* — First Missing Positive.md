# Q017* — First Missing Positive

**LeetCode:** [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)  
**Difficulty:** Hard  
**Pattern:** Cyclic Sort / In-place Hashing

## 1. Problem

Find the smallest missing positive integer in an unsorted array.

**Requirements:** O(n) time and O(1) extra space.

```text
Input:  [3, 4, -1, 1]
Output: 2

Input:  [1, 2, 0]
Output: 3
```

## 2. My Solution — Cyclic Sort

**Key Idea:** For an array of length `n`, the answer must be in `[1, n+1]`.

Place each valid number `x` at its correct index `x - 1`.

Then scan for the first position where `nums[i] != i + 1`.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int index = nums[i] - 1;

            while (index >= 0 && index < nums.length
                    && nums[index] != index + 1) {
                nums[i] = nums[index];
                nums[index] = index + 1;
                index = nums[i] - 1;
            }
        }

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }

        return nums.length + 1;
    }
}
```

**Complexity:**
- Time: **O(n)** — Each successful swap places a number into its correct position.
- Extra Space: **O(1)**

**My Initial Mistakes:**
- Tried using `0` or `n+1` to mark visited positions, which lost necessary information.
- Incorrectly checked whether the destination value was valid. Only the current number needs a valid target index.
- Initially used `i--` to reprocess swapped elements; an inner `while` is clearer.

## 3. Official / Standard Solution — Negative Marking

**Alternative approach:** Use the sign of each array element to record whether a number exists.

1. Replace values outside `[1, n]` with `n+1`.
2. For every valid number `x`, mark `nums[x-1]` as negative.
3. Find the first positive position.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0 || nums[i] > n) {
                nums[i] = n + 1;
            }
        }

        for (int i = 0; i < n; i++) {
            int num = Math.abs(nums[i]);

            if (num <= n) {
                int index = num - 1;
                nums[index] = -Math.abs(nums[index]);
            }
        }

        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) {
                return i + 1;
            }
        }

        return n + 1;
    }
}
```

**Complexity:** O(n) time, O(1) extra space.

Both approaches use the original array as an implicit hash structure.

## 4. Keywords

- **Cyclic Sort / In-place Hashing**
- **Value → Index:** `x → x - 1`
- **Valid Range:** `1 <= x <= n`
- **Answer Range:** `[1, n+1]`
- **Duplicate Check:** Avoid swapping if the target position already contains `x`.
- **Amortized Analysis:** Nested loops can still be O(n) because each successful swap fixes a position.
- **Negative Marking:** Use `-Math.abs()` to mark presence.
- **Complexity:** O(n) time / O(1) extra space.

**Recall:** Place each valid positive integer at its corresponding index, then return the first mismatched index + 1.