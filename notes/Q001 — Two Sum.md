# Q001 Two Sum

## Problem

Given an integer array `nums` and an integer `target`, find two different elements whose sum equals `target`.

Return the **indices** of those two elements.

You may assume that exactly one valid solution exists, and the same element cannot be used twice.

### Example

```text
Input:
nums = [2, 7, 11, 15]
target = 9

Output:
[0, 1]
```

Because:

`nums[0] + nums[1] = 2 + 7 = 9`

---

## Pattern

HashMap / Hash Lookup

## Key Idea

While iterating through the array:

- `key` = the number we want to find later: `target - nums[i]`
- `value` = the current index `i`

If the current `nums[i]` already exists as a key in the map, we have found the pair.

## Java

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i])) {
            return new int[]{map.get(nums[i]), i};
        }

        map.put(target - nums[i], i);
    }

    return new int[]{};
}
```

## Complexity

- Time: `O(n)`
- Space: `O(n)`

## Common Mistakes

- Return the **indices**, not the values.
- **Check before inserting** to avoid using the same element twice.
- Duplicate values in the array can still be handled correctly.

## Recall Keywords

`a + b = target`  
`b = target - a`  
`HashMap: trade space for faster lookup`