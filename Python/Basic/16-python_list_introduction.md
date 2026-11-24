# Python Lists: Introduction
### Beginner → Advanced Notes

**Topics in this section:** List data type · Operations · Methods · Comprehensions · Nested lists
**These notes cover:** what a list is, creation, indexing and slicing (read and write), operations, and traversal, with a short preview of the rest.

---

## Table of Contents
1. [What is a List?](#1-what-is-a-list)
2. [Creating a List](#2-creating-a-list)
3. [Representation](#3-representation)
4. [Heterogeneous Elements](#4-heterogeneous-elements)
5. [Lists are Mutable](#5-lists-are-mutable)
6. [Indexing and Slicing: Reading](#6-indexing-and-slicing-reading)
7. [Indexing and Slicing: Writing](#7-indexing-and-slicing-writing)
8. [Operations on Lists](#8-operations-on-lists)
9. [List Traversal](#9-list-traversal)
10. [Preview: Methods, Comprehensions, Nested Lists](#10-preview-methods-comprehensions-nested-lists)
11. [Cheat Sheet](#11-cheat-sheet)
12. [Common Mistakes](#12-common-mistakes)
13. [Practice Questions](#13-practice-questions)

---

## 1. What is a List?

### Beginner level

A **list** is a **collection of elements** stored under **one name**, written inside **square brackets** and separated by commas.

```python
L1 = [1, 2, 3, 4, 5, 6]
L2 = [1.1, 1.2, 1.3, 1.4]
L3 = ['john', 'mary', 'anil']
```

### Key properties

| Property                  | Meaning                                                           |
|---------------------------|-------------------------------------------------------------------|
| **Ordered**               | elements keep the position you gave them; each has an **index**   |
| **Mutable**               | you can **change, add and remove** elements after creating it     |
| **Allows duplicates**     | the same value can appear many times: `[1, 1, 2]`                 |
| **Heterogeneous**         | elements can be of **different data types**                       |
| **Dynamic size**          | grows and shrinks as needed                                       |
| **Indexable / sliceable** | `L[0]`, `L[1:3]`                                                  |

```python
print(type([1, 2]))     # <class 'list'>
print(len([1, 2, 3]))   # 3
```

### Advanced: how it works inside

- A list stores **references (pointers)** to objects, not the objects themselves. That is why it can hold any mix of types.
- It is implemented as a **dynamic array**.

| Operation                    | Cost                         |
|------------------------------|------------------------------|
| Read/write `L[i]`            | O(1) (very fast)             |
| `append()` at the end        | O(1) on average              |
| Insert/delete at the front or middle | O(n) (elements shift) |
| `x in L` (search)            | O(n) (checks one by one)     |
| `len(L)`                     | O(1)                         |

### Lists vs other collections

| Type    | Ordered | Mutable | Duplicates | Written as     |
|---------|---------|---------|------------|----------------|
| `list`  | ✅      | ✅      | ✅         | `[1, 2, 3]`    |
| `tuple` | ✅      | ❌      | ✅         | `(1, 2, 3)`    |
| `set`   | ❌      | ✅      | ❌         | `{1, 2, 3}`    |
| `dict`  | ✅ (by insertion) | ✅ | keys unique | `{'a': 1}`   |

---

## 2. Creating a List

### Method 1: square brackets `[ ]`

```python
L1 = [1, 2, 3, 4, 5]
L4 = []                  # empty list
```

### Method 2: `list(iterable)`

`list()` converts **any iterable** (tuple, string, range, set, dictionary...) into a list.

```python
L2 = list((3, 5, 79))        # from a tuple
print(L2)                    # [3, 5, 79]

L3 = list('abcde')           # from a string
print(L3)                    # ['a', 'b', 'c', 'd', 'e']

print(list(range(5)))        # [0, 1, 2, 3, 4]
print(list(range(10, 0, -3)))# [10, 7, 4, 1]
print(list({'x': 1, 'y': 2}))# ['x', 'y']     (keys of a dictionary)
print(list())                # []            (empty list)
```

> 📝 In your notes `list((3,5,,79))` has a double comma. It must be `list((3, 5, 79))`. Note the **double brackets**: the inner `(3, 5, 79)` is a single tuple argument.

### Method 3: other common ways

```python
print([0] * 5)                         # [0, 0, 0, 0, 0]   (repetition)
print([x * x for x in range(1, 6)])    # [1, 4, 9, 16, 25] (comprehension)
print('red green blue'.split())        # ['red', 'green', 'blue']
print(input().split())                 # list of words typed by the user
```

### Reading numbers from the user

```python
nums = [int(x) for x in input("Enter numbers: ").split()]
print(nums)       # input "4 8 15"  ->  [4, 8, 15]
```

---

## 3. Representation

Lists are printed inside **square brackets**, elements separated by **comma + space**. Strings inside the list are shown **with quotes**.

```python
L = [1, 'two', 3.0]
print(L)             # [1, 'two', 3.0]
print(str(L))        # "[1, 'two', 3.0]"
print(len(L))        # 3
print(L == [1, 'two', 3.0])   # True
```

An empty list is **falsy**, a non-empty list is **truthy**:

```python
if not []:
    print("empty")          # runs
if [0]:
    print("non-empty")      # runs (a list containing 0 is still non-empty)
```

---

## 4. Heterogeneous Elements

A list can mix **different data types**.

```python
L1 = [7, 3.2, 'john', True, 5 + 6j]

for x in L1:
    print(x, type(x).__name__)
```

```
7 int
3.2 float
john str
True bool
(5+6j) complex
```

> 📝 In your notes the complex number was written `5+6i`. Python uses **`j`**, so write `5+6j`.

A list can even hold **other lists, tuples, dictionaries and functions**:

```python
L = [1, [2, 3], (4, 5), {'a': 6}, len]
```

> Good practice: although Python allows it, lists usually hold **similar** items (all numbers, all names). Mix types only when the data really belongs together.

---

## 5. Lists are Mutable

**Mutable** = the list can be **changed in place** after it is created.

```python
L1 = [1, 2, 3, 4, 5]

L1[3] = 10          # change an element
print(L1)           # [1, 2, 3, 10, 5]

L1.append(25)       # add at the end
print(L1)           # [1, 2, 3, 10, 5, 25]

L1.remove(10)       # remove the first element equal to 10
print(L1)           # [1, 2, 3, 5, 25]
```

### Same object, new contents

```python
L = [1, 2, 3]
before = id(L)
L.append(4)
print(id(L) == before)     # True  (same list object was modified)
```

Compare with **strings and tuples**, which are immutable:

```python
s = 'hello'
s[0] = 'j'          # ❌ TypeError
t = (1, 2, 3)
t[0] = 9            # ❌ TypeError
```

### Intermediate: two names, one list (aliasing)

```python
a = [1, 2]
b = a              # NOT a copy: both names point to the same list
b.append(3)
print(a)           # [1, 2, 3]   (a changed too!)
```

Make a real (shallow) copy with any of these:

```python
b = a.copy()
b = a[:]
b = list(a)
```

### Advanced: shallow vs deep copy

```python
import copy

a = [[1, 2], [3, 4]]
b = a.copy()              # shallow: inner lists are shared
b[0].append(99)
print(a)                  # [[1, 2, 99], [3, 4]]

c = copy.deepcopy(a)      # deep: everything copied
c[0].append(100)
print(a)                  # unchanged by c
```

### Methods that change the list return `None`

```python
L = [3, 1, 2]
x = L.sort()
print(x)           # None
print(L)           # [1, 2, 3]
L = L.sort()       # ❌ common mistake: L becomes None
```

---

## 6. Indexing and Slicing: Reading

Because a list is mutable, indexing and slicing can be used for both **reading** and **writing**. First, reading.

### Index map

```python
L1 = [3, 6, 9, 15, 18, 21]
```

```
value :    3    6    9   15   18   21
index :    0    1    2    3    4    5        <- positive (left to right)
neg   :   -6   -5   -4   -3   -2   -1        <- negative (right to left)
```

### Reading with an index

```python
print(L1[4])       # 18
print(L1[-3])      # 15
x = L1[-3]
print(x)           # 15
print(L1[0], L1[-1])    # 3 21   (first and last)
```

An index outside the range raises an error:

```python
print(L1[6])       # IndexError: list index out of range
print(L1[-7])      # IndexError
```

Valid indexes: `-len(L)` to `len(L) - 1`.

### Reading with a slice

```python
L[start:stop:step]
```

| Part    | Meaning                          | Default (step > 0) |
|---------|----------------------------------|--------------------|
| `start` | first index (**included**)       | `0`                |
| `stop`  | end index (**NOT included**)     | `len(L)`           |
| `step`  | gap between indexes              | `1`                |

> ✅ Same rule as `range()` and strings: the **last (stop) index is not taken**.

```python
L1 = [3, 6, 9, 15, 18, 21]

print(L1[2:5])         # [9, 15, 18]            indexes 2, 3, 4
print(L1[-5:-2])       # [6, 9, 15]             indexes 1, 2, 3
print(L1[::1])         # [3, 6, 9, 15, 18, 21]  whole list (a copy)
print(L1[::2])         # [3, 9, 18]             every 2nd element
print(L1[::-1])        # [21, 18, 15, 9, 6, 3]  reversed
print(L1[4:0:-1])      # [18, 15, 9, 6]         indexes 4, 3, 2, 1 (0 not included)
print(L1[-3:-7:-1])    # [15, 9, 6, 3]          from index 3 backwards to the start
```

More useful forms:

```python
print(L1[:3])          # [3, 6, 9]       first three
print(L1[3:])          # [15, 18, 21]    from index 3 to the end
print(L1[-2:])         # [18, 21]        last two
print(L1[:-1])         # [3, 6, 9, 15, 18]  all except the last
print(L1[1::2])        # [6, 15, 21]     odd positions
```

### Rules to remember

| Rule                                                         | Example                         |
|--------------------------------------------------------------|---------------------------------|
| Slicing **never raises IndexError**; extra range is trimmed  | `L1[2:100]` → `[9, 15, 18, 21]` |
| An empty slice gives `[]`                                    | `L1[4:2]` → `[]`                |
| A slice always creates a **new list** (shallow copy)         | `L1[:]` is a copy               |
| With a **negative step**, `start` should be larger than `stop` | `L1[5:1:-1]` → `[21, 18, 15, 9]` |
| `step` cannot be `0`                                         | `L1[::0]` → `ValueError`        |

---

## 7. Indexing and Slicing: Writing

### Writing with an index: replaces ONE element

```python
L1 = [1, 2, 3, 4, 5]
L1[3] = 10
print(L1)           # [1, 2, 3, 10, 5]
```

If you assign a **list** to an **index**, the list becomes **one element** (a nested list):

```python
L1 = [1, 2, 3, 4, 5]
L1[3] = [10, 11]
print(L1)           # [1, 2, 3, [10, 11], 5]
```

An index that does not exist **cannot** be assigned:

```python
L1[10] = 5          # ❌ IndexError: list assignment index out of range
```

### Writing with a slice: replaces MANY elements

```python
L[start:stop] = [any number of elements]          # simple slice
L[start:stop:step] = [exact number of elements]   # extended slice (step is not 1)
```

| Slice form               | Number of new elements needed        | List size can change? |
|--------------------------|--------------------------------------|-----------------------|
| `L[a:b] = iterable`      | **any** number (even zero)           | ✅ yes (grow/shrink)  |
| `L[a:b:step] = iterable` (step ≠ 1) | **exactly** as many as the slice | ❌ no            |

> The value on the right must be an **iterable** (usually a list). `L[0:0] = 10` is an error.

### Insert elements: use an empty slice `L[i:i]`

```python
l1 = [1, 2, 3, 4, 5, 6]
l1[3:3] = [10]
print(l1)           # [1, 2, 3, 10, 4, 5, 6]        inserted before index 3
```

```python
l1 = [1, 2, 3, 4, 5, 6]
l1[3:3] = [10, 11, 12]
print(l1)           # [1, 2, 3, 10, 11, 12, 4, 5, 6]   insert several at once
```

```python
l1 = [1, 2, 3, 4, 5, 6]
l1[9:9] = [20]
print(l1)           # [1, 2, 3, 4, 5, 6, 20]        index 9 is beyond the end, so it is added at the end
```

```python
l1 = [1, 2, 3, 4, 5, 6]
l1[0:0] = 10        # ❌ TypeError: can only assign an iterable
l1[0:0] = [10]      # ✅ [10, 1, 2, 3, 4, 5, 6]
```

### Replace elements with a different count

The slice `[1:4]` selects **3 elements** (indexes 1, 2, 3).

```python
l1 = [1, 2, 3, 4, 5, 6]
l1[1:4] = [10]
print(l1)           # [1, 10, 5, 6]               3 elements replaced by 1 (list shrinks)
```

```python
l2 = [1, 2, 3, 4, 5, 6]
l2[1:4] = [10, 11]
print(l2)           # [1, 10, 11, 5, 6]           3 replaced by 2 (shrinks)
```

```python
l1 = [1, 2, 3, 4, 5, 6]
l1[1:4] = [10, 11, 12, 14, 13]
print(l1)           # [1, 10, 11, 12, 14, 13, 5, 6]   3 replaced by 5 (grows)
```

> 📝 In your notes `l1=[1:4]=[10,11,12,14,13]` is a typo for `l1[1:4] = [10, 11, 12, 14, 13]`.

### Extended slices (with a step): exact count required

```python
l1 = [1, 2, 3, 4, 5, 6, 7]
l1[::-1] = [10, 11, 12, 13, 14]
```

```
ValueError: attempt to assign sequence of size 5 to extended slice of size 7
```

The slice `[::-1]` selects **all 7** elements, so exactly **7** new values are needed:

```python
l1 = [1, 2, 3, 4, 5, 6, 7]
l1[::-1] = [10, 11, 12, 13, 14, 15, 16]
print(l1)           # [16, 15, 14, 13, 12, 11, 10]    values are written from the right
```

Another one: `l1[3:0:-1]` selects indexes 3, 2, 1, which is **3** elements.

```python
l1 = [1, 2, 3, 4, 5, 6, 7]
l1[3:0:-1] = [12, 13, 14]
print(l1)           # [1, 14, 13, 12, 5, 6, 7]
# index 3 <- 12,  index 2 <- 13,  index 1 <- 14
```

With a positive step:

```python
l = [1, 2, 3, 4, 5, 6]
l[::2] = [0, 0, 0]          # indexes 0, 2, 4 (three elements)
print(l)                    # [0, 2, 0, 4, 0, 6]
```

### Delete with `del` or an empty slice

```python
L = [0, 1, 2, 3, 4, 5]
del L[2]            # [0, 1, 3, 4, 5]          remove one element

L = [0, 1, 2, 3, 4, 5]
del L[1:3]          # [0, 3, 4, 5]             remove a slice
L = [0, 1, 2, 3, 4, 5]
L[1:3] = []         # [0, 3, 4, 5]             same result

L = [0, 1, 2, 3, 4, 5]
del L[::2]          # [1, 3, 5]                remove every 2nd element

L = [0, 1, 2, 3, 4, 5]
del L[:]            # []                       empty the list (same as L.clear())
```

### Handy slice-assignment patterns

| Task                        | Code                         |
|-----------------------------|------------------------------|
| Insert at the beginning     | `L[:0] = [x, y]`             |
| Add at the end              | `L[len(L):] = [x, y]` (same as `extend`) |
| Replace everything          | `L[:] = [7, 8, 9]`           |
| Insert at index `i`         | `L[i:i] = [x]` (same as `L.insert(i, x)`) |
| Remove a part               | `L[a:b] = []`                |

### Advanced notes

- The right side can be **any iterable**, not only a list:

```python
L = [1, 2, 3]
L[1:1] = 'ab'
print(L)            # [1, 'a', 'b', 2, 3]
```

- `L[:] = ...` changes the list **in place** (other names pointing to it see the change), while `L = ...` just points the name to a new list.

---

## 8. Operations on Lists

| Operation       | Operator                    | Result                          |
|-----------------|-----------------------------|---------------------------------|
| Concatenation   | `+`                         | new list joining two lists      |
| Repetition      | `*`                         | new list repeated n times       |
| Membership      | `in`, `not in`              | `True` / `False`                |
| Comparison      | `<  <=  >  >=  ==  !=`      | `True` / `False`                |

### Concatenation with `+`

```python
L1 = [1, 2, 3]
L2 = [8, 9, 10]

L3 = L1 + L2
print(L3)            # [1, 2, 3, 8, 9, 10]

print(L2 + [11])     # [8, 9, 10, 11]
print(L2)            # [8, 9, 10]    (L2 is unchanged: + makes a NEW list)
```

Both sides must be **lists**:

```python
print([1, 2] + 3)        # ❌ TypeError
print([1, 2] + (3, 4))   # ❌ TypeError: can only concatenate list (not "tuple") to list
print([1, 2] + [3])      # ✅
```

### `extend()`: add many elements to the same list

```python
L1 = [1, 2, 3]
L1.extend([4, 5, 6, 7])
print(L1)            # [1, 2, 3, 4, 5, 6, 7]
```

| Feature              | `L1 + L2`              | `L1.extend(L2)`                      |
|----------------------|------------------------|--------------------------------------|
| Changes `L1`?        | ❌ no, makes a new list | ✅ yes, in place                     |
| Returns              | the new list           | `None`                               |
| Accepts              | lists only             | **any iterable** (tuple, string...)  |

```python
L = [1, 2]
L.extend((3, 4))        # ✅ [1, 2, 3, 4]
L += (5, 6)             # ✅ += behaves like extend: [1, 2, 3, 4, 5, 6]
```

`append()` vs `extend()`:

```python
L = [1, 2]
L.append([3, 4])        # [1, 2, [3, 4]]   one nested element
L = [1, 2]
L.extend([3, 4])        # [1, 2, 3, 4]     elements added individually
```

### Repetition with `*`

```python
L1 = [1, 2, 3]
L2 = L1 * 3
print(L2)             # [1, 2, 3, 1, 2, 3, 1, 2, 3]

print([0] * 5)        # [0, 0, 0, 0, 0]
print([1, 2] * 0)     # []
```

> ⚠️ **Trap with nested lists:** `*` copies the **reference**, not the inner list.

```python
m = [[0] * 3] * 3          # three names for the SAME inner list
m[0][0] = 1
print(m)                   # [[1, 0, 0], [1, 0, 0], [1, 0, 0]]   ← all rows changed!

m = [[0] * 3 for _ in range(3)]   # ✅ three separate lists
m[0][0] = 1
print(m)                   # [[1, 0, 0], [0, 0, 0], [0, 0, 0]]
```

### Membership with `in` and `not in`

Returns a **Boolean**.

```python
L1 = [1, 2, 3, 4, 5]
print(3 in L1)           # True
print(9 in L1)           # False
print(9 not in L1)       # True
```

With strings:

```python
colors = ['red', 'green', 'blue']
print('green' in colors)       # True
print('gre' in colors)         # False   (must match a whole element)
print('gre' in 'green')        # True    (but for a STRING it checks the substring)
```

> 📝 In your notes `'green '` has a trailing space. That element would not equal `'green'`, so `'green' in L1` would be `False`. Watch out for stray spaces.

#### ⚠️ `in` does NOT look inside nested lists

Your note said "it will check nested list also". That is not correct: `in` checks **only the top-level elements**.

```python
L = [1, [2, 3], 4]

print(1 in L)          # True
print(2 in L)          # False   ← 2 is inside the nested list
print([2, 3] in L)     # True    ← the nested list itself is an element
print(2 in L[1])       # True    ← look inside explicitly
```

To search inside nested lists you must loop:

```python
found = any(2 in x for x in L if isinstance(x, list))
```

**Performance:** `in` on a list checks elements one by one (**O(n)**). For many lookups use a **set**: `s = set(L)`; `x in s` is almost instant.

#### Using `in` in a loop

```python
L1 = ['red', 'green', 'blue']
for x in L1:
    print(x)
```

(Here `in` is part of the `for` statement, not the membership operator.)

### Comparison

Lists are compared **element by element, from left to right** (like strings in dictionary order). The **first different pair** decides.

```python
l1 = [1, 2, 3]
l2 = [1, 2, 3]
l3 = [2, 1, 3]

print(l1 == l2)      # True    same elements in the same order
print(l1 == l3)      # False   order matters
print(l1 != l3)      # True
```

> 📝 "Not equal" is **`!=`** (not `!==`).

```python
print([1, 2, 3] < [1, 2, 4])     # True     3 < 4 at the first difference
print([1, 2, 3] < [1, 3])        # True     2 < 3 at index 1
print([1, 2] < [1, 2, 0])        # True     a prefix is smaller
print([3] > [1, 2, 3])           # True     3 > 1 at index 0
print([] < [0])                  # True     empty list is the smallest
print([1, 2] == (1, 2))          # False    a list never equals a tuple
```

Comparing incompatible element types fails:

```python
print([1, 'a'] < [1, 2])         # ❌ TypeError: '<' not supported between 'str' and 'int'
```

`==` vs `is`:

```python
print(l1 == l2)      # True    same values
print(l1 is l2)      # False   two different list objects
```

### Other built-in functions for lists

```python
L = [4, 1, 7, 3]
print(len(L))        # 4
print(min(L))        # 1
print(max(L))        # 7
print(sum(L))        # 15
print(sorted(L))     # [1, 3, 4, 7]   (new list; L unchanged)
print(list(reversed(L)))   # [3, 7, 1, 4]
print(any([0, 0, 1]))      # True
print(all([1, 2, 0]))      # False
```

---

## 9. List Traversal

**Traversal** = visiting each element of the list. Three ways: `for` loop, `for` with `range()`, and `while` loop.

### 1. `for` loop (direct elements)

```python
l1 = [5, 6, 7, 8, 9]
for x in l1:
    print(x)
```

```
5
6
7
8
9
```

### 2. `for` loop with `range(len(...))` (needs the index)

```python
l1 = [5, 6, 7, 8, 9]
for i in range(len(l1)):
    print(i, l1[i])
```

```
0 5
1 6
2 7
3 8
4 9
```

> 📝 In your notes `l1(i)` uses round brackets, which tries to **call** the list (`TypeError: 'list' object is not callable`). Use square brackets: `l1[i]`.

### 3. `while` loop

```python
l1 = [1, 2, 4, 5, 6, 7]
i = 0
while i < len(l1):
    print(l1[i])
    i = i + 1
```

Output: `1 2 4 5 6 7` (one per line).

### Which one to use?

| Need                                        | Use                         |
|---------------------------------------------|-----------------------------|
| Just the values                             | `for x in L`                |
| Values **and** positions                    | `for i, x in enumerate(L)`  |
| Change elements by position                 | `for i in range(len(L))`    |
| Unknown steps / custom stopping rule        | `while`                     |

### Intermediate level: more traversal styles

```python
L = ['a', 'b', 'c']

# index and value together
for i, x in enumerate(L):
    print(i, x)                    # 0 a / 1 b / 2 c

for i, x in enumerate(L, start=1):
    print(i, x)                    # 1 a / 2 b / 3 c

# backwards
for x in reversed(L):
    print(x)                       # c b a

for i in range(len(L) - 1, -1, -1):
    print(L[i])                    # c b a

# every second element
for x in L[::2]:
    print(x)                       # a c

# two lists together
names = ['Asha', 'Ravi']
marks = [90, 85]
for n, m in zip(names, marks):
    print(n, m)
```

### Common traversal programs

```python
L = [7, 41, 6, 64, 93]

total = 0
for x in L:
    total += x
print(total)               # 211

print(total / len(L))      # 42.2   (average)

big = L[0]
for x in L:
    if x > big:
        big = x
print(big)                 # 93
```

### Changing elements while traversing

**Assigning to the loop variable does NOT change the list:**

```python
L = [1, 2, 3]
for x in L:
    x = x * 2
print(L)                   # [1, 2, 3]   unchanged
```

**Use the index to change it:**

```python
L = [1, 2, 3]
for i in range(len(L)):
    L[i] = L[i] * 2
print(L)                   # [2, 4, 6]
```

### ⚠️ Do not add or remove elements while looping over the same list

```python
L = [2, 4, 6, 7]
for x in L:
    if x % 2 == 0:
        L.remove(x)
print(L)                   # [4, 7]   ← 4 was skipped! (elements shift after each removal)
```

Safe alternatives:

```python
L = [2, 4, 6, 7]

for x in L[:]:             # loop over a COPY
    if x % 2 == 0:
        L.remove(x)
print(L)                   # [7]

L = [2, 4, 6, 7]
L = [x for x in L if x % 2 != 0]    # comprehension
print(L)                   # [7]
```

---

## 10. Preview: Methods, Comprehensions, Nested Lists

(Each gets its own detailed section next. This is a quick look.)

### Common list methods

| Method                  | What it does                           | Example → result                         |
|-------------------------|----------------------------------------|------------------------------------------|
| `append(x)`             | add `x` at the end                     | `[1,2]` → `[1,2,3]` with `append(3)`     |
| `extend(iterable)`      | add many at the end                    | `[1]` → `[1,2,3]` with `extend([2,3])`   |
| `insert(i, x)`          | insert `x` before index `i`            | `[1,3]` → `[1,2,3]` with `insert(1,2)`   |
| `remove(x)`             | delete the first `x` (error if absent) | `[1,2,1]` → `[2,1]` with `remove(1)`     |
| `pop(i)`                | remove and **return** item (default last) | `[1,2,3].pop()` → `3`                 |
| `clear()`               | remove everything                      | `[1,2]` → `[]`                           |
| `index(x)`              | position of first `x`                  | `[5,6,7].index(6)` → `1`                 |
| `count(x)`              | how many times `x` occurs              | `[1,2,1].count(1)` → `2`                 |
| `sort()`                | sort in place                          | `[3,1,2]` → `[1,2,3]`                    |
| `reverse()`             | reverse in place                       | `[1,2,3]` → `[3,2,1]`                    |
| `copy()`                | shallow copy                           | `L.copy()`                               |

### List comprehension

A compact way to build a list.

```python
squares = [x * x for x in range(1, 6)]
print(squares)                        # [1, 4, 9, 16, 25]

evens = [x for x in range(10) if x % 2 == 0]
print(evens)                          # [0, 2, 4, 6, 8]

words = ['hi', 'python', 'ok']
print([w.upper() for w in words if len(w) > 2])   # ['PYTHON']
```

### Nested lists (list of lists)

```python
matrix = [[1, 2, 3],
          [4, 5, 6]]

print(matrix[1])          # [4, 5, 6]
print(matrix[1][2])       # 6          (row 1, column 2)
print(len(matrix))        # 2 rows
print(len(matrix[0]))     # 3 columns

for row in matrix:
    for value in row:
        print(value, end=' ')
    print()
# 1 2 3
# 4 5 6

print([x for row in matrix for x in row])    # [1, 2, 3, 4, 5, 6]  (flatten)
```

---

## 11. Cheat Sheet

```text
CREATE        [1, 2, 3]    []    list(iterable)    list('abc')    list(range(5))
PROPERTIES    ordered, mutable, duplicates allowed, any types, dynamic size

INDEX         L[0] first    L[-1] last      IndexError if out of range
SLICE READ    L[start:stop:step]            stop excluded, never raises IndexError
              L[::-1] reversed    L[:] copy    L[::2] every 2nd

WRITE         L[i] = x                       replaces ONE element
              L[a:b] = iterable              any count (grow / shrink / insert / delete)
              L[a:b:step] = iterable         exact count needed (step is not 1)
              L[i:i] = [x]                   insert         L[a:b] = []   delete
              del L[i]    del L[a:b]    del L[::2]

OPERATORS     L1 + L2 (new list)    L * n    x in L    x not in L
              ==  !=  <  <=  >  >=  (element by element, left to right)

TRAVERSE      for x in L          for i in range(len(L))          while i < len(L)
              for i, x in enumerate(L)        for x in reversed(L)
```

---

## 12. Common Mistakes

| Mistake                                              | Fix                                                     |
|------------------------------------------------------|---------------------------------------------------------|
| `list((3,5,,79))` (double comma)                     | `list((3, 5, 79))`                                      |
| `5+6i` for a complex number                          | `5+6j`                                                  |
| `L1[0:0] = 10`                                       | slice assignment needs an iterable: `L1[0:0] = [10]`    |
| `L[3] = [10, 11]` expecting two elements             | that creates a nested list; use `L[3:4] = [10, 11]`     |
| Extended slice with the wrong count (`L[::-1] = [..5 items..]` on 7) | give exactly as many values as the slice selects |
| `L[10] = 5` on a short list                          | index assignment never extends a list; use `append`     |
| Thinking `in` searches nested lists                  | it checks only top-level elements                       |
| `l1 !== l2`                                          | `l1 != l2`                                              |
| `l1(i)` to read an element                           | `l1[i]`                                                 |
| `L = L.sort()` (or `append`, `reverse`, `extend`)    | these return `None`; just call `L.sort()`               |
| `b = a` and expecting a copy                         | `b = a.copy()` or `a[:]`                                |
| `[[0]*3]*3` for a matrix                             | `[[0]*3 for _ in range(3)]`                             |
| Removing items while looping over the same list      | loop over `L[:]` or use a comprehension                 |
| Changing the loop variable to update elements        | assign by index: `L[i] = ...`                           |
| `[1, 2] + (3, 4)`                                    | both operands must be lists (or use `extend`)           |
| Comparing mixed types: `[1,'a'] < [1,2]`             | keep element types comparable                           |
| Using `range(len(L)+1)`                              | last index is `len(L) - 1`                              |
| Using a list as a default argument `def f(x=[])`     | use `None` and create the list inside the function      |

---

## 13. Practice Questions

Predict the output first.

```python
L = [10, 20, 30, 40, 50]

# 1
print(L[1:4])

# 2
print(L[::-1])

# 3
print(L[-2:])

# 4
print(L[::2])

# 5
M = [10, 20, 30, 40, 50]
M[1:3] = [0]
print(M)

# 6
N = [1, 2, 3]
N[1:1] = [8, 9]
print(N)

# 7
print([1, 2] * 2 + [3])

# 8
print(2 in [1, [2, 3]])

# 9
print([1, 2, 3] < [1, 2, 4], [1, 2] < [1, 2, 0], [1, 2, 3] == [3, 2, 1])

# 10
P = [1, 2, 3, 4, 5, 6, 7]
P[::2] = [0, 0, 0, 0]
print(P)

# 11
a = [1, 2]
b = a
b.append(3)
print(a)

# 12
m = [[0] * 2] * 2
m[0][0] = 5
print(m)

# 13
Q = [1, 2, 3]
Q[5:5] = [9]
print(Q)

# 14
R = [1, 2, 3]
R[1] = [8, 9]
print(R)

# 15
print(list('abc'), list(range(3, 0, -1)))

# 16
S = [1, 2, 3, 4, 5, 6]
del S[::2]
print(S)
```

<details>
<summary>Answers</summary>

1. `[20, 30, 40]`
2. `[50, 40, 30, 20, 10]`
3. `[40, 50]`
4. `[10, 30, 50]`
5. `[10, 0, 40, 50]` (2 elements replaced by 1)
6. `[1, 8, 9, 2, 3]` (inserted before index 1)
7. `[1, 2, 1, 2, 3]`
8. `False` (2 is inside the nested list, not a top-level element)
9. `True True False`
10. `[0, 2, 0, 4, 0, 6, 0]` (indexes 0, 2, 4, 6 are four elements)
11. `[1, 2, 3]` (`a` and `b` are the same list)
12. `[[5, 0], [5, 0]]` (both rows are the same inner list)
13. `[1, 2, 3, 9]` (index 5 is beyond the end, so it is added at the end)
14. `[1, [8, 9], 3]`
15. `['a', 'b', 'c'] [3, 2, 1]`
16. `[2, 4, 6]` (indexes 0, 2, 4 were deleted)

</details>

### Mini challenges

1. Read `n` numbers into a list and print the sum, average, maximum and minimum.
2. Reverse a list **without** using `reverse()` or `[::-1]` (use a loop).
3. Find the **second largest** element.
4. Remove duplicates but keep the original order.
5. Rotate a list left by `k` positions using slicing.
6. Count the even and odd numbers in a list.
7. Merge two sorted lists into one sorted list.
8. Find the elements common to two lists.
9. Swap the first and last elements of a list.
10. Insert an element at index `i` using **only slice assignment**.
