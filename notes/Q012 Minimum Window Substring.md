# Q012 Minimum Window Substring

## Problem

Given two strings `s` and `t`, return the shortest substring of `s` that contains every character in `t`, including duplicate characters.

If no valid substring exists, return an empty string.

### Example

```text
Input:
s = "ADOBECODEBANC"
t = "ABC"

Output:
"BANC"
```

---

## Pattern

Variable-Size Sliding Window / HashMap / Frequency Count

## Key Idea

Maintain a window:

```text
[left, right]
```

The window expands with `right` until it contains all required characters.

Once the window becomes valid:

```text
shrink from left
```

while it is still valid.

The goal is:

```text
expand until valid
shrink while valid
```

This finds the minimum valid window.

---

## State

`need` stores the required frequency of each character in `t`.

```text
character -> required count
```

`window` stores the frequency inside the current window.

```text
character -> current count
```

Example:

```text
t = "AABC"

need:
A -> 2
B -> 1
C -> 1
```

---

## `required` and `matched`

Define:

```text
required = number of distinct characters in t
matched = number of character types currently satisfying their required count
```

For:

```text
t = "AABC"
```

we have:

```text
required = 3
```

If the current window contains:

```text
A -> 4
B -> 1
C -> 1
```

then:

```text
matched = 3
```

The extra `A`s do not increase `matched`.

The window is valid when:

```java
matched == required
```

---

## Standard Solution

```java
public String minWindow(String s, String t) {
    int bestStart = 0;
    int bestLength = 0;

    int left = 0;

    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();

    for (char c : t.toCharArray()) {
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int required = need.size();
    int matched = 0;

    for (int right = 0; right < s.length(); right++) {
        char cRight = s.charAt(right);

        if (need.containsKey(cRight)) {
            window.put(
                cRight,
                window.getOrDefault(cRight, 0) + 1
            );

            int windowCount = window.get(cRight);
            int needCount = need.get(cRight);

            if (windowCount == needCount) {
                matched++;
            }
        }

        while (matched == required) {
            int length = right - left + 1;

            if (bestLength == 0 || length < bestLength) {
                bestStart = left;
                bestLength = length;
            }

            char cLeft = s.charAt(left);

            if (need.containsKey(cLeft)) {
                window.put(
                    cLeft,
                    window.get(cLeft) - 1
                );

                if (window.get(cLeft) < need.get(cLeft)) {
                    matched--;
                }
            }

            left++;
        }
    }

    return bestLength == 0
        ? ""
        : s.substring(bestStart, bestStart + bestLength);
}
```

---

## Why Shrink Only While Valid?

Once:

```text
matched == required
```

the current window contains everything needed.

But it may contain unnecessary characters.

So keep moving `left` to find a smaller valid window.

The moment removing a character causes:

```text
matched < required
```

the window becomes invalid.

Stop shrinking and let `right` expand again.

This is the key control flow:

```text
expand right
    ↓
window becomes valid
    ↓
shrink left while valid
    ↓
window becomes invalid
    ↓
expand right again
```

---

## When Does `matched` Increase?

Suppose:

```text
need[A] = 2
```

Window count changes:

```text
0 -> 1 -> 2 -> 3
```

`matched` increases only when:

```text
1 -> 2
```

because that is the moment `A` becomes satisfied.

So:

```java
if (windowCount == needCount) {
    matched++;
}
```

---

## When Does `matched` Decrease?

Suppose:

```text
need[A] = 2
```

Window count changes while shrinking:

```text
3 -> 2 -> 1
```

`A` is still satisfied at:

```text
2
```

but becomes unsatisfied at:

```text
1
```

So:

```java
if (window.get(cLeft) < need.get(cLeft)) {
    matched--;
}
```

---

## Why Not Use a Set?

A `Set` only tells us whether a character exists.

But duplicates matter.

For:

```text
t = "AABC"
```

the window needs:

```text
A twice
B once
C once
```

So frequency counts are required.

---

## Why Variable-Size Sliding Window?

The answer length is unknown.

Unlike a fixed-size window problem, we must dynamically:

```text
expand when invalid
shrink when valid
```

The two pointers cooperate:

```text
right -> obtains enough characters
left  -> removes unnecessary characters
```

---

## Complexity

Let:

```text
n = s.length()
m = t.length()
```

Building `need`:

```text
O(m)
```

`right` moves at most `n` times.

`left` also moves at most `n` times.

Therefore:

```text
Time: O(n + m)
```

Usually simplified to:

```text
O(n)
```

when focusing on scanning `s`.

With HashMaps:

```text
Space: O(m)
```

More precisely, it depends on the number of distinct characters in `t`.

---

## My Main Bug

My first version correctly detected when the window became valid, but then continued moving `left` until it reached `right`.

That destroyed useful characters even after the window had already become invalid.

Wrong idea:

```text
valid
→ keep shrinking until left reaches right
```

Correct idea:

```text
valid
→ shrink while valid
→ stop immediately when invalid
```

The important lesson:

**It is not enough to know when to start shrinking. You must also know exactly when to stop shrinking.**

---

## Java Pitfall: `Integer == Integer`

`Map<Character, Integer>.get()` returns `Integer`.

Avoid relying on:

```java
window.get(c) == need.get(c)
```

because `==` may compare object references.

Safer:

```java
int windowCount = window.get(c);
int needCount = need.get(c);

if (windowCount == needCount) {
    ...
}
```

or use `.equals()`.

---

## Common Mistakes

- `substring` must be continuous.
- Duplicate characters in `t` must be counted.
- Use frequency counts, not only a `Set`.
- `matched` counts satisfied character types, not total matched characters.
- Increase `matched` only when a character just reaches its required count.
- Decrease `matched` only when shrinking makes its count insufficient.
- Shrink only while the window is valid.
- Stop shrinking immediately when the window becomes invalid.
- Window length is `right - left + 1`.
- Avoid comparing `Integer` objects with `==`.

---

## Recall Keywords

`Variable-size Sliding Window`

`need / window`

`required / matched`

`expand until valid`

`shrink while valid`

`frequency count`

`matched == required`

`stop shrinking when invalid`

`O(n + m)`