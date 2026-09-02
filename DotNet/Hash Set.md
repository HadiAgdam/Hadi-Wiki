



A `HashSet<T>` is a collection designed for **unique values** and **fast lookup**.


```c#
HashSet<int> students = new() { 10, 20, 30, 40 };
```


The syntax is the same, but the performance is very different.


| Collection   | `.Contains()` |
| ------------ | ------------- |
| `List<T>`    | O(n)          |
| `HashSet<T>` | O(1) average  |


---
```c#
var ids = new HashSet<int>();

ids.Add(10);
ids.Add(20);
ids.Add(10);
ids.Add(30);
```

```
10
20
30
```

You can also see whether adding actually happened:

```c#
bool added = ids.Add(10);
```

---

### HashSet has actual set operations

```c#
var R = new HashSet<int> { 1, 2, 3, 4, 5 };
var A = new HashSet<int> { 2, 4 };


var B = R.Except(A);
```

```
1
3
5
```


---

### A `HashSet` is **not primarily for accessing items by index**.


This is normal with a List:

```c#
students[0]
students[1]
```

A HashSet doesn't work like that.

It's designed for questions like:

```c#
Contains()
Add()
Remove()
```

### `List<T>`

> "Give me the collection of students in this order."

### `HashSet<T>`

> "Tell me whether this student exists."

That's the mental model I'd recommend.