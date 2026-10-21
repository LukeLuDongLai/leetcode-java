# Q016 — Product of Array Except Self

**LeetCode:** [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)  
**Difficulty:** Medium  
**Pattern:** Prefix & Suffix Products

## 1. Problem

Given an integer array `nums`, return an array `answer` where `answer[i]` equals the product of all elements except `nums[i]`.

**Requirements:**
- Do not use division.
- O(n) time complexity.
- Follow-up: O(1) extra space (excluding the output array).

**Example:**

```text
Input:  nums = [1, 2, 3, 4]
Output:        [24, 12, 8, 6]
```

## 2. My Solution

**Approach:** Two passes using prefix and suffix products.

- First pass (left → right): Store the product of all elements before `i` in `answer[i]`.
- Second pass (right → left): Multiply `answer[i]` by the product of all elements after `i`.
- Use running product variables instead of additional arrays.

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] answer = new int[nums.length];

        int leftProduct = 1;
        for (int i = 0; i < nums.length; i++) {
            answer[i] = leftProduct;
            leftProduct *= nums[i];
        }

        int rightProduct = 1;
        for (int j = nums.length - 1; j >= 0; j--) {
            answer[j] *= rightProduct;
            rightProduct *= nums[j];
        }

        return answer;
    }
}
```

**Complexity:**
- Time: **O(n)** — Two linear passes.
- Extra Space: **O(1)** — Output array excluded.

## 3. Official / Standard Solution

My solution is equivalent to the standard optimal **Prefix & Suffix Products** approach.

The key formula is:

```text
answer[i] = leftProduct(i) × rightProduct(i)
```

Where:
- `leftProduct(i)` excludes `nums[i]` and everything to its right.
- `rightProduct(i)` excludes `nums[i]` and everything to its left.

The standard optimization stores prefix products directly in the output array and calculates suffix products using one running variable.

No additional implementation is needed.

## 4. Keywords

- **Pattern:** Prefix Product + Suffix Product
- **Direction:** Left-to-right pass + right-to-left pass
- **Formula:** `answer[i] = leftProduct × rightProduct`
- **Initialization:** Product starts at `1`, not `0`.
- **Critical Order:** Use the running product **before** multiplying by `nums[i]` to exclude the current element.
- **Optimization:** Reuse the output array to avoid auxiliary arrays.
- **Edge Cases:** Zeros and negative numbers work without special handling.
- **Complexity:** `O(n)` time, `O(1)` extra space.