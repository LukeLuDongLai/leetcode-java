# Q019 — Spiral Matrix

**LeetCode:** [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)  
**Difficulty:** Medium  
**Pattern:** Matrix / Simulation / Boundary Traversal

## 1. Problem

Given an `m × n` matrix, return all elements in clockwise spiral order.

**Example:**

```text
Input:
1  2  3
4  5  6
7  8  9

Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

## 2. My Solution — Boundary Traversal + Counter

**Key Idea:**
- Maintain four boundaries: `top`, `bottom`, `left`, `right`.
- Traverse in four directions: Right → Down → Left → Up.
- Shrink the corresponding boundary after each direction.
- Use `count` to stop once all elements have been visited.

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> sequence = new ArrayList<>();
        int m = matrix.length;
        int n = matrix[0].length;

        int top = 0, bottom = m - 1;
        int left = 0, right = n - 1;
        int count = 0;

        while (count < m * n) {
            // Right
            for (int j = left; j <= right; j++) {
                sequence.add(matrix[top][j]);
                count++;
            }
            top++;
            if (count >= m * n) break;

            // Down
            for (int i = top; i <= bottom; i++) {
                sequence.add(matrix[i][right]);
                count++;
            }
            right--;
            if (count >= m * n) break;

            // Left
            for (int j = right; j >= left; j--) {
                sequence.add(matrix[bottom][j]);
                count++;
            }
            bottom--;
            if (count >= m * n) break;

            // Up
            for (int i = bottom; i >= top; i--) {
                sequence.add(matrix[i][left]);
                count++;
            }
            left++;
        }

        return sequence;
    }
}
```

**Complexity:**
- Time: **O(m × n)** — Each element is visited once.
- Extra Space: **O(1)** — Excluding the output list.

## 3. Standard Solution — Boundary Checks

A cleaner approach removes `count` and uses the boundaries to control traversal.

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        int top = 0, bottom = matrix.length - 1;
        int left = 0, right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {

            // Right
            for (int j = left; j <= right; j++)
                result.add(matrix[top][j]);
            top++;

            // Down
            for (int i = top; i <= bottom; i++)
                result.add(matrix[i][right]);
            right--;

            // Left
            if (top <= bottom) {
                for (int j = right; j >= left; j--)
                    result.add(matrix[bottom][j]);
                bottom--;
            }

            // Up
            if (left <= right) {
                for (int i = bottom; i >= top; i--)
                    result.add(matrix[i][left]);
                left++;
            }
        }

        return result;
    }
}
```

**Why only two extra checks?**

- **Right:** Both boundaries are valid when entering `while`.
- **Down:** The `for` condition automatically checks `top <= bottom`.
- **Left:** Must check `top <= bottom` to avoid revisiting a completed row.
- **Up:** Must check `left <= right` to avoid revisiting a completed column.

**Complexity:** O(m × n) time / O(1) extra space.

## 4. Keywords

- **Pattern:** Boundary Traversal / Simulation
- **Four Boundaries:** `top`, `bottom`, `left`, `right`
- **Direction:** Right → Down → Left → Up
- **Boundary Updates:** `top++`, `right--`, `bottom--`, `left++`
- **Termination:** `top > bottom || left > right`
- **Edge Cases:** Single row, single column, rectangular matrix
- **Important:** A `for` loop may already enforce a boundary condition, making some additional `if` checks unnecessary.

**Recall:** Traverse the outer rectangle clockwise, shrink its boundaries, and repeat until the boundaries cross. Check the remaining row before moving left and the remaining column before moving up.