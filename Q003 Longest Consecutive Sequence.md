# Q003 Longest Consecutive Sequence

## Problem

Given an unsorted integer array `nums`, return the length of the longest sequence of consecutive integers.

The numbers in the sequence do not need to appear next to each other in the original array.

### Example

```text
Input:
[100, 4, 200, 1, 3, 2]

Output:
4
```

The longest consecutive sequence is:

```text
[1, 2, 3, 4]
```

---

## Pattern

HashSet / Sequence Start / Avoid Repeated Work

## Key Idea

Put all numbers into a `HashSet`.

Only start scanning from the beginning of a consecutive sequence:

- If `num - 1` does not exist, then `num` is the start of a sequence.
- Keep checking `num + 1`.
- Count the length of the current consecutive sequence.

## Java

```java
public int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<>();

    for (int num : nums) {
        set.add(num);
    }

    int max = 0;

    for (int num : set) {
        if (!set.contains(num - 1)) {
            int current = num;
            int count = 1;

            while (set.contains(current + 1)) {
                current++;
                count++;
            }

            max = Math.max(max, count);
        }
    }

    return max;
}
```

## Complexity

- Time: `O(n)` average
- Space: `O(n)`

## Why It Is Not O(n²)

Although the code contains a `for` loop with a nested `while` loop, only sequence-starting numbers enter the `while` loop.

Each number is scanned as part of a consecutive sequence at most once, so the total amount of work remains linear.

## Common Mistakes

- Sorting is unnecessary and would increase the time complexity to `O(n log n)`.
- `HashSet` automatically removes duplicates.
- The key condition is:

```java
!set.contains(num - 1)
```

- Do not start scanning forward from every number, or the worst-case time complexity becomes `O(n²)`.

## Recall Keywords

`HashSet`  
`find the sequence start`  
`scan each sequence once`  
`avoid repeated work`