# Q018 — Set Matrix Zeroes

**LeetCode:** [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)  
**Difficulty:** Medium  
**Pattern:** Matrix / In-place Marking

## 1. Problem

Given an `m × n` matrix, if an element is `0`, set its entire row and column to `0`. Modify the matrix in place.

**Example:**

```text
Input:
1 1 1
1 0 1
1 1 1

Output:
1 0 1
0 0 0
1 0 1
```

## 2. My Solution — In-place Marking

**Key Idea:**
- Use the first row (`matrix[0][j]`) to mark columns that need zeroing.
- Use the first column (`matrix[i][0]`) to mark rows that need zeroing.
- Use `firstColZero` to separately track whether the first column needs zeroing.
- Traverse backwards when applying markers to avoid overwriting them prematurely.

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        boolean firstColZero = false;

        // Step 1: Record zero markers
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    if (j == 0) {
                        firstColZero = true;
                        continue;
                    }

                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }

        // Step 2: Apply markers backwards
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if ((j > 0 &&
                    (matrix[0][j] == 0 || matrix[i][0] == 0))
                    || (j == 0 && firstColZero)) {

                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

**Complexity:**
- Time: **O(m × n)** — Two matrix traversals.
- Extra Space: **O(1)** — One boolean variable.

**Initial Approach:** Used `boolean[] zeroRows` and `boolean[] zeroCols` to store markers. Correct, but required O(m + n) extra space.

**My Mistake:** Used `break` when detecting a zero in the first column, accidentally skipping other elements in the same row. Replaced it with `continue`.

## 3. Official / Standard Solution

The standard O(1)-space solution uses the same in-place marking technique.

**Core logic:**

1. Reuse the first row and first column as marker arrays.
2. Handle the shared position `matrix[0][0]` carefully.
3. Use one boolean to independently track the first column.
4. Apply markers without destroying information that is still needed.

My optimized solution follows this standard approach, so no additional implementation is necessary.

## 4. Keywords

- **Pattern:** Matrix / In-place Marking
- **Row Marker:** `matrix[i][0]`
- **Column Marker:** `matrix[0][j]`
- **Special Case:** `matrix[0][0]` cannot independently represent both first-row and first-column states.
- **Extra Variable:** `firstColZero`
- **Traversal:** Reverse order protects marker values until they are used.
- **Java:** `break` exits the loop; `continue` skips only the current iteration.
- **Complexity:** O(m × n) time / O(1) extra space.

**Recall:** Use the matrix's first row and column as marker arrays, track the first column separately, and apply changes from bottom-right to top-left.