# Q014 Merge Intervals

## Problem

Given an array of intervals where:

```text
intervals[i] = [start, end]
```

merge all overlapping intervals and return the resulting non-overlapping intervals.

### Example

```text
Input:
[[1,3], [2,6], [8,10], [15,18]]

Output:
[[1,6], [8,10], [15,18]]
```

Because:

```text
[1,3] and [2,6]
```

overlap and merge into:

```text
[1,6]
```

---

## Pattern

Sorting / Intervals / Linear Scan

---

## Key Idea

Sort all intervals by their starting position.

After sorting, process intervals from left to right while maintaining the current merged interval:

```text
[currentStart, currentEnd]
```

For the next interval:

```text
[nextStart, nextEnd]
```

If:

```text
currentEnd >= nextStart
```

the intervals overlap.

Merge them by extending:

```text
currentEnd = max(currentEnd, nextEnd)
```

If they do not overlap, the current merged interval is complete and can be added to the result.

---

## Why Sorting Helps

Before sorting:

```text
[8,10]
[1,3]
[2,6]
```

it is difficult to know which intervals should be compared.

After sorting by start:

```text
[1,3]
[2,6]
[8,10]
```

we only need to compare the next interval with the current merged interval.

Sorting creates the ordering needed for a linear scan.

---

## Java

```java
public int[][] merge(int[][] intervals) {
    Arrays.sort(
        intervals,
        (a, b) -> Integer.compare(a[0], b[0])
    );

    List<int[]> result = new ArrayList<>();

    int i = 0;

    while (i < intervals.length) {
        int start = i;
        int currentMax = intervals[i][1];

        while (
            i < intervals.length - 1 &&
            currentMax >= intervals[i + 1][0]
        ) {
            i++;

            currentMax = Math.max(
                currentMax,
                intervals[i][1]
            );
        }

        result.add(new int[]{
            intervals[start][0],
            currentMax
        });

        i++;
    }

    return result.toArray(
        new int[result.size()][]
    );
}
```

---

## Overlap Condition

After sorting by start, suppose:

```text
current = [a, b]
next    = [c, d]
```

Since:

```text
a <= c
```

the intervals overlap when:

```text
b >= c
```

So:

```java
if (currentEnd >= nextStart)
```

means they can be merged.

---

## Merge Rule

When two intervals overlap:

```text
current = [1,10]
next    = [2,3]
```

the merged interval is still:

```text
[1,10]
```

Therefore, the new end must be:

```text
max(currentEnd, nextEnd)
```

not simply:

```text
nextEnd
```

---

## My Main Bug

My first version used:

```java
intervals[i][1] >= intervals[i + 1][0]
```

to decide whether the next interval overlaps.

This compares the next interval only with the immediately previous original interval.

That fails for:

```text
[[1,10], [2,3], [4,5]]
```

Because:

```text
[1,10] already covers [4,5]
```

but:

```text
[2,3] and [4,5]
```

do not overlap.

The correct state is:

```text
currentEnd = maximum end of the entire merged group
```

Then compare:

```text
currentEnd >= nextStart
```

---

## Second Bug

After correctly maintaining:

```java
currentMax
```

I initially returned:

```java
intervals[end][1]
```

instead of:

```java
currentMax
```

This also fails when an earlier interval has a larger end.

Example:

```text
[[1,10], [2,3], [4,5]]
```

The last interval ends at:

```text
5
```

but the merged interval must end at:

```text
10
```

So the result must use:

```java
currentMax
```

---

## Alternative Standard Structure

A common version stores the last merged interval directly:

```java
public int[][] merge(int[][] intervals) {
    Arrays.sort(
        intervals,
        (a, b) -> Integer.compare(a[0], b[0])
    );

    List<int[]> result = new ArrayList<>();

    for (int[] interval : intervals) {
        if (
            result.isEmpty() ||
            result.get(result.size() - 1)[1] < interval[0]
        ) {
            result.add(interval);
        } else {
            int[] last = result.get(result.size() - 1);

            last[1] = Math.max(
                last[1],
                interval[1]
            );
        }
    }

    return result.toArray(new int[result.size()][]);
}
```

Same idea:

```text
compare next interval with current merged interval
```

---

## Complexity

Sorting:

```text
O(n log n)
```

Linear scan:

```text
O(n)
```

Therefore:

```text
Time: O(n log n)
```

The result may contain up to `n` intervals:

```text
Output Space: O(n)
```

Ignoring the output, the scan itself uses only a few variables.

---

## Common Mistakes

- Forgetting to sort intervals first.
- Comparing with the previous original interval instead of the current merged interval.
- Using the latest interval's end instead of the maximum merged end.
- Forgetting that `[1,4]` and `[4,5]` overlap.
- New merged end must be:

```text
max(currentEnd, nextEnd)
```

- Sorting dominates the total time complexity.
- Nested `while` does not make the scan `O(n²)` because `i` only moves forward.

---

## Recall Keywords

`sort by start`

`currentStart / currentEnd`

`currentEnd >= nextStart`

`overlap`

`currentEnd = max(currentEnd, nextEnd)`

`finish current group when no overlap`

`sort + linear scan`

`O(n log n)`