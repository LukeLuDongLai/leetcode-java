# Q005 Container With Most Water

## Problem

Given an integer array `height`, where each element represents the height of a vertical line, choose two lines that together with the x-axis form a container.

Return the maximum amount of water the container can hold.

### Example

```text
Input:
[1, 8, 6, 2, 5, 4, 8, 3, 7]

Output:
49
```

The maximum area is formed by the lines at indices `1` and `8`:

```text
width = 8 - 1 = 7
height = min(8, 7) = 7

area = 7 * 7 = 49
```

---

## Pattern

Two Pointers / Opposite Direction

## Key Idea

Use two pointers:

- `left`: left boundary
- `right`: right boundary

The current area is:

`(right - left) * min(height[left], height[right])`

Move the shorter side each time. Since the width always decreases, the only chance to get a larger area is to find a taller boundary.

## Java

```java
public int maxArea(int[] height) {
    int left = 0;
    int right = height.length - 1;
    int maxArea = 0;

    while (left < right) {
        int width = right - left;
        int effectiveHeight = Math.min(height[left], height[right]);

        maxArea = Math.max(
            maxArea,
            width * effectiveHeight
        );

        if (height[left] > height[right]) {
            right--;
        } else {
            left++;
        }
    }

    return maxArea;
}
```

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Why Move the Shorter Side?

The area is limited by the shorter boundary:

`area = width * min(leftHeight, rightHeight)`

If we move the taller side:

- the width becomes smaller
- the shorter boundary stays the same
- the area cannot improve

Therefore, move the shorter side and try to find a taller boundary.

## Common Mistakes

- Width is `right - left`, not `right - left + 1`.
- Effective height is `Math.min(...)`.
- Move the shorter side, not the taller side.
- Both pointers only move inward, so the total time complexity is `O(n)`.

## Recall Keywords

`Two Pointers`  
`width * shorter height`  
`move the shorter side`  
`O(n) / O(1)`