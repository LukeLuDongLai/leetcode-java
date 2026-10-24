# Q013 Maximum Subarray

## Problem

Given an integer array `nums`, find the contiguous subarray with the largest sum and return that sum.

### Example

```text
Input:
[-2,1,-3,4,-1,2,1,-5,4]

Output:
6
```

The maximum-sum subarray is:

```text
[4,-1,2,1]
```

with sum:

```text
6
```

---

## Pattern

Dynamic Programming / Kadane's Algorithm

Alternative:

Prefix Sum + Minimum Prefix

---

## DP State

Define:

```text
dp[i] =
maximum subarray sum ending exactly at index i
```

The phrase **ending at `i`** is important.

This state contains exactly the information needed to decide whether `nums[i + 1]` should continue the previous subarray or start a new one.

---

## Transition

At index `i`, there are only two choices:

### 1. Continue the previous subarray

```text
dp[i - 1] + nums[i]
```

### 2. Start a new subarray at `i`

```text
nums[i]
```

Therefore:

```text
dp[i] = max(
    nums[i],
    dp[i - 1] + nums[i]
)
```

---

## Base Case

```text
dp[0] = nums[0]
```

The first position has no previous subarray to continue.

---

## Final Answer

The answer is not necessarily:

```text
dp[n - 1]
```

because the best subarray may end before the last element.

Therefore:

```text
answer = max(dp[i])
```

---

## Standard DP Solution

```java
public int maxSubArray(int[] nums) {
    int[] dp = new int[nums.length];

    dp[0] = nums[0];
    int maxSum = dp[0];

    for (int i = 1; i < nums.length; i++) {
        dp[i] = Math.max(
            nums[i],
            dp[i - 1] + nums[i]
        );

        maxSum = Math.max(maxSum, dp[i]);
    }

    return maxSum;
}
```

### Complexity

- Time: `O(n)`
- Space: `O(n)`

---

## Kadane's Algorithm

Notice that:

```text
dp[i]
```

depends only on:

```text
dp[i - 1]
```

So the whole `dp` array is unnecessary.

Use one variable:

```text
currentSum
```

to represent the current DP state.

```java
public int maxSubArray(int[] nums) {
    int currentSum = nums[0];
    int maxSum = nums[0];

    for (int i = 1; i < nums.length; i++) {
        currentSum = Math.max(
            nums[i],
            currentSum + nums[i]
        );

        maxSum = Math.max(
            maxSum,
            currentSum
        );
    }

    return maxSum;
}
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`

This is Kadane's Algorithm.

---

## `currentSum` vs `maxSum`

`currentSum`:

```text
maximum subarray sum ending at the current index
```

`maxSum`:

```text
best subarray sum found anywhere so far
```

Example:

```text
nums = [5, -10, 3]
```

```text
num       currentSum       maxSum
5              5              5
-10           -5              5
3              3              5
```

The current state may become worse, while the historical best must still be preserved.

---

## Core Intuition

At every number, ask:

```text
Should I keep the previous subarray?

or

Should I restart from the current number?
```

If the previous accumulated sum helps:

```text
continue
```

If it hurts:

```text
restart
```

That is the entire idea behind Kadane's Algorithm.

---

## My Prefix Sum Approach

Another valid `O(n)` approach is:

```text
subarraySum =
currentPrefix - previousPrefix
```

For a fixed `currentPrefix`, maximize the subarray sum by subtracting the smallest previous prefix.

So maintain:

```text
sum = current prefix sum
minPrefix = smallest previous prefix sum
```

```java
public int maxSubArray(int[] nums) {
    int sum = 0;
    int minPrefix = 0;
    int maxSum = Integer.MIN_VALUE;

    for (int num : nums) {
        sum += num;

        maxSum = Math.max(
            maxSum,
            sum - minPrefix
        );

        minPrefix = Math.min(
            minPrefix,
            sum
        );
    }

    return maxSum;
}
```

Important order:

```text
calculate answer first
then update minPrefix
```

Otherwise the current prefix could subtract itself and incorrectly create an empty subarray.

---

## Kadane vs Prefix Sum

### Prefix Sum View

```text
currentPrefix - minimumPreviousPrefix
```

Think:

> Remove the worst previous prefix.

### Kadane View

```text
max(nums[i], previousBest + nums[i])
```

Think:

> Keep the previous subarray only if it helps.

Both lead to:

```text
O(n)
```

but Kadane is the more standard solution for this problem.

---

## Dynamic Programming Checklist

For this problem:

```text
State:
dp[i] = best subarray sum ending at i

Transition:
dp[i] = max(nums[i], dp[i - 1] + nums[i])

Base Case:
dp[0] = nums[0]

Answer:
max(dp[i])

Optimization:
dp[i - 1] -> currentSum
```

This is the basic DP workflow:

```text
define state
→ derive transition
→ define base case
→ identify final answer
→ optimize space if possible
```

---

## Common Mistakes

- A subarray must be contiguous.
- Do not initialize `maxSum = 0` because all numbers may be negative.
- `currentSum` is not the final answer; keep a separate global maximum.
- Do not assume the best subarray ends at the final index.
- Kadane decides whether to continue or restart at every position.
- In the Prefix Sum approach, use the minimum prefix from before the current prefix.
- Update `maxSum` before updating `minPrefix`.

---

## Recall Keywords

`Dynamic Programming`

`dp[i] = best sum ending at i`

`continue vs restart`

`currentSum / maxSum`

`Kadane's Algorithm`

`state compression`

`Prefix Sum + minPrefix`

`O(n) / O(1)`