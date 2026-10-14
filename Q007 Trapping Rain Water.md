# Q007 Trapping Rain Water

## Problem

Given an array `height` where `height[i]` represents the height of a vertical bar, compute how much rainwater can be trapped after raining.

### Example

```text
Input:
[4, 2, 0, 3, 2, 5]

Output:
9
```

---

## Pattern

Prefix / Suffix Maximum / Two Pointers

## Core Formula

The water trapped at position `i` is:

`min(leftMax, rightMax) - height[i]`

Key idea:

The water level is limited by the lower of the highest walls on the left and right.

---

## My Approach: Split by the Global Maximum

First find the tallest bar and its index `maxIndex`.

### Left Side

The global maximum is guaranteed to be on the right.

So the right boundary is always high enough, and we only need to maintain:

`maxLeft`

Water at the current position:

`maxLeft - height[i]`

### Right Side

The global maximum is guaranteed to be on the left.

So we only need to maintain:

`maxRight`

Water at the current position:

`maxRight - height[i]`

```java
public int trap(int[] height) {
    int maxHeight = 0;
    int maxIndex = 0;

    for (int i = 0; i < height.length; i++) {
        if (height[i] > maxHeight) {
            maxHeight = height[i];
            maxIndex = i;
        }
    }

    int water = 0;

    int maxLeft = 0;
    for (int i = 0; i < maxIndex; i++) {
        maxLeft = Math.max(maxLeft, height[i]);
        water += maxLeft - height[i];
    }

    int maxRight = 0;
    for (int i = height.length - 1; i > maxIndex; i--) {
        maxRight = Math.max(maxRight, height[i]);
        water += maxRight - height[i];
    }

    return water;
}
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`

Finding the global maximum scans the array once.

The left and right scans together cover about `n` elements:

`n + (n - 1) -> O(n)`

---

## Standard Two-Pointer Approach

Maintain:

- `leftMax`
- `rightMax`

Whichever side has the smaller maximum boundary can be processed safely.

```java
public int trap(int[] height) {
    int left = 0;
    int right = height.length - 1;

    int leftMax = 0;
    int rightMax = 0;
    int water = 0;

    while (left <= right) {
        if (leftMax <= rightMax) {
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left];
            left++;
        } else {
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right];
            right--;
        }
    }

    return water;
}
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`

## Why the Two-Pointer Approach Works

If:

`leftMax <= rightMax`

then we already know that the right side has a boundary at least as high as `leftMax`.

Therefore, the water level at the current `left` position is determined by `leftMax`, so it is safe to process `left`.

The right side is symmetric.

## Connection Between the Two Approaches

My approach:

`find global maximum -> process left and right separately`

Two-pointer approach:

`do not find the global maximum first -> dynamically decide which side is safe to process`

Both rely on the same principle:

**Once the lower maximum boundary is known, the trapped water at that position can be determined.**

## Common Mistakes

- This is not the same formula as Q005's `width * height`.
- Total trapped water is the sum of water at each position.
- Water level depends on `min(leftMax, rightMax)`.
- The left and right scans together still cover only about `n` positions.
- In the two-pointer solution, pointer movement depends on `leftMax / rightMax`, not only on the current bar heights.

## Recall Keywords

`min(leftMax, rightMax) - height[i]`  
`split by global maximum`  
`leftMax / rightMax`  
`lower boundary determines water level`  
`Two Pointers`  
`O(n) / O(1)`