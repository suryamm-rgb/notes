# Python List Methods
### Adding, Removing, Searching, Reversing and Sorting
### Beginner → Advanced Notes

---

## Table of Contents
1. [Overview of List Methods](#1-overview-of-list-methods)
2. [Adding Elements: append, extend, insert, copy](#2-adding-elements)
3. [Removing Elements: pop, remove, clear, del](#3-removing-elements)
4. [Index and Count](#4-index-and-count)
5. [reverse()](#5-reverse)
6. [sort()](#6-sort)
7. [Return Values and Speed](#7-return-values-and-speed)
8. [Mini Programs](#8-mini-programs)
9. [Cheat Sheet](#9-cheat-sheet)
10. [Common Mistakes](#10-common-mistakes)
11. [Practice Questions](#11-practice-questions)

---

## 1. Overview of List Methods

A **method** is a function that belongs to an object and is called with a dot: `list_name.method(arguments)`.

Because lists are **mutable**, most list methods **change the list in place**.

`list` has **11 public methods**:

| Group                | Methods                                  |
|----------------------|------------------------------------------|
| **Adding**           | `append`, `extend`, `insert`, `copy`     |
| **Removing**         | `pop`, `remove`, `clear` (and the `del` statement) |
| **Searching**        | `index`, `count`                         |
| **Reordering**       | `reverse`, `sort`                        |

See them all:

```python
print([m for m in dir(list) if not m.startswith('__')])
# ['append', 'clear', 'copy', 'count', 'extend', 'index',
#  'insert', 'pop', 'remove', 'reverse', 'sort']

help(list.insert)       # description of one method
```

> ⚠️ **Golden rule:** methods that change the list in place (`append`, `extend`, `insert`, `remove`, `clear`, `reverse`, `sort`) **return `None`**. Never write `L = L.sort()`.

---

## 2. Adding Elements

### 2.1 `append(element)`: add ONE element at the end

```python
l1 = [1, 2, 3, 4, 5]
l1.append(6)
print(l1)            # [1, 2, 3, 4, 5, 6]
```

`append()` takes **only one element at a time**.

```python
l1.append(7, 8)      # ❌ TypeError: list.append() takes exactly one argument (2 given)
```

If that one element is a **list**, it is added **as a single nested element**:

```python
l1 = [1, 2, 3]
l1.append([4, 5])
print(l1)            # [1, 2, 3, [4, 5]]
print(len(l1))       # 4
```

Adding in a loop:

```python
squares = []
for i in range(1, 6):
    squares.append(i * i)
print(squares)       # [1, 4, 9, 16, 25]
```

### 2.2 `extend(iterable)`: add MANY elements at the end

Adds **each item of an iterable** individually.

```python
l1 = [1, 2, 3]
l1.extend([5, 6, 7])
print(l1)            # [1, 2, 3, 5, 6, 7]
```

```python
l1 = [1, 2, 3, 4]
l1.extend(range(5, 8))
print(l1)            # [1, 2, 3, 4, 5, 6, 7]
```

Any iterable works (list, tuple, string, range, set, dict keys...):

```python
L = [1]
L.extend((2, 3))         # tuple   -> [1, 2, 3]
L.extend('ab')           # string  -> [1, 2, 3, 'a', 'b']
```

The argument **must be iterable**:

```python
L.extend(5)              # ❌ TypeError: 'int' object is not iterable
```

#### `append` vs `extend`

```python
L = [1, 2]
L.append([3, 4])         # [1, 2, [3, 4]]      one nested element
L = [1, 2]
L.extend([3, 4])         # [1, 2, 3, 4]        two separate elements
```

| Feature        | `append(x)`                  | `extend(iterable)`                 |
|----------------|------------------------------|------------------------------------|
| Adds           | **one** element (`x` itself) | **each item** of the iterable      |
| Argument       | any object                   | must be iterable                   |
| `[1].…([2,3])` gives | `[1, [2, 3]]`          | `[1, 2, 3]`                        |

#### Ways to join lists

| Code                  | Changes the original? | Notes                                   |
|-----------------------|-----------------------|-----------------------------------------|
| `a.extend(b)`         | ✅ yes                | in place, accepts any iterable          |
| `a += b`              | ✅ yes                | same as extend                          |
| `c = a + b`           | ❌ no                 | creates a new list; lists only          |

### 2.3 `insert(index, element)`: add at a position

Inserts the element **before** the given index; the later elements shift right.

```python
l1 = [1, 2, 3, 4]
l1.insert(0, 50)
print(l1)            # [50, 1, 2, 3, 4]        at the front

l1 = [1, 2, 3, 4]
l1.insert(2, 'x')
print(l1)            # [1, 2, 'x', 3, 4]       before index 2
```

**Index beyond the end:** no error. The element is simply added at the end.

```python
l1 = [1, 2, 3, 4]
l1.insert(70, 'python')
print(l1)            # [1, 2, 3, 4, 'python']
```

**Negative index:** counts from the end; the element goes **before** that position.

```python
l1 = [1, 2, 3, 4]
l1.insert(-1, 9)
print(l1)            # [1, 2, 3, 9, 4]
```

Inserting a list adds it as **one** element:

```python
l1 = [1, 2, 3]
l1.insert(1, [8, 9])
print(l1)            # [1, [8, 9], 2, 3]
```

| Call                    | Same as                      |
|-------------------------|------------------------------|
| `L.insert(0, x)`        | `L[:0] = [x]`                |
| `L.insert(len(L), x)`   | `L.append(x)`                |
| `L.insert(i, x)`        | `L[i:i] = [x]`               |

> Insertion at the front or middle is **slow for big lists** (every later element must shift): O(n).

### 2.4 `copy()`: make a copy of the list

```python
l1 = [1, 2, 3, 4]
l2 = l1.copy()
l2.append(99)
print(l1)            # [1, 2, 3, 4]       original is safe
print(l2)            # [1, 2, 3, 4, 99]
```

Compare with simple assignment (no copy at all):

```python
a = [1, 2, 3]
b = a                # same list, two names
b.append(4)
print(a)             # [1, 2, 3, 4]   a changed too!

c = a.copy()         # real, independent list
```

Other ways to copy: `a[:]`, `list(a)`, `copy.copy(a)`.

#### Intermediate / Advanced: `copy()` is a SHALLOW copy

The new list is separate, but **inner objects are shared**.

```python
a = [[1, 2], [3, 4]]
b = a.copy()
b[0].append(99)
print(a)             # [[1, 2, 99], [3, 4]]   inner list shared!

b.append([5, 6])
print(a)             # [[1, 2, 99], [3, 4]]   the outer list is independent

import copy
c = copy.deepcopy(a) # copies everything, including inner lists
c[0].append(100)
print(a)             # unchanged by c
```

---

## 3. Removing Elements

| Tool            | Removes by...        | Returns the removed item? | If it fails           |
|-----------------|----------------------|---------------------------|-----------------------|
| `pop(index)`    | **position**         | ✅ yes                    | `IndexError`          |
| `remove(x)`     | **value** (first match) | ❌ `None`              | `ValueError`          |
| `clear()`       | everything           | ❌ `None`                 | never fails           |
| `del L[i]`      | position, slice, or the whole variable | ❌ (a statement) | `IndexError` / `NameError` |

### 3.1 `pop(index)`: remove by position and get it back

Default index is **`-1`** (the last element).

```python
L1 = [1, 2, 3, 4, 5, 6]

x = L1.pop()
print(x)             # 6          the removed element is returned
print(L1)            # [1, 2, 3, 4, 5]

print(L1.pop(0))     # 1          remove the first element
print(L1)            # [2, 3, 4, 5]

print(L1.pop(-2))    # 4          second from the end
print(L1)            # [2, 3, 5]
```

**Errors:**

```python
L1 = []
L1.pop()             # ❌ IndexError: pop from empty list

[1, 2].pop(5)        # ❌ IndexError: pop index out of range
```

Check first when the list may be empty:

```python
if L1:
    L1.pop()
```

#### Advanced: stack and queue

```python
stack = []
stack.append(1); stack.append(2); stack.append(3)
print(stack.pop(), stack.pop(), stack.pop())     # 3 2 1   (last in, first out)
```

`pop()` at the end is **fast** (O(1)). `pop(0)` at the front is **slow** (O(n)). For a real queue use `collections.deque` and `popleft()`.

### 3.2 `remove(element)`: remove by value

Removes the **first** occurrence of the value.

```python
L1 = [1, 2, 3, 4, 5, 6, 3]
L1.remove(3)
print(L1)            # [1, 2, 4, 5, 6, 3]    only the first 3 is gone
```

If the value is missing:

```python
L1.remove(99)        # ❌ ValueError: list.remove(x): x not in list
```

Safe way:

```python
if 99 in L1:
    L1.remove(99)
```

**Remove ALL occurrences:**

```python
L = [1, 2, 3, 2, 4, 2]

# 1. while loop
while 2 in L:
    L.remove(2)
print(L)             # [1, 3, 4]

# 2. comprehension (creates a new list)
L = [1, 2, 3, 2, 4, 2]
L = [x for x in L if x != 2]
print(L)             # [1, 3, 4]
```

### 3.3 `clear()`: remove all elements

```python
L1 = [1, 2, 3]
L1.clear()
print(L1)            # []
print(len(L1))       # 0
```

Equivalent: `del L1[:]` or `L1[:] = []`.

#### `clear()` vs `L1 = []`

```python
a = [1, 2]
b = a                # two names, one list

a = []               # a now points to a NEW empty list
print(b)             # [1, 2]   b still has the old list

a = b
a.clear()            # empties the list itself
print(b)             # []       b sees the change
```

### 3.4 `del` statement

`del` is a **statement** (not a method), so no dot and no brackets after it.

```python
L1 = [1, 2, 3, 4]
del L1[2]
print(L1)            # [1, 2, 4]
```

**Slices:**

```python
L = [0, 1, 2, 3, 4, 5]
del L[1:3]
print(L)             # [0, 3, 4, 5]

L = [0, 1, 2, 3, 4, 5]
del L[::2]
print(L)             # [1, 3, 5]

L = [0, 1, 2, 3, 4, 5]
del L[-1]
print(L)             # [0, 1, 2, 3, 4]
```

**Delete the whole variable:**

```python
L = [1, 2, 3]
del L                # the name L no longer exists
print(L)             # ❌ NameError: name 'L' is not defined
```

### Which removal tool?

| I want to...                                   | Use                    |
|------------------------------------------------|------------------------|
| remove the last item **and use it**            | `pop()`                |
| remove by position and use the item            | `pop(i)`               |
| remove by position, no need for the item       | `del L[i]`             |
| remove by **value**                            | `remove(x)`            |
| remove a **range** of positions                | `del L[a:b]`           |
| empty the list                                 | `clear()`              |
| remove all items matching a condition          | list comprehension     |

---

## 4. Index and Count

### 4.1 `index(element, start, end)`: position of an element

Returns the index of the **first** occurrence. `start` and `end` are **optional** and limit the search to the slice `[start:end]` (the `end` is excluded). The returned index is still counted from the **beginning of the list**.

```python
L1 = [10, 20, 30, 20, 40]

print(L1.index(20))          # 1
print(L1.index(20, 2))       # 3      search from index 2 onwards
print(L1.index(40, 0, 5))    # 4
```

Not found → **`ValueError`**:

```python
L1.index(50)                 # ❌ ValueError: 50 is not in list
L1.index(20, 2, 3)           # ❌ ValueError (only index 2, which holds 30, is searched)
```

Safe way:

```python
if 50 in L1:
    print(L1.index(50))
else:
    print("not found")
```

> Unlike `str.find()`, **lists have no `find()`**. `index()` raises an error instead of returning `-1`.

**Find ALL positions (Advanced):**

```python
L = [10, 20, 10, 30, 10]
positions = []
start = 0
while True:
    try:
        i = L.index(10, start)
    except ValueError:
        break
    positions.append(i)
    start = i + 1
print(positions)             # [0, 2, 4]

# shorter:
print([i for i, x in enumerate(L) if x == 10])     # [0, 2, 4]
```

### 4.2 `count(element)`: how many times it appears

```python
L1 = [10, 30, 20, 30, 40, 30]
print(L1.count(30))      # 3
print(L1.count(20))      # 1
print(L1.count(99))      # 0      (no error when missing)
```

Counts only **top-level** elements that are **equal** (`==`) to the value:

```python
print([[1], [1], 2].count([1]))      # 2
print([[1], [1], 2].count(1))        # 0      (1 is not equal to [1])
print([1, True, 1.0].count(1))       # 3      (1 == True == 1.0)
```

Count of every distinct value (Advanced):

```python
from collections import Counter
print(Counter([1, 2, 1, 3, 1]))      # Counter({1: 3, 2: 1, 3: 1})
```

---

## 5. reverse()

Reverses the list **in place** (the order of the elements is flipped). Returns `None`.

```python
l1 = [1, 2, 3, 4, 5]
l1.reverse()
print(l1)            # [5, 4, 3, 2, 1]
```

> 📝 In your notes `reversea()` is a typo for `reverse()`.

### Three ways to reverse

| Way                    | Changes the original? | Returns                    |
|------------------------|-----------------------|----------------------------|
| `L.reverse()`          | ✅ yes                | `None`                     |
| `L[::-1]`              | ❌ no                 | a **new reversed list**    |
| `reversed(L)`          | ❌ no                 | an **iterator** (use `list(...)`) |

```python
L = [1, 2, 3]
print(L[::-1])               # [3, 2, 1]
print(list(reversed(L)))     # [3, 2, 1]
print(reversed(L))           # <list_reverseiterator object at 0x...>
for x in reversed(L):
    print(x)                 # 3 2 1
print(L)                     # [1, 2, 3]  (unchanged)
```

> `reverse()` **reverses the order**; it does **not sort** in descending order. `[3, 1, 2].reverse()` gives `[2, 1, 3]`.

---

## 6. sort()

### Syntax

```python
list.sort(*, key=None, reverse=False)
```

| Parameter  | Meaning                                              | Default |
|------------|------------------------------------------------------|---------|
| `key`      | a function applied to **each element**; elements are ordered by the function's result | `None` (compare the elements themselves) |
| `reverse`  | `True` → **descending** order                        | `False` |

- Sorts the list **in place** and returns **`None`**.
- The `*` means `key` and `reverse` are **keyword-only**: you must write the names.

```python
L = [3, 1, 2]
L.sort(True)          # ❌ TypeError: sort() takes no positional arguments
L.sort(reverse=True)  # ✅
```

> 📝 In your notes `reverse=false` should be `reverse=False` (capital F).

### Basic sorting

```python
l1 = [70, 10, 60, 20, 50, 30, 40]

l1.sort()
print(l1)            # [10, 20, 30, 40, 50, 60, 70]      ascending

l1.sort(reverse=True)
print(l1)            # [70, 60, 50, 40, 30, 20, 10]      descending
```

Strings are sorted in **dictionary order by Unicode value** (uppercase letters come before lowercase):

```python
words = ['banana', 'Apple', 'cherry', 'apple']
words.sort()
print(words)         # ['Apple', 'apple', 'banana', 'cherry']
```

### `key`: sort by a rule

The `key` function receives **one element** and returns the value to compare. The list keeps the original elements; only the **order** is decided by the keys.

**By length: `key=len`**

```python
l1 = ['fig', 'apple', 'banana', 'kiwi']
l1.sort(key=len)
print(l1)            # ['fig', 'kiwi', 'apple', 'banana']      lengths 3, 4, 5, 6
```

> ⚠️ `key=len` works only when every element **has a length** (strings, lists, tuples). `[3, 1, 2].sort(key=len)` raises `TypeError: object of type 'int' has no len()`.

**Case-insensitive: `key=str.lower`**

```python
l1 = ['Bat', 'Dog', 'apple', 'cat']

l1.sort()
print(l1)            # ['Bat', 'Dog', 'apple', 'cat']       uppercase first

l1.sort(key=str.lower)
print(l1)            # ['apple', 'Bat', 'cat', 'Dog']       like a dictionary
```

> 📝 In your notes the last string `'cat]` is missing its closing quote: `'cat'`.

**By absolute value: `key=abs`**

```python
L = [-5, 2, -1, 4]
L.sort(key=abs)
print(L)             # [-1, 2, 4, -5]
```

**Numbers stored as strings: `key=int`**

```python
L = ['10', '9', '2']
L.sort()
print(L)             # ['10', '2', '9']       text order (wrong for numbers)
L.sort(key=int)
print(L)             # ['2', '9', '10']
```

**`key` with `reverse`:**

```python
words = ['fig', 'apple', 'kiwi']
words.sort(key=len, reverse=True)
print(words)         # ['apple', 'kiwi', 'fig']
```

### Intermediate: `lambda` keys

A **lambda** is a small one-line function.

```python
students = [('Asha', 90), ('Ravi', 85), ('Meena', 92)]

students.sort(key=lambda s: s[1])                  # by marks, low to high
print(students)      # [('Ravi', 85), ('Asha', 90), ('Meena', 92)]

students.sort(key=lambda s: s[1], reverse=True)    # high to low
print(students)      # [('Meena', 92), ('Asha', 90), ('Ravi', 85)]
```

**Several sort rules at once** (return a tuple):

```python
words = ['pear', 'fig', 'apple', 'kiwi']
words.sort(key=lambda w: (len(w), w))      # by length, then alphabetically
print(words)         # ['fig', 'kiwi', 'pear', 'apple']
```

### Advanced: sort is STABLE

Elements with **equal keys keep their original relative order**.

```python
L = ['bb', 'a', 'cc', 'd']
L.sort(key=len)
print(L)             # ['a', 'd', 'bb', 'cc']      'a' before 'd', 'bb' before 'cc'
```

This makes **multi-step sorting** possible: sort by the secondary rule first, then by the primary rule.

```python
L = ['b', 'A', 'C', 'a']
L.sort(key=str.lower)
print(L)             # ['A', 'a', 'b', 'C']      'A' stays before 'a' (it came first)
```

### Sorting nested lists

Lists are compared element by element, so sorting a list of lists works naturally:

```python
L = [[2, 'b'], [1, 'z'], [2, 'a']]
L.sort()
print(L)             # [[1, 'z'], [2, 'a'], [2, 'b']]
```

### Mixed types cannot be sorted

```python
[3, 'a', 1].sort()   # ❌ TypeError: '<' not supported between instances of 'str' and 'int'
```

### `sort()` vs `sorted()`

| Feature          | `L.sort()`                | `sorted(L)`                      |
|------------------|---------------------------|----------------------------------|
| Changes `L`?     | ✅ yes, in place          | ❌ no                            |
| Returns          | `None`                    | a **new sorted list**            |
| Works on         | **lists only**            | **any iterable** (tuple, string, set, dict) |
| Parameters       | `key`, `reverse`          | `key`, `reverse`                 |

```python
L = [3, 1, 2]
print(sorted(L))            # [1, 2, 3]
print(L)                    # [3, 1, 2]   unchanged
print(sorted('hello'))      # ['e', 'h', 'l', 'l', 'o']
print(sorted((3, 1, 2)))    # [1, 2, 3]
x = L.sort()
print(x)                    # None
```

Python's sort is **Timsort**: O(n log n), very fast on partly sorted data.

---

## 7. Return Values and Speed

### What each method returns

| Method          | Returns                           |
|-----------------|-----------------------------------|
| `append(x)`     | `None`                            |
| `extend(it)`    | `None`                            |
| `insert(i, x)`  | `None`                            |
| `remove(x)`     | `None`                            |
| `clear()`       | `None`                            |
| `reverse()`     | `None`                            |
| `sort()`        | `None`                            |
| `pop(i)`        | **the removed element**           |
| `index(x)`      | an **int** (position)             |
| `count(x)`      | an **int**                        |
| `copy()`        | a **new list**                    |

### Speed (n = length of the list)

| Method                  | Time                          |
|-------------------------|-------------------------------|
| `append(x)`             | O(1) on average               |
| `pop()` (last)          | O(1)                          |
| `extend(k items)`       | O(k)                          |
| `insert(i, x)`          | O(n)                          |
| `pop(i)`, `del L[i]`    | O(n) (except at the end)      |
| `remove(x)`             | O(n)                          |
| `index(x)`, `count(x)`, `x in L` | O(n)                 |
| `reverse()`, `copy()`, `clear()` | O(n)                 |
| `sort()`                | O(n log n)                    |

---

## 8. Mini Programs

**1. Stack (last in, first out)**

```python
stack = []
for ch in 'abc':
    stack.append(ch)
while stack:
    print(stack.pop(), end=' ')      # c b a
print()
```

**2. Remove duplicates, keeping the order**

```python
L = [3, 1, 3, 2, 1]
result = []
for x in L:
    if x not in result:
        result.append(x)
print(result)        # [3, 1, 2]
```

**3. Top 3 scores**

```python
scores = [55, 92, 78, 92, 60]
print(sorted(scores, reverse=True)[:3])      # [92, 92, 78]
```

**4. Sort students by marks (highest first)**

```python
students = [('Asha', 90), ('Ravi', 85), ('Meena', 92)]
students.sort(key=lambda s: s[1], reverse=True)
for name, marks in students:
    print(name, marks)
```

**5. Rotate right by one**

```python
L = [1, 2, 3, 4]
L.insert(0, L.pop())
print(L)             # [4, 1, 2, 3]
```

**6. Median**

```python
data = [7, 1, 5, 3, 9]
data.sort()
print(data[len(data) // 2])      # 5
```

**7. Second largest**

```python
L = [7, 41, 6, 64, 93, 64]
unique = sorted(set(L))
print(unique[-2])                # 64
```

**8. Insert into a sorted list**

```python
L = [10, 20, 40, 50]
x = 30
i = 0
while i < len(L) and L[i] < x:
    i += 1
L.insert(i, x)
print(L)             # [10, 20, 30, 40, 50]
```

**9. Remove all negative numbers**

```python
L = [3, -1, 4, -5, 9]
L = [x for x in L if x >= 0]
print(L)             # [3, 4, 9]
```

**10. Sort words by length, then alphabetically**

```python
words = ['pear', 'fig', 'apple', 'kiwi']
words.sort(key=lambda w: (len(w), w))
print(words)         # ['fig', 'kiwi', 'pear', 'apple']
```

---

## 9. Cheat Sheet

```text
ADD        L.append(x)            one element at the end
           L.extend(iterable)     each item at the end
           L.insert(i, x)         before index i (beyond the end -> appended)
           L.copy()               shallow copy

REMOVE     L.pop()                remove + return last        IndexError if empty
           L.pop(i)               remove + return index i     IndexError if bad index
           L.remove(x)            first match by value        ValueError if missing
           L.clear()              remove everything
           del L[i]   del L[a:b]  del L[::2]   del L (deletes the name)

SEARCH     L.index(x, start, end) position of first match     ValueError if missing
           L.count(x)             number of matches (0 if none)

ORDER      L.reverse()            reverse in place
           L.sort(key=None, reverse=False)    sort in place (keyword-only arguments)
           sorted(L)  reversed(L) L[::-1]     versions that do not change L

REMEMBER   all in-place methods return None (only pop, index, count, copy return something)
```

---

## 10. Common Mistakes

| Mistake                                              | Fix                                                    |
|------------------------------------------------------|--------------------------------------------------------|
| `L = L.append(x)` / `L = L.sort()`                   | just call `L.append(x)`; the result is `None`          |
| `print(L.sort())` expecting a sorted list            | use `print(sorted(L))`                                 |
| `append(1, 2)` or `append([1,2])` to add two items   | `extend([1, 2])`                                       |
| `extend(5)`                                          | argument must be iterable: `extend([5])`               |
| `L.pop()` on an empty list                           | check `if L:` first                                    |
| `L.remove(x)` when `x` may be missing                | check `if x in L:`                                     |
| Expecting `remove` to delete all matches             | it removes only the first; use a loop or comprehension |
| `L.index(x)` for a missing item                      | it raises `ValueError`; check with `in`                |
| Looking for `L.find(x)`                              | lists have no `find`; use `index` (or `in`)            |
| `reversea()`, `reverse=false`, `'cat]`               | `reverse()`, `reverse=False`, `'cat'`                  |
| `sort(True)`                                         | `sort(reverse=True)` (keyword-only)                    |
| `sort(key=len)` on numbers                           | use `key=str` or another suitable function             |
| Sorting a list with mixed types                      | convert to one type first                              |
| `b = a` and expecting a copy                         | `b = a.copy()`                                         |
| Using `copy()` on nested lists and editing inner lists | `copy.deepcopy()`                                    |
| `del L[i]` while looping over `L`                    | loop over a copy or use a comprehension                |
| `L1 = []` hoping to clear other references           | `L1.clear()`                                           |
| Thinking `reverse()` sorts in descending order       | it only flips the current order                        |
| Using `del L` then using `L`                         | `NameError`; use `clear()` to empty the list           |
| `insert(70, x)` expecting an error                   | no error; the element goes to the end                  |

---

## 11. Practice Questions

Predict the output first.

```python
# 1
L = [1, 2, 3]
L.append([4, 5])
print(L)

# 2
L = [1, 2, 3]
L.extend([4, 5])
print(L)

# 3
L = [1, 2, 3, 4]
L.insert(1, 'x')
print(L)

# 4
L = [1, 2, 3]
L.insert(50, 9)
print(L)

# 5
L = [10, 20, 30]
print(L.pop(), L)

# 6
L = [1, 2, 3, 2]
L.remove(2)
print(L)

# 7
L = [5, 6, 7]
print(L.index(7))

# 8
L = [1, 2, 1, 1]
print(L.count(1))

# 9
L = [3, 1, 2]
L.sort(reverse=True)
print(L)

# 10
L = ['b', 'A', 'C', 'a']
L.sort()
print(L)
L.sort(key=str.lower)
print(L)

# 11
x = [3, 1, 2].sort()
print(x)

# 12
a = [1, 2]
b = a
c = a.copy()
a.append(3)
print(b, c)

# 13
L = [1, 2, 3]
L.clear()
print(L, len(L))

# 14
L = [0, 1, 2, 3, 4, 5]
del L[1::2]
print(L)

# 15
L = [10, 20, 30, 20]
print(L.index(20, 2))

# 16
words = ['fig', 'apple', 'kiwi']
words.sort(key=len)
print(words)

# 17
L = [1, 2, 3, 4, 5]
L.reverse()
print(L)

# 18
print([].pop())
```

<details>
<summary>Answers</summary>

1. `[1, 2, 3, [4, 5]]`
2. `[1, 2, 3, 4, 5]`
3. `[1, 'x', 2, 3, 4]`
4. `[1, 2, 3, 9]` (index beyond the end → added at the end)
5. `30 [10, 20]`
6. `[1, 3, 2]` (only the first 2 is removed)
7. `2`
8. `3`
9. `[3, 2, 1]`
10. `['A', 'C', 'a', 'b']` then `['A', 'a', 'b', 'C']` (stable sort keeps `'A'` before `'a'`)
11. `None`
12. `[1, 2, 3] [1, 2]` (`b` is the same list as `a`; `c` is an independent copy)
13. `[] 0`
14. `[0, 2, 4]`
15. `3`
16. `['fig', 'kiwi', 'apple']`
17. `[5, 4, 3, 2, 1]`
18. `IndexError: pop from empty list`

</details>

### Mini challenges

1. Read numbers until the user types `done`, store them with `append`, then print the sorted list and the largest value.
2. Remove all occurrences of a given value from a list **in place**.
3. Print each distinct element of a list with its count (use `count()`).
4. Reverse a list without `reverse()`, `reversed()` or slicing.
5. Sort a list of names by their **last letter**.
6. Sort a list of words by length (longest first), then alphabetically.
7. Merge two lists and sort the result without duplicates.
8. Implement a simple **to-do list** menu with add, remove, show and clear options.
9. Implement a **stack** and a **queue** using list methods and compare the speed of `pop()` vs `pop(0)`.
10. Given a list of `(name, marks)` tuples, print the names ranked by marks.
