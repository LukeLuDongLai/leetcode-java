# Q004 Move Zeroes

## Problem

Given an integer array `nums`, move all `0`s to the end while keeping the relative order of the non-zero elements.

The operation must be done **in place**, without creating a separate copy of the array.

### Example

```text
Input:
[0, 1, 0, 3, 12]

Output:
[1, 3, 12, 0, 0]
```

---

## Pattern

Two Pointers / In-place Array

## Key Idea

Use two pointers:

- `i`: the next position to write to
- `j`: scans to the right for the next non-zero element

Write all non-zero elements to the front in order. Once there are no more non-zero elements, fill the remaining positions with `0`.

## Java

```java
public void moveZeroes(int[] nums) {
    int j = 0;

    for (int i = 0; i < nums.length; i++) {
        while (j < nums.length && nums[j] == 0) {
            j++;
        }

        if (j < nums.length) {
            nums[i] = nums[j];
            j++;
        } else {
            nums[i] = 0;
        }
    }
}
```

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Why It Is Not O(n²)

Although the code contains a `for` loop with a nested `while` loop, `j` only moves to the right and never moves backward.

- `i` moves at most `n` times
- `j` moves at most `n` times

Therefore:

`O(n + n) = O(n)`

## Common Mistakes

- Array length is `nums.length`, not `nums.length()`.
- A `void` method does not need to return the array.
- The relative order of non-zero elements must be preserved.
- Two pointers are useful because they have different responsibilities, not simply because there are two variables.
- Avoid restarting the search for the next non-zero element every time a `0` is found.

## Recall Keywords

`Two Pointers`  
`i = write position`  
`j = find next non-zero`  
`in-place`  
`O(n) / O(1)`