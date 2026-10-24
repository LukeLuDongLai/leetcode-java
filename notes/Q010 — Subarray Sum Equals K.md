# Q010 Subarray Sum Equals K

## Problem

Given an integer array `nums` and an integer `k`, return the number of continuous subarrays whose sum equals `k`.

Subarrays with the same values but different positions count as different subarrays.

### Example

```text
Input:
nums = [1, 1, 1]
k = 2

Output:
2
```

The valid subarrays are:

```text
nums[0..1] = [1, 1]
nums[1..2] = [1, 1]
```

---

## Pattern

Prefix Sum / HashMap / Frequency Count

## Core Formula

For two prefix sums:

`currentPrefix - previousPrefix = k`

Rearrange:

`previousPrefix = currentPrefix - k`

So when the current prefix sum is `sum`, we only need to ask:

`How many previous prefix sums equal sum - k?`

Each match represents one different subarray whose sum is `k`.

---

## Standard Solution

The `HashMap` stores:

- `key` = prefix sum
- `value` = how many times this prefix sum has appeared before

```java
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();

    map.put(0, 1);

    int sum = 0;
    int count = 0;

    for (int num : nums) {
        sum += num;

        count += map.getOrDefault(sum - k, 0);

        map.put(
            sum,
            map.getOrDefault(sum, 0) + 1
        );
    }

    return count;
}
```

## Why `map.put(0, 1)`?

It represents the prefix sum before the array starts:

`prefix sum = 0`

If a subarray starting from index `0` already has sum `k`, then:

`sum - k = 0`

and this initial state allows it to be counted correctly.

---

## Why Store Frequency?

The same prefix sum can appear at multiple positions.

If:

`sum - k`

has appeared 3 times before, then the current position can form 3 different valid subarrays.

So:

`existence only -> not enough`

We need:

`prefix sum -> frequency`

---

## Why Check Before `put`?

The map must contain only prefix sums from positions before the current one.

Correct order:

```java
count += map.getOrDefault(sum - k, 0);
map.put(sum, map.getOrDefault(sum, 0) + 1);
```

If we insert first, then when `k = 0`, the current prefix sum can match itself.

That would incorrectly count an empty subarray.

---

## Why Ordinary Sliding Window Does Not Work

The array can contain negative numbers.

Therefore, when the window expands:

`sum` may increase or decrease

There is no reliable monotonic behavior, so we cannot safely decide how to move a pointer based only on:

`sum < k` or `sum > k`

---

## My Thought Process

### First Attempt

Build a prefix sum array:

```java
prefix[right] - prefix[left]
```

This gives the sum of any subarray in `O(1)`.

Problem:

I tried to move `left` and `right` only forward, similar to Sliding Window.

However, negative numbers destroy monotonicity, so an old `left` cannot be safely discarded.

### Second Attempt

From:

`prefix[right] - prefix[left] = k`

derive:

`prefix[left] = prefix[right] - k`

So instead of searching for `left`, use a `HashMap` to directly look up previous prefix sums.

My initial bug was:

`insert current prefix -> then search`

This fails when `k = 0` because the current prefix can match itself.

Fix:

`search first -> then insert`

---

## Complexity

- Time: `O(n)`
- Space: `O(n)`

Each element is processed once, and `HashMap` lookup/insertion is `O(1)` on average.

## Common Mistakes

- A `subarray` must be continuous.
- Subarrays with identical values but different positions count separately.
- Negative numbers make ordinary Sliding Window unreliable.
- The map stores the **frequency** of each prefix sum, not just whether it exists.
- The initial state must include `0 -> 1`.
- Always **check before inserting** the current prefix sum.
- `k = 0` is a useful case for exposing the self-matching bug.

## Recall Keywords

`Prefix Sum`  
`current - previous = k`  
`previous = current - k`  
`HashMap<Prefix, Frequency>`  
`map.put(0, 1)`  
`check before put`  
`O(n) / O(n)`