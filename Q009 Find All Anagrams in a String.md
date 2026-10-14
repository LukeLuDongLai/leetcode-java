# Q009 Find All Anagrams in a String

## Problem

Given two strings `s` and `p`, return the starting indices of all substrings in `s` that are anagrams of `p`.

An anagram contains the same characters with the same frequencies, but possibly in a different order.

### Example

```text
Input:
s = "cbaebabacd"
p = "abc"

Output:
[0, 6]
```

The matching substrings are:

```text
"cba" -> starts at index 0
"bac" -> starts at index 6
```

---

## Pattern

Sliding Window / Frequency Count

## Key Idea

Anagrams must have:

- The same length
- The same frequency for every character

Since every candidate substring must have length `p.length()`, maintain a fixed-size sliding window.

Each time the window moves one position to the right:

- The leftmost character leaves: `count--`
- A new character enters from the right: `count++`

Then compare the character frequencies of the current window with those of `p`.

## Java

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();

    if (s.length() < p.length()) {
        return result;
    }

    int[] countS = new int[26];
    int[] countP = new int[26];

    for (int i = 0; i < p.length(); i++) {
        countP[p.charAt(i) - 'a']++;
        countS[s.charAt(i) - 'a']++;
    }

    for (int i = 0; i <= s.length() - p.length(); i++) {
        if (Arrays.equals(countS, countP)) {
            result.add(i);
        }

        countS[s.charAt(i) - 'a']--;

        int next = i + p.length();

        if (next < s.length()) {
            countS[s.charAt(next) - 'a']++;
        }
    }

    return result;
}
```

## Complexity

Let:

- `n = s.length()`
- `m = p.length()`

Complexity:

- Time: `O(n + m)`, usually simplified to `O(n)`
- Space: `O(1)`, because both frequency arrays always have size 26

`Arrays.equals()` compares 26 entries:

`O(26) = O(1)`

## Why Sliding Window?

Two adjacent substrings of length `m` differ by only two characters:

- One character leaves
- One character enters

Therefore, there is no need to recount every character for each substring.

Instead, update the previous window's frequency counts incrementally.

## Common Mistakes

- Return immediately if `s.length() < p.length()`.
- Use `Arrays.equals()` to compare array contents, not `==`.
- The window size must always be `p.length()`.
- `count[c - 'a']` assumes that the characters are lowercase letters from `a` to `z`.

## Recall Keywords

`Fixed-size Sliding Window`  
`frequency count`  
`remove left / add right`  
`Arrays.equals()`  
`reuse previous window state`  
`O(n) / O(1)`