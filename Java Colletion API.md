# Java API for LeetCode

### Array / String

```
int[] nums;

nums.length

String s;

s.length();
s.charAt(i);
s.substring(a, b);

StringBuilder sb = new StringBuilder();
sb.append(x);
sb.reverse();
```

### HashMap

```
Map<Integer, Integer> map = new HashMap<>();

map.put(key, value);

map.get(key);

map.containsKey(key);

map.getOrDefault(key, 0);

map.remove(key);
```

### HashSet

```
Set<Integer> set = new HashSet<>();

set.add(x);

set.contains(x);

set.remove(x);
```

### List

```
List<Integer> list = new ArrayList<>();

list.add(x);

list.get(i);

list.size();
```

### Stack / Queue

 `Deque`：

```
Deque<Integer> deque = new ArrayDeque<>();
```

Queue：

```
deque.offer(x);
deque.poll();
deque.peek();
```

Stack：

```
deque.push(x);
deque.pop();
deque.peek();
```

### Heap

```
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
```

Max Heap：

```
PriorityQueue<Integer> maxHeap =
    new PriorityQueue<>(Comparator.reverseOrder());
```

