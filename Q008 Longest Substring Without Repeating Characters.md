# Q008 Longest Substring Without Repeating Characters

## Problem

Given a string `s`, return the length of the longest substring without repeating characters.

A substring must contain consecutive characters from the original string.

### Example

```text
Input:
"abcabcbb"

Output:
3
```

The longest substring without repeating characters is:

```text
"abc"
```

---

## Pattern

Sliding Window / HashMap / Two Pointers

## Key Idea

Maintain a window that contains no duplicate characters.

- `right`: expands the window to the right
- `left`: marks the left boundary of the window
- `Map<Character, Integer>`: stores the most recent index of each character

When a duplicate character appears, use its previous index to move `left`.

---

## My Approach: Remove Characters One by One

When a duplicate character is found:

- Find its previous index
- Move `left` forward
- Remove characters that leave the window from the map

```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();

    int left = 0;
    int right = 0;
    int maxLength = 0;

    while (right < s.length()) {
        char c = s.charAt(right);

        if (map.containsKey(c)) {
            maxLength = Math.max(maxLength, right - left);

            int index = map.get(c);

            while (left <= index) {
                map.remove(s.charAt(left));
                left++;
            }
        }

        map.put(c, right);
        right++;
    }

    maxLength = Math.max(maxLength, right - left);

    return maxLength;
}
```

### Complexity

- Time: `O(n)`
- Space: `O(n)`

Although there is a nested `while` loop, both `left` and `right` only move forward.

Each character enters and leaves the window at most once, so the total work is still linear.

---

## Optimized Approach: Jump `left` Directly

The map does not need to contain only characters inside the current window.

Instead, let it always store:

`character -> most recent index`

When a duplicate character appears, move `left` directly:

```java
left = Math.max(left, map.get(c) + 1);
```

There is no need to remove old characters one by one.

```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();

    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);

        if (map.containsKey(c)) {
            left = Math.max(left, map.get(c) + 1);
        }

        map.put(c, right);

        maxLength = Math.max(
            maxLength,
            right - left + 1
        );
    }

    return maxLength;
}
```

## Why Use `Math.max`?

The previous occurrence of a character may already be outside the current window.

For example:

```text
"abba"
```

When processing the final `a`:

```text
previous index of a = 0
left = 2
```

We must not move:

```text
left -> 1
```

because the left boundary should never move backward.

Therefore:

`left = max(left, lastIndex + 1)`

---

## Optimization Insight

My approach:

`duplicate -> remove characters one by one -> left++`

Optimized approach:

`duplicate index already known -> jump directly`

Important idea:

**If a loop only moves toward a position that is already known, consider jumping directly to that position.**

Old data does not always need to be deleted if we can determine whether it is already outside the current valid range.

## Common Mistakes

- A `substring` must be consecutive.
- The length of window `[left, right]` is `right - left + 1`.
- `left` must never move backward.
- The longest valid window may continue until the end of the string.
- The map stores the most recent index of each character.

## Recall Keywords

`Sliding Window`  
`last occurrence index`  
`left / right`  
`left never moves backward`  
`remove one by one -> direct jump`  
`O(n)`