# Python Tuple Data Type
### Beginner → Advanced Notes

**Topics:** Introduction · Creation · Indexing and slicing · Packing and unpacking · Operators · "Comprehensions" · Methods

---

## Table of Contents
1. [What is a Tuple?](#1-what-is-a-tuple)
2. [Creating a Tuple](#2-creating-a-tuple)
3. [Representation and Immutability](#3-representation-and-immutability)
4. [Indexing and Slicing (Read Only)](#4-indexing-and-slicing-read-only)
5. [Packing and Unpacking](#5-packing-and-unpacking)
6. [Operators on Tuples](#6-operators-on-tuples)
7. [Tuple Traversal](#7-tuple-traversal)
8. [Tuple "Comprehension"](#8-tuple-comprehension)
9. [Tuple Methods and Built-in Functions](#9-tuple-methods-and-built-in-functions)
10. [Tuple vs List](#10-tuple-vs-list)
11. [Advanced Topics](#11-advanced-topics)
12. [Mini Programs](#12-mini-programs)
13. [Cheat Sheet](#13-cheat-sheet)
14. [Common Mistakes](#14-common-mistakes)
15. [Practice Questions](#15-practice-questions)

---

## 1. What is a Tuple?

### Beginner level

A **tuple** is a **collection of elements** written inside **round brackets** `( )`, separated by commas. It is the **immutable** (unchangeable) cousin of the list.

```python
t1 = (1, 2, 3, 4, 5)
t2 = (1.8, 1.8, 3.5, 6, )               # trailing comma is allowed
t3 = (7, 3.2, 'john', True, 5 + 6j)     # mixed types
```

> 📝 In your notes the complex number was written `5+6i`. Python uses **`j`**: `5+6j`.

### Key properties

| Property            | Meaning                                                         |
|---------------------|-----------------------------------------------------------------|
| **Ordered**         | elements keep their position; each has an index                 |
| **Immutable**       | once created, you **cannot change, add or remove** elements     |
| **Allows duplicates** | the same value can appear many times: `(1, 1, 2)`             |
| **Heterogeneous**   | elements can be of different types                              |
| **Indexable / sliceable** | `t[0]`, `t[1:3]`                                          |
| **Hashable** (if all elements are hashable) | can be a dictionary key or set member   |

### Tuple is "the same as a list, but it cannot change"

| Feature      | List          | Tuple          |
|--------------|---------------|----------------|
| Brackets     | `[1, 2, 3]`   | `(1, 2, 3)`    |
| Ordered      | ✅            | ✅             |
| Duplicates   | ✅            | ✅             |
| Mixed types  | ✅            | ✅             |
| **Mutable**  | ✅ yes        | ❌ **no**      |

### Why use a tuple?

- **Safety:** data that must not change by accident (days of the week, RGB colours, coordinates).
- **Dictionary keys and set members:** only immutable (hashable) objects are allowed.
- **Slightly faster and smaller** than a list.
- **Returning several values** from a function.
- It tells the reader: "this is a fixed record".

---

## 2. Creating a Tuple

### Method 1: round brackets

```python
t1 = (1, 2, 3, 4, 5)
t3 = ()                      # empty tuple
```

### Method 2: `tuple(iterable)`

Converts **any iterable** into a tuple.

```python
t2 = tuple([1, 2, 3, 4, 5, 6])
print(t2)                    # (1, 2, 3, 4, 5, 6)

t3 = tuple('python')
print(t3)                    # ('p', 'y', 't', 'h', 'o', 'n')

t4 = tuple(range(1, 5))
print(t4)                    # (1, 2, 3, 4)

print(tuple())               # ()
print(tuple({'a': 1, 'b': 2}))   # ('a', 'b')    keys of a dictionary
```

`tuple()` takes **one** argument only:

```python
tuple(1, 2, 3, 4, 5)         # ❌ TypeError: tuple expected at most 1 argument, got 5
tuple((1, 2, 3, 4, 5))       # ✅ pass ONE iterable (a tuple here)
tuple([1, 2, 3, 4, 5])       # ✅ or a list
```

> 📝 In your notes `tup;e('python')` is a typo for `tuple('python')`.

### Method 3: without brackets (tuple packing)

The **comma** makes a tuple, not the brackets.

```python
t5 = 3, 6, 9
print(t5, type(t5))          # (3, 6, 9) <class 'tuple'>

t6 = 1, 2, 3, 4, 5, 6, 7
print(t6)                    # (1, 2, 3, 4, 5, 6, 7)
```

### ⚠️ The single-element tuple

```python
t4 = (3)
print(type(t4))              # <class 'int'>     ← just the number 3 in brackets!

t4 = (3,)
print(type(t4))              # <class 'tuple'>   ← the comma makes it a tuple

t7 = 3,                      # also a tuple
print(type(t7))              # <class 'tuple'>
```

| Expression      | Type      | Why                                   |
|-----------------|-----------|---------------------------------------|
| `(3)`           | `int`     | brackets only group an expression     |
| `(3,)`          | `tuple`   | the **comma** creates the tuple       |
| `('a')`         | `str`     |                                       |
| `('a',)`        | `tuple`   |                                       |
| `()`            | `tuple`   | the only tuple made without a comma   |
| `(1 + 2)`       | `int`     |                                       |

### Checking the type

```python
t1 = (1, 2, 3)
print(type(t1))                    # <class 'tuple'>
print(isinstance(t1, tuple))       # True
print(len(t1))                     # 3
```

---

## 3. Representation and Immutability

### Representation

```python
t1 = (6, 5, 4, 2, 3, 2)
print(t1)           # (6, 5, 4, 2, 3, 2)
print(len(t1))      # 6
print(t1[2])        # 4
```

A single-element tuple prints with a comma: `(3,)`. An empty tuple prints `()`.

### Reading is allowed

```python
t1 = (6, 5, 4, 2, 3, 2)
print(t1[2])        # 4
print(t1[-1])       # 2

for x in t1:
    print(x)        # 6 5 4 2 3 2 (one per line)
```

### Changing is NOT allowed

```python
t1[2] = 10
```

```
TypeError: 'tuple' object does not support item assignment
```

```python
t1.append(7)        # ❌ AttributeError: 'tuple' object has no attribute 'append'
del t1[0]           # ❌ TypeError: 'tuple' object doesn't support item deletion
```

A tuple has **no** `append`, `extend`, `insert`, `remove`, `pop`, `clear`, `sort` or `reverse`.

### How do I "change" a tuple?

You cannot change it, but you can **create a new tuple** and assign it back.

```python
t = (1, 2, 3)

t = t + (4,)                 # add at the end        -> (1, 2, 3, 4)
t = t[:1] + (9,) + t[2:]     # replace index 1       -> (1, 9, 3, 4)
t = t[:-1]                   # remove the last       -> (1, 9, 3)
```

Or go through a **list**:

```python
t = (1, 2, 3)
lst = list(t)               # tuple -> list
lst[1] = 99                 # change the list
t = tuple(lst)              # list -> tuple
print(t)                    # (1, 99, 3)
```

> The name `t` now points to a **new** tuple. The old tuple object was never modified.

---

## 4. Indexing and Slicing (Read Only)

For tuples only **read** operations exist (no writing).

### Index map

```python
t1 = 3, 6, 9, 12, 15, 18, 21
```

```
value :    3    6    9   12   15   18   21
index :    0    1    2    3    4    5    6        <- positive
neg   :   -7   -6   -5   -4   -3   -2   -1        <- negative
```

### Indexing

```python
print(t1[4])        # 15
print(t1[-3])       # 15     (same element as index 4)
print(t1[0], t1[-1])# 3 21   first and last
print(t1[7])        # ❌ IndexError: tuple index out of range
```

### Slicing: `t[start:stop:step]`

`start` is **included**, `stop` is **not included**. A slice always gives a **new tuple**.

```python
print(t1[:])         # (3, 6, 9, 12, 15, 18, 21)    whole tuple
print(t1[2:])        # (9, 12, 15, 18, 21)          from index 2 to the end
print(t1[2:5])       # (9, 12, 15)                  indexes 2, 3, 4
print(t1[-5:-2])     # (9, 12, 15)                  indexes 2, 3, 4
print(t1[:3])        # (3, 6, 9)                    first three
print(t1[-2:])       # (18, 21)                     last two

print(t1[::])        # (3, 6, 9, 12, 15, 18, 21)
print(t1[::1])       # (3, 6, 9, 12, 15, 18, 21)
print(t1[::2])       # (3, 9, 15, 21)              every 2nd element
print(t1[::-1])      # (21, 18, 15, 12, 9, 6, 3)   reversed
print(t1[4::-1])     # (15, 12, 9, 6, 3)           from index 4 backwards to the start
```

| Rule                                                    | Example                              |
|---------------------------------------------------------|--------------------------------------|
| Slicing never raises `IndexError`                       | `t1[2:100]` → `(9, 12, 15, 18, 21)`  |
| Empty slice → `()`                                      | `t1[4:2]` → `()`                     |
| Negative step goes right to left                        | `t1[5:1:-1]` → `(18, 15, 12, 9)`     |
| `step` cannot be 0                                      | `t1[::0]` → `ValueError`             |

> Slice assignment (`t1[1:3] = (0,)`) is an **error** for tuples because they are immutable.

---

## 5. Packing and Unpacking

### Packing: putting values into a tuple

```python
T5 = 1, 2, 3, 4, 5, 6, 7        # no brackets needed
print(T5)                       # (1, 2, 3, 4, 5, 6, 7)
```

### Unpacking: taking values out into variables

The number of variables must **match** the number of elements.

```python
T1 = (1, 2, 3)

a, b, c = T1
print(a, b, c)                  # 1 2 3

a, b, c, d = 1, 2, 3, 4         # packing and unpacking in one line
print(a, b, c, d)               # 1 2 3 4
```

If the counts differ, Python raises a `ValueError`:

```python
a, b, c, d, e = T1
# ValueError: not enough values to unpack (expected 5, got 3)

a, b = T1
# ValueError: too many values to unpack (expected 2)
```

### Star unpacking `*`: collect the rest

One variable with a `*` collects **all the remaining elements** into a **list**.

```python
T1 = (1, 2, 3)

a, b, *c = T1
print(a, b, c)                  # 1 2 [3]       ← c is a LIST
```

```python
first, *middle, last = (1, 2, 3, 4, 5)
print(first)                    # 1
print(middle)                   # [2, 3, 4]
print(last)                     # 5

head, *tail = (10, 20, 30)
print(head, tail)               # 10 [20, 30]

*init, last = (10, 20, 30)
print(init, last)               # [10, 20] 30

a, *b = (1,)
print(a, b)                     # 1 []          the starred variable can be empty
```

Rules:
- Only **one** starred variable is allowed in one unpacking.
- The starred variable always receives a **list** (even when unpacking a tuple).
- Works with **any iterable**: `a, *b = 'hello'` gives `a = 'h'`, `b = ['e', 'l', 'l', 'o']`.

### Ignoring values with `_`

```python
name, _, marks = ('Asha', 21, 90)
print(name, marks)              # Asha 90
```

### Swapping without a temporary variable

```python
a, b = 10, 20
a, b = b, a
print(a, b)                     # 20 10
```

(Right side is packed into the tuple `(b, a)`, then unpacked.)

### Nested unpacking

```python
(a, (b, c)), d = (1, (2, 3)), 4
print(a, b, c, d)               # 1 2 3 4

for i, (x, y) in enumerate([(1, 2), (3, 4)]):
    print(i, x, y)              # 0 1 2   then   1 3 4
```

### The `*` operator with function calls and joining

`*` in front of a tuple **spreads** its items.

```python
t = (1, 2, 3)
print(*t)                       # 1 2 3
print(*t, sep='-')              # 1-2-3

def add(x, y, z):
    return x + y + z
print(add(*t))                  # 6

t1 = (1, 2, 3)
t2 = (8, 9, 10)
print((*t1, *t2))               # (1, 2, 3, 8, 9, 10)
print((*t1, 99, *t2))           # (1, 2, 3, 99, 8, 9, 10)
```

### Returning several values from a function

Python is really returning **one tuple**.

```python
def min_max(L):
    return min(L), max(L)

result = min_max([4, 8, 1, 6])
print(result)                   # (1, 8)

lo, hi = min_max([4, 8, 1, 6])
print(lo, hi)                   # 1 8
```

---

## 6. Operators on Tuples

| Operation      | Operator              | Result                              |
|----------------|-----------------------|-------------------------------------|
| Concatenation  | `+`                   | a **new** tuple                     |
| Repetition     | `*`                   | a **new** tuple                     |
| Membership     | `in`, `not in`        | `True` / `False`                    |
| Comparison     | `< <= > >= == !=`     | `True` / `False`                    |
| Indexing/slice | `t[i]`, `t[a:b:c]`    | element / new tuple                 |

### Concatenation `+`

```python
T1 = (1, 2, 3)
T2 = (8, 9, 10)

t3 = T1 + T2
print(t3)                       # (1, 2, 3, 8, 9, 10)
print(T1)                       # (1, 2, 3)   unchanged
```

Both sides must be tuples:

```python
print((1, 2) + [3])             # ❌ TypeError: can only concatenate tuple (not "list") to tuple
print((1, 2) + (3))             # ❌ TypeError (the (3) is an int)
print((1, 2) + (3,))            # ✅ (1, 2, 3)
```

`+=` creates a new tuple and rebinds the name:

```python
x = (1, 2, 3)
y = x
y += (4,)
print(x, y)                     # (1, 2, 3) (1, 2, 3, 4)   x is not affected
```

### Repetition `*`

```python
T1 = (1, 2, 3)
T4 = T1 * 3
print(T4)                       # (1, 2, 3, 1, 2, 3, 1, 2, 3)

print((0,) * 5)                 # (0, 0, 0, 0, 0)
print((1, 2) * 0)               # ()
print((0) * 5)                  # 0     ← (0) is an int, so this is multiplication!
```

### Membership `in` / `not in`

```python
T = (1, 2, 3, 4, 5)
print(3 in T)                   # True
print(9 in T)                   # False
print(9 not in T)               # True

names = ('red', 'green', 'blue')
print('green' in names)         # True
print('gre' in names)           # False    (must match a whole element)
```

Like lists, `in` checks only the **top-level** elements:

```python
t = (1, (2, 3), 4)
print(2 in t)                   # False
print((2, 3) in t)              # True
print(2 in t[1])                # True
```

### Comparison

Tuples are compared **element by element from the left** (like lists and strings). The first difference decides.

```python
print((1, 2, 3) == (1, 2, 3))   # True
print((1, 2, 3) == (3, 2, 1))   # False    order matters
print((1, 2, 3) < (1, 2, 4))    # True     3 < 4
print((1, 2) < (1, 2, 0))       # True     prefix is smaller
print((3,) > (1, 9, 9))         # True     3 > 1
print((1, 2) == [1, 2])         # False    a tuple never equals a list
```

This is why a list of tuples sorts naturally:

```python
print(sorted([(2, 'b'), (1, 'z'), (2, 'a')]))    # [(1, 'z'), (2, 'a'), (2, 'b')]
```

---

## 7. Tuple Traversal

```python
t1 = (5, 6, 7, 8, 9)

# 1. for loop
for x in t1:
    print(x)

# 2. with the index
for i in range(len(t1)):
    print(i, t1[i])

# 3. while loop
i = 0
while i < len(t1):
    print(t1[i])
    i += 1

# 4. enumerate
for i, x in enumerate(t1, start=1):
    print(i, x)

# 5. backwards
for x in reversed(t1):
    print(x)
```

Tuples of tuples (records):

```python
students = (('Asha', 90), ('Ravi', 85), ('Meena', 92))

for name, marks in students:        # unpack each record
    print(name, marks)
```

---

## 8. Tuple "Comprehension"

### Important fact: there is NO tuple comprehension

Round brackets with a `for` inside do **not** create a tuple. They create a **generator expression**.

```python
g = (x for x in range(1, 5))
print(g)                  # <generator object <genexpr> at 0x...>
print(type(g))            # <class 'generator'>
```

A generator makes values **one at a time** and can be used **only once**.

```python
print(list(g))            # [1, 2, 3, 4]
print(list(g))            # []     ← already used up
```

> 📝 In your notes `T = (iterable)` (just brackets) does not make a tuple from an iterable. Use `tuple(iterable)`.

### Way 1: `tuple(generator_expression)` (recommended)

```python
T2 = tuple(x for x in range(1, 5))
print(T2)                 # (1, 2, 3, 4)

T3 = tuple(x ** 2 for x in range(1, 5))
print(T3)                 # (1, 4, 9, 16)
```

(When `tuple()` receives a generator as its only argument, you do not need extra brackets.)

### Way 2: the star trick `(*generator, )`

The `*` **unpacks** the generator inside a tuple display. The **trailing comma** is required.

```python
T1 = (*(x for x in range(1, 5)),)
print(T1)                 # (1, 2, 3, 4)

T4 = (*(x.lower() for x in 'pyThoN'),)
print(T4)                 # ('p', 'y', 't', 'h', 'o', 'n')

T5 = (*(int(x) for x in '1234'),)
print(T5)                 # (1, 2, 3, 4)
```

> 📝 In your notes `rnage` is a typo for `range`.

### Fixing the T6 example

```python
T6 = ((x for x in 'abcd7e' if x.isalpha()),)
print(T6)                 # (<generator object <genexpr> at 0x...>,)
```

This makes a tuple **containing one generator object**, because the `*` is missing. Correct versions:

```python
T6 = (*(x for x in 'abcd7e' if x.isalpha()),)
print(T6)                 # ('a', 'b', 'c', 'd', 'e')

T6 = tuple(x for x in 'abcd7e' if x.isalpha())
print(T6)                 # ('a', 'b', 'c', 'd', 'e')
```

### More examples

```python
# filter
print(tuple(x for x in range(10) if x % 2 == 0))               # (0, 2, 4, 6, 8)

# if-else
print(tuple('even' if x % 2 == 0 else 'odd' for x in range(3))) # ('even', 'odd', 'even')

# nested (tuple of tuples)
print(tuple(tuple(j for j in range(i)) for i in range(1, 4)))
# ((0,), (0, 1), (0, 1, 2))

# with map and filter
print(tuple(map(str, (1, 2, 3))))                              # ('1', '2', '3')
print(tuple(filter(None, (0, 1, '', 'a'))))                    # (1, 'a')
```

### Alternatives

| Way                                  | Builds a list first? | Note                              |
|--------------------------------------|----------------------|-----------------------------------|
| `tuple(x for x in ...)`              | ❌ no                | clean and memory friendly         |
| `tuple([x for x in ...])`            | ✅ yes               | works, but makes a temporary list |
| `(*(x for x in ...),)`               | ❌ no                | the "star trick"                  |

---

## 9. Tuple Methods and Built-in Functions

### The only two methods

Because tuples cannot change, they have just **two methods**.

| Method                       | Purpose                               | If missing         |
|------------------------------|---------------------------------------|--------------------|
| `count(x)`                   | how many times `x` appears            | returns `0`        |
| `index(x, start, end)`       | position of the **first** `x`         | raises `ValueError`|

```python
t1 = (6, 5, 4, 2, 3, 2)

print(t1.count(2))         # 2
print(t1.count(9))         # 0
print(t1.index(2))         # 3       first 2
print(t1.index(2, 4))      # 5       search from index 4 onward
print(t1.index(9))         # ❌ ValueError: tuple.index(x): x not in tuple
```

### Built-in functions that work on tuples

```python
t = (4, 1, 7, 3)

print(len(t))                  # 4
print(min(t), max(t))          # 1 7
print(sum(t))                  # 15
print(sorted(t))               # [1, 3, 4, 7]    ← returns a LIST
print(tuple(sorted(t)))        # (1, 3, 4, 7)    ← a sorted tuple
print(tuple(reversed(t)))      # (3, 7, 1, 4)
print(any((0, 0, 1)))          # True
print(all((1, 2, 0)))          # False
print(list(t))                 # [4, 1, 7, 3]
```

> There is no `t.sort()` or `t.reverse()`. Use `sorted(t)` and `t[::-1]`, which build new objects.

### Comparison with list methods

| List method                                 | Tuple?              |
|---------------------------------------------|---------------------|
| `append`, `extend`, `insert`                | ❌ (changes data)   |
| `remove`, `pop`, `clear`                    | ❌ (changes data)   |
| `sort`, `reverse`                           | ❌ (changes order)  |
| `copy`                                      | ❌ (not needed; tuples cannot change, `t[:]` returns the same tuple) |
| `count`, `index`                            | ✅                  |

---

## 10. Tuple vs List

| Feature                  | List                         | Tuple                                   |
|--------------------------|------------------------------|-----------------------------------------|
| Brackets                 | `[ ]`                        | `( )` (or just commas)                  |
| Mutable                  | ✅                           | ❌                                      |
| Methods                  | 11                           | 2 (`count`, `index`)                    |
| Dictionary key / set item| ❌ (unhashable)              | ✅ (if all items are hashable)          |
| Speed and memory         | slightly slower, larger      | slightly faster, smaller                |
| Typical use              | collection that **changes**  | fixed **record** (point, date, colour)  |
| Comprehension            | `[x for ...]`                | none (use `tuple(x for ...)`)           |
| Single element           | `[3]`                        | `(3,)`                                  |
| Safe to share            | needs care (can be changed)  | ✅ cannot be changed by others          |

**Rule of thumb:**
- Items of the **same kind** that grow or shrink → **list** (`marks`, `names`).
- Items of **different meaning** forming one fixed record → **tuple** (`('Asha', 21, 'Bengaluru')`, `(x, y)`, `(r, g, b)`).

Converting:

```python
list((1, 2, 3))        # [1, 2, 3]
tuple([1, 2, 3])       # (1, 2, 3)
```

---

## 11. Advanced Topics

### 1. Immutable, but its items can be mutable

A tuple cannot change **which objects it holds**, but if one of them is a list, **that list can still change**.

```python
t = (1, [2, 3])
t[1].append(4)
print(t)                # (1, [2, 3, 4])      the tuple is "changed" through its list

t[1] = [9]              # ❌ TypeError (cannot replace the element itself)
```

A famous gotcha:

```python
t = (1, [2, 3])
t[1] += [4]             # TypeError ... but the list WAS extended first!
print(t)                # (1, [2, 3, 4])
```

A tuple is **truly constant** only if all its elements are immutable too.

### 2. Hashability: tuples as dictionary keys and set items

```python
grid = {(0, 0): 'start', (2, 3): 'goal'}
print(grid[(2, 3)])                 # goal

visited = {(0, 0), (0, 1)}
print((0, 1) in visited)            # True

print(hash((1, 2)))                 # works
hash((1, [2]))                      # ❌ TypeError: unhashable type: 'list'
{[1, 2]: 'a'}                       # ❌ TypeError (a list cannot be a key)
```

### 3. Named tuples: tuples with field names

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)

print(p)                # Point(x=3, y=4)
print(p.x, p[1])        # 3 4        by name or by index
x, y = p                # still a tuple: unpacking works
print(p._replace(x=10)) # Point(x=10, y=4)    creates a new one
```

### 4. `*args` is a tuple

```python
def show(*args):
    print(type(args), args)

show(1, 2, 3)           # <class 'tuple'> (1, 2, 3)
```

### 5. Extended unpacking in loops

```python
for first, *rest in [(1, 2, 3), (4, 5, 6)]:
    print(first, rest)
# 1 [2, 3]
# 4 [5, 6]
```

### 6. Pattern matching with tuples

```python
point = (0, 5)
match point:
    case (0, 0):
        print('origin')
    case (0, y):
        print('on y-axis at', y)       # this runs
    case (x, 0):
        print('on x-axis at', x)
```

### 7. `del`

```python
t = (1, 2, 3)
del t[0]            # ❌ TypeError
del t               # ✅ deletes the NAME t (the tuple object is freed if nothing else uses it)
```

### 8. Identity and copying

```python
t = (1, 2, 3)
print(t[:] is t)            # True   slicing the whole tuple returns the same object
print(tuple(t) is t)        # True   tuple() of a tuple returns the same object
print(() is ())             # True   the empty tuple is a single shared object
```

No need for a `copy()` method: an immutable object can be shared safely. (Do **not** rely on `is` to compare tuple values; use `==`.)

### 9. Type hints

```python
def get_point() -> tuple[int, int]:
    return (3, 4)

def get_row() -> tuple[str, int, float]:     # fixed length, each position typed
    return ('Asha', 21, 88.5)

def numbers() -> tuple[int, ...]:            # any length, all ints
    return (1, 2, 3)
```

---

## 12. Mini Programs

**1. Swap two numbers**

```python
a, b = 5, 9
a, b = b, a
print(a, b)                       # 9 5
```

**2. Function returning several values**

```python
def stats(L):
    return min(L), max(L), sum(L) / len(L)

print(stats([4, 8, 6]))           # (4, 8, 6.0)
lo, hi, avg = stats([4, 8, 6])
```

**3. Sort records by one field**

```python
data = [('Asha', 90), ('Ravi', 85), ('Meena', 92)]
print(sorted(data, key=lambda t: t[1]))
# [('Ravi', 85), ('Asha', 90), ('Meena', 92)]
print(max(data, key=lambda t: t[1]))      # ('Meena', 92)
```

**4. "Modify" a tuple via a list**

```python
t = (1, 2, 3)
lst = list(t)
lst[1] = 99
t = tuple(lst)
print(t)                          # (1, 99, 3)
```

**5. Remove duplicates, keeping the order**

```python
print(tuple(dict.fromkeys((3, 1, 3, 2, 1))))     # (3, 1, 2)
```

**6. Coordinates as dictionary keys**

```python
board = {(0, 0): 'X', (1, 1): 'O'}
print(board.get((1, 1)))          # O
print(board.get((2, 2), '.'))     # .
```

**7. Constants**

```python
RED = (255, 0, 0)
r, g, b = RED
print(r, g, b)                    # 255 0 0

date = (2024, 5, 17)
year, month, day = date
print(f'{day}/{month}/{year}')    # 17/5/2024
```

**8. `zip` and `enumerate` produce tuples**

```python
print(list(zip([1, 2], ['a', 'b'])))       # [(1, 'a'), (2, 'b')]
print(list(enumerate('ab')))               # [(0, 'a'), (1, 'b')]
print(list({'x': 1}.items()))              # [('x', 1)]
```

**9. Max of tuples compares element by element**

```python
print(max([(2, 'b'), (3, 'a'), (3, 'c')]))   # (3, 'c')
```

**10. Merge tuples and unpack the ends**

```python
t = (*(1, 2, 3), *(4, 5))
first, *mid, last = t
print(t, first, mid, last)        # (1, 2, 3, 4, 5) 1 [2, 3, 4] 5
```

---

## 13. Cheat Sheet

```text
CREATE        (1, 2, 3)    ()    (3,)    3,    tuple(iterable)    tuple('abc')
              (3) is an int !   the COMMA makes the tuple
PROPERTIES    ordered · immutable · duplicates · mixed types · hashable (if items are)

READ          t[i]   t[-1]   t[a:b:c]   (slice never raises IndexError)
WRITE         impossible: t[i] = x, append, remove ... → errors
"CHANGE"      t = t + (x,)     t = t[:i] + (x,) + t[i+1:]     or  list → edit → tuple

PACK          t = 1, 2, 3
UNPACK        a, b, c = t       a, *b = t (b is a list)       a, b = b, a (swap)
SPREAD        print(*t)    f(*t)    (*t1, *t2)

OPERATORS     +   *   in   not in   ==  !=  <  <=  >  >=
METHODS       t.count(x)    t.index(x, start, end)
BUILT-INS     len  min  max  sum  sorted (returns list)  reversed  any  all

"COMPREHENSION"   tuple(x for x in ...)     (*(x for x in ...),)     ← (x for x in ...) alone is a generator
```

---

## 14. Common Mistakes

| Mistake                                              | Fix                                                        |
|------------------------------------------------------|------------------------------------------------------------|
| `t = (3)` expecting a tuple                          | `t = (3,)`                                                 |
| `tuple(1, 2, 3)`                                     | `tuple((1, 2, 3))` or `(1, 2, 3)`                          |
| `tup;e(...)`, `rnage`, `5+6i`                        | `tuple`, `range`, `5+6j`                                   |
| `t[0] = 9`, `t.append(4)`                            | tuples are immutable: build a new tuple                    |
| `(1, 2) + [3]` or `(1, 2) + (3)`                     | `(1, 2) + (3,)`                                            |
| `(0) * 5` expecting `(0, 0, 0, 0, 0)`                | `(0,) * 5`                                                 |
| `T = (x for x in range(4))` expecting a tuple        | that is a generator: use `tuple(x for x in range(4))`      |
| Forgetting `*` or the trailing comma in `(*(gen),)`  | `(*(x for x in ...),)`                                     |
| `a, b, c, d, e = (1, 2, 3)`                          | number of variables must match, or use a `*` variable      |
| Expecting the starred variable to be a tuple         | `a, *b = t` makes `b` a **list**                           |
| Using `t.sort()` or `t.reverse()`                    | `sorted(t)`, `t[::-1]`                                     |
| Believing a tuple with a list inside is fully constant | the inner list can still change                          |
| Using a list as a dictionary key                     | use a tuple                                                |
| `x = 1,` by accident (a stray comma)                 | it creates a tuple `(1,)`, not `1`                         |
| `t.index(x)` when `x` may be missing                 | check `x in t` first                                       |
| `print(sorted(t))` and expecting a tuple             | `sorted` returns a list; wrap with `tuple(...)`            |

---

## 15. Practice Questions

Predict the output first.

```python
# 1
print(type((3)), type((3,)))

# 2
t = (6, 5, 4, 2, 3, 2)
print(t[2], t.count(2), t.index(2))

# 3
print(t[1:4])

# 4
print(t[::-1])

# 5
a, *b = (1, 2, 3)
print(a, b)

# 6
a, b = 1, 2
a, b = b, a
print(a, b)

# 7
print((1, 2) * 2 + (3,))

# 8
print(2 in (1, (2, 3)))

# 9
print(tuple('hi'))

# 10
print(tuple(x * x for x in range(4)))

# 11
print((1, 2, 3) < (1, 2, 4), (1, 2) < (1, 2, 0))

# 12
t = (1, [2, 3])
t[1].append(4)
print(t)

# 13
t = (1, 2, 3)
t[0] = 9

# 14
first, *mid, last = range(1, 6)
print(first, mid, last)

# 15
print(len(()), len((1,)), bool(()))

# 16
x = (1, 2, 3)
y = x
y += (4,)
print(x, y)

# 17
print(sorted((3, 1, 2)))

# 18
print(*(1, 2, 3), sep='-')
```

<details>
<summary>Answers</summary>

1. `<class 'int'> <class 'tuple'>`
2. `4 2 3`
3. `(5, 4, 2)`
4. `(2, 3, 2, 4, 5, 6)`
5. `1 [2, 3]` (the starred variable is a list)
6. `2 1`
7. `(1, 2, 1, 2, 3)`
8. `False` (2 is inside the nested tuple)
9. `('h', 'i')`
10. `(0, 1, 4, 9)`
11. `True True`
12. `(1, [2, 3, 4])`
13. `TypeError: 'tuple' object does not support item assignment`
14. `1 [2, 3, 4] 5`
15. `0 1 False`
16. `(1, 2, 3) (1, 2, 3, 4)`
17. `[1, 2, 3]` (a list)
18. `1-2-3`

</details>

### Mini challenges

1. Read a line of numbers, convert it into a tuple of integers, and print the min, max and sum.
2. Write a function that returns the quotient and remainder of two numbers as a tuple, and unpack the result.
3. Given a tuple, create a new tuple with its first and last elements swapped.
4. Remove all occurrences of a value from a tuple (hint: filter and convert back).
5. Given a list of `(name, marks)` tuples, print the names sorted by marks, highest first.
6. Rotate a tuple left by `k` positions using slicing and `+`.
7. Use tuples as dictionary keys to store a 3x3 board and print it.
8. Create a tuple of the first 10 squares using `tuple(...)` with a generator.
9. Use star unpacking to print the first, last and the sum of the middle elements of a tuple.
10. Define a `namedtuple` for a student with `name`, `age` and `marks`, and print the student with the highest marks.
