# Q021 — Search a 2D Matrix II

**LeetCode:** [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)  
**Difficulty:** Medium  
**Pattern:** Matrix / Staircase Search

## 1. Problem

Given an `m × n` matrix where each row and column is sorted in ascending order, determine whether `target` exists.

**Example:**

```text
Matrix:
1   4   7  11
2   5   8  12
3   6   9  16
10 13  14  17

target = 8  → true
target = 20 → false
```

## 2. My Solution — Staircase Search

**Key Idea:**
- Start from the **top-right corner**.
- If `current > target`, move left (`j--`) to eliminate the current column.
- If `current < target`, move down (`i++`) to eliminate the current row.
- Continue until the target is found or the indices go out of bounds.

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int i = 0, j = n - 1;

        while (i < m && j >= 0) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] > target) {
                j--;
            } else {
                i++;
            }
        }

        return false;
    }
}
```

**Complexity:**
- Time: **O(m + n)** — Each step eliminates one row or column.
- Extra Space: **O(1)**

## 3. Official / Standard Solution

My solution follows the standard **Staircase Search** approach and is already optimal in worst-case asymptotic complexity.

Alternative: Start from the **bottom-left corner**, moving right when the current value is too small and up when it is too large.

Other approaches:

| Approach | Time | Space |
|---|---|---|
| Brute Force | O(m × n) | O(1) |
| Binary Search Each Row | O(m log n) | O(1) |
| Staircase Search | O(m + n) | O(1) |

## 4. Keywords

- **Pattern:** Staircase Search / Matrix Search
- **Starting Point:** Top-right or bottom-left corner
- **Top-right:** Too large → Left; Too small → Down
- **Elimination:** Discard an entire row or column per step
- **Why Not Top-left?** Both right and down contain larger values, so the direction is ambiguous.
- **Complexity:** O(m + n) time / O(1) extra space

**Recall:** Start at the top-right corner. Compare, eliminate one row or column, and repeat. No backtracking is needed.