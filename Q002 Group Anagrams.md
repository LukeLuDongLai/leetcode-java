# Q002 Group Anagrams

## Problem

Given an array of strings `strs`, group together all strings that are anagrams of each other.

Two strings are anagrams if they contain the same characters with the same frequencies, but possibly in a different order.

### Example

```text
Input:
["eat", "tea", "tan", "ate", "nat", "bat"]

Output:
[
  ["eat", "tea", "ate"],
  ["tan", "nat"],
  ["bat"]
]
```

The order of the groups and the strings inside each group does not matter.

---

## Pattern

HashMap / Grouping / Canonical Representation

## Key Idea

Convert all anagrams into the same canonical key, then use a `HashMap` to group strings with the same key.

My approach:

- Count the frequency of all 26 lowercase letters.
- Reconstruct a canonical string based on those frequencies.
- Put strings with the same canonical key into the same `List`.

## Java

```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();

    for (String str : strs) {
        String key = toAnagram(str);

        if (!map.containsKey(key)) {
            map.put(key, new ArrayList<>());
        }

        map.get(key).add(str);
    }

    return new ArrayList<>(map.values());
}

private String toAnagram(String str) {
    int[] count = new int[26];

    for (int i = 0; i < str.length(); i++) {
        count[str.charAt(i) - 'a']++;
    }

    StringBuilder sb = new StringBuilder();

    for (int i = 0; i < 26; i++) {
        char ch = (char) ('a' + i);

        for (int j = 0; j < count[i]; j++) {
            sb.append(ch);
        }
    }

    return sb.toString();
}
```

## Complexity

Let:

- `n` = number of strings
- `k` = average / maximum string length

Complexity:

- Time: `O(n * k)`
- Space: `O(n * k)`

## Common Mistakes

- `count[ch - 'a']` assumes that all characters are lowercase letters from `a` to `z`.
- Nested loops do not necessarily mean `O(k²)`. The total of all `count[i]` values is only `k`.
- `List` is an interface and cannot be instantiated with `new List<>()`; use `new ArrayList<>()`.
- The grouped values can be returned directly with:

```java
new ArrayList<>(map.values())
```

## Alternative Approach

Sort each string and use the sorted string as the key:

```java
char[] chars = str.toCharArray();
Arrays.sort(chars);
String key = new String(chars);
```

Complexity:

- Time: `O(n * k log k)`

This approach is simpler, but sorting each string is asymptotically slower than character counting when the character set is fixed.

## Recall Keywords

`anagram -> same characteristics -> same key -> HashMap grouping`

`canonical representation`

`character counting avoids sorting every string`