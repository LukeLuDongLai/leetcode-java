# Q020 — Rotate Image

**LeetCode:** [48. Rotate Image](https://leetcode.com/problems/rotate-image/)  
**Difficulty:** Medium  
**Pattern:** Matrix / In-place Rotation

## 1. Problem

Rotate an `n × n` matrix **90° clockwise**, in place, without using an additional matrix.

**Example:**

```text
Input:          Output:
1  2  3         7  4  1
4  5  6    →    8  5  2
7  8  9         9  6  3
```

## 2. My Solution — Transpose + Reverse

**Key Idea:**
1. **Transpose:** Swap `matrix[i][j]` with `matrix[j][i]` across the main diagonal.
2. **Reverse:** Reverse each row to achieve a 90° clockwise rotation.

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Transpose
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Reverse each row
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
            }
        }
    }
}
```

**Complexity:**
- Time: **O(n²)**
- Extra Space: **O(1)**

## 3. Alternative Solution — Four-way Cyclic Swap

**Key Idea:** Rotate four corresponding elements simultaneously, processing the matrix layer by layer.

Coordinate transformation:

`(i, j) → (j, n - 1 - i)`

Four positions in one cycle:

```text
A = (i, j)
B = (j, n-1-i)
C = (n-1-i, n-1-j)
D = (n-1-j, i)

A → B → C → D → A
```

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        for (int i = 0; i < n / 2; i++) {
            for (int j = i; j < n - 1 - i; j++) {
                int temp = matrix[i][j];

                matrix[i][j] = matrix[n-1-j][i];
                matrix[n-1-j][i] = matrix[n-1-i][n-1-j];
                matrix[n-1-i][n-1-j] = matrix[j][n-1-i];
                matrix[j][n-1-i] = temp;
            }
        }
    }
}
```

**Complexity:**
- Time: **O(n²)**
- Extra Space: **O(1)**

Both approaches are optimal. Transpose + Reverse is generally easier to implement and verify.

## 4. Keywords

- **Pattern:** Matrix / In-place Rotation
- **Clockwise 90°:** Transpose → Reverse each row
- **Alternative:** Reverse row order → Transpose
- **Transpose:** `matrix[i][j] ↔ matrix[j][i]`
- **Coordinate Mapping:** `(i, j) → (j, n-1-i)`
- **Four-way Swap:** Move four elements in one cycle using `temp`.
- **Layer Traversal:** Outer loop `i < n/2`; inner loop `j = i` to `n-1-i` (exclusive).
- **Complexity:** O(n²) time / O(1) extra space

**Recall:** For clockwise rotation, transpose then reverse every row. Alternatively, rotate four corresponding positions layer by layer.