# Python List Comprehension and Nested List Comprehension
### Beginner → Advanced Notes

---

## Table of Contents
1. [What is a List Comprehension?](#1-what-is-a-list-comprehension)
2. [Basic Syntax and Anatomy](#2-basic-syntax-and-anatomy)
3. [Comprehension with a Condition (Filtering)](#3-comprehension-with-a-condition-filtering)
4. [Comprehension with if-else (Transforming)](#4-comprehension-with-if-else-transforming)
5. [Working with Strings, Functions and Other Iterables](#5-working-with-strings-functions-and-other-iterables)
6. [Multiple for Clauses (Nested Loops in One Line)](#6-multiple-for-clauses-nested-loops-in-one-line)
7. [Nested List Comprehension (Lists of Lists)](#7-nested-list-comprehension-lists-of-lists)
8. [Flat vs Nested: Do Not Mix Them Up](#8-flat-vs-nested-do-not-mix-them-up)
9. [Set, Dictionary and Generator Comprehensions](#9-set-dictionary-and-generator-comprehensions)
10. [Advanced Topics](#10-advanced-topics)
11. [When NOT to Use a Comprehension](#11-when-not-to-use-a-comprehension)
12. [Mini Programs](#12-mini-programs)
13. [Cheat Sheet](#13-cheat-sheet)
14. [Common Mistakes](#14-common-mistakes)
15. [Practice Questions](#15-practice-questions)

---

## 1. What is a List Comprehension?

### Beginner level

A **list comprehension** is a short way to **create a new list** from an existing iterable (a list, string, range...) in **a single line**.

**Task:** make a list of the squares of 1 to 5.

**With a normal loop (4 lines):**

```python
squares = []
for x in range(1, 6):
    squares.append(x * x)
print(squares)          # [1, 4, 9, 16, 25]
```

**With a list comprehension (1 line):**

```python
squares = [x * x for x in range(1, 6)]
print(squares)          # [1, 4, 9, 16, 25]
```

Both give the same result. The comprehension is shorter and usually easier to read once you know the pattern.

### Why use it?

| Benefit                | Explanation                                                  |
|------------------------|--------------------------------------------------------------|
| **Short**              | replaces `result = []` + loop + `append`                     |
| **Readable**           | the whole idea ("squares of 1 to 5") sits in one line        |
| **Fast**               | usually a little faster than the same loop with `append()`   |
| **Pythonic**           | the style Python programmers expect                          |

> The word is **comprehension** (it "comprehends", i.e. describes, the whole list at once).

---

## 2. Basic Syntax and Anatomy

```python
[ expression  for  item  in  iterable ]
```

```
[ x * x   for   x   in   range(1, 6) ]
  ^            ^         ^
  |            |         └── where the items come from (any iterable)
  |            └── name for one item at a time
  └── what goes into the new list for each item
```

### How to read it

> "Give me `x * x` **for** every `x` **in** `range(1, 6)`."

### How to convert a loop into a comprehension (3 steps)

```python
result = []                       # (a) the new list  -> becomes the [ ]
for x in range(1, 6):             # (b) the for line  -> goes AFTER the expression
    result.append(x * x)          # (c) what is appended -> goes at the FRONT
```

1. Take **what you append** (`x * x`) → put it first.
2. Add the **`for` line** (`for x in range(1, 6)`) after it.
3. Wrap everything in **`[ ]`**.

### More basic examples

```python
nums = [1, 2, 3, 4]

print([x * 2 for x in nums])           # [2, 4, 6, 8]
print([x + 10 for x in nums])          # [11, 12, 13, 14]
print([x ** 3 for x in nums])          # [1, 8, 27, 64]
print([x for x in nums])               # [1, 2, 3, 4]   (a copy)

print([0 for _ in range(5)])           # [0, 0, 0, 0, 0]
print(['hi' for _ in range(3)])        # ['hi', 'hi', 'hi']
print([3 * i for i in range(1, 6)])    # [3, 6, 9, 12, 15]   (table of 3)
```

`_` is a conventional name for a loop variable you do not use.

> ✅ A list comprehension **always creates a new list**. The original list is not changed.

---

## 3. Comprehension with a Condition (Filtering)

Add an **`if`** at the **end** to **keep only** the items that pass.

```python
[ expression  for  item  in  iterable  if  condition ]
```

```python
evens = [x for x in range(1, 11) if x % 2 == 0]
print(evens)                           # [2, 4, 6, 8, 10]

positives = [x for x in [3, -1, 4, -5] if x > 0]
print(positives)                       # [3, 4]
```

Loop version:

```python
evens = []
for x in range(1, 11):
    if x % 2 == 0:
        evens.append(x)
```

### Filter and transform together

```python
# squares of the even numbers only
print([x * x for x in range(1, 11) if x % 2 == 0])     # [4, 16, 36, 64, 100]
```

The `if` decides **which items are used**; the expression at the front decides **what is stored**.

### Several conditions

```python
# divisible by both 2 and 3
print([x for x in range(1, 31) if x % 2 == 0 if x % 3 == 0])     # [6, 12, 18, 24, 30]
print([x for x in range(1, 31) if x % 2 == 0 and x % 3 == 0])    # same result

# divisible by 2 or 3
print([x for x in range(1, 11) if x % 2 == 0 or x % 3 == 0])     # [2, 3, 4, 6, 8, 9, 10]
```

### Remove unwanted values

```python
L = [1, 2, 3, 2, 4, 2]
print([x for x in L if x != 2])        # [1, 3, 4]    (remove every 2)
```

---

## 4. Comprehension with if-else (Transforming)

To choose **between two values** for each item, use the **conditional expression** `A if condition else B` at the **front**.

```python
[ value_if_true  if  condition  else  value_if_false   for  item  in  iterable ]
```

```python
print(['even' if x % 2 == 0 else 'odd' for x in range(5)])
# ['even', 'odd', 'even', 'odd', 'even']

print([x if x > 0 else 0 for x in [-1, 2, -3, 4]])
# [0, 2, 0, 4]     (negative numbers replaced by 0)

print(['pass' if m >= 35 else 'fail' for m in [80, 20, 35]])
# ['pass', 'fail', 'pass']
```

### The most important rule: where does `if` go?

| Goal                              | Position of `if`       | Pattern                                           |
|-----------------------------------|------------------------|---------------------------------------------------|
| **Filter** (drop some items)      | **at the end**         | `[x for x in L if condition]`                     |
| **Choose** a value (keep all items) | **at the front**, with `else` | `[A if condition else B for x in L]`       |

```python
L = [1, 2, 3, 4, 5]

print([x for x in L if x % 2 == 0])             # [2, 4]            only 2 items (filter)
print([x if x % 2 == 0 else 0 for x in L])      # [0, 2, 0, 4, 0]   still 5 items (choose)
```

A common error:

```python
[x for x in L if x % 2 == 0 else 0]      # ❌ SyntaxError (else is not allowed after the filter if)
[x if x % 2 == 0 else 0 for x in L]      # ✅
```

### Several branches (chained conditional)

```python
marks = [95, 72, 55, 30]
grades = ['A' if m >= 90 else 'B' if m >= 60 else 'C' if m >= 40 else 'F' for m in marks]
print(grades)                             # ['A', 'B', 'C', 'F']
```

(If this gets hard to read, use a normal loop or a helper function.)

### Filter and choose together

```python
# only numbers above 0, labelled
print(['big' if x > 10 else 'small' for x in [5, 15, -2, 30] if x > 0])
# ['small', 'big', 'big']
```

---

## 5. Working with Strings, Functions and Other Iterables

A comprehension can loop over **any iterable**: string, tuple, range, set, dictionary, file lines, `zip()`, `enumerate()`.

### Strings

```python
print([c.upper() for c in 'abc'])                       # ['A', 'B', 'C']
print([c for c in 'education' if c in 'aeiou'])         # ['e', 'u', 'a', 'i', 'o']
print([int(c) for c in 'a1b2c3' if c.isdigit()])        # [1, 2, 3]
print([ord(c) for c in 'ABC'])                          # [65, 66, 67]
print([len(w) for w in 'the quick brown fox'.split()])  # [3, 5, 5, 3]
print([w.capitalize() for w in 'hello big world'.split()])   # ['Hello', 'Big', 'World']
print([s.strip() for s in ' a , b '.split(',')])        # ['a', 'b']
```

### Calling functions

```python
print([abs(x) for x in [-3, 2, -1]])               # [3, 2, 1]
print([round(x, 1) for x in [1.26, 2.34]])         # [1.3, 2.3]
print([str(x) for x in range(3)])                  # ['0', '1', '2']
print([int(x) for x in input().split()])           # numbers typed by the user
```

### `enumerate()`: index and value

```python
L = [10, 20, 10, 30]
print([i for i, x in enumerate(L) if x == 10])      # [0, 2]    positions of 10
print([f'{i}:{x}' for i, x in enumerate(L)])        # ['0:10', '1:20', '2:10', '3:30']
```

### `zip()`: two lists together

```python
a = [1, 2, 3]
b = [10, 20, 30]
print([x + y for x, y in zip(a, b)])        # [11, 22, 33]
print([f'{n} scored {m}' for n, m in zip(['Asha', 'Ravi'], [90, 85])])
# ['Asha scored 90', 'Ravi scored 85']
```

### Dictionary

```python
d = {'a': 1, 'b': 5, 'c': 3}
print([k for k, v in d.items() if v > 2])       # ['b', 'c']
print([v * 10 for v in d.values()])             # [10, 50, 30]
```

### Using `all()` and `any()` inside (a prime-number list)

```python
primes = [n for n in range(2, 30)
          if all(n % i != 0 for i in range(2, int(n ** 0.5) + 1))]
print(primes)      # [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

### Formatting output

```python
print([f'3 x {i} = {3 * i}' for i in range(1, 4)])
# ['3 x 1 = 3', '3 x 2 = 6', '3 x 3 = 9']
```

---

## 6. Multiple for Clauses (Nested Loops in One Line)

You can write **more than one `for`** inside the same brackets. This replaces **nested loops** and gives **one flat list**.

```python
[ expression  for item1 in iterable1  for item2 in iterable2 ]
```

### Same order as nested loops

The **first `for` is the outer loop**, the **last `for` is the inner loop**.

```python
result = [(i, j) for i in range(1, 4) for j in range(1, 3)]
print(result)
# [(1, 1), (1, 2), (2, 1), (2, 2), (3, 1), (3, 2)]
```

Equivalent nested loop:

```python
result = []
for i in range(1, 4):             # first for  -> OUTER loop
    for j in range(1, 3):         # second for -> INNER loop
        result.append((i, j))
```

Trace:

| `i` (outer) | `j` (inner) | stored   |
|-------------|-------------|----------|
| 1           | 1, 2        | (1,1), (1,2) |
| 2           | 1, 2        | (2,1), (2,2) |
| 3           | 1, 2        | (3,1), (3,2) |

### More examples

```python
# all products
print([i * j for i in range(1, 4) for j in range(1, 4)])
# [1, 2, 3, 2, 4, 6, 3, 6, 9]

# combinations of two lists
print([(n, c) for n in [1, 2] for c in 'ab'])
# [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]

# unique pairs: the inner range depends on the outer variable
print([(i, j) for i in range(1, 5) for j in range(i + 1, 5)])
# [(1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]

# with a condition at the end
print([(i, j) for i in range(1, 4) for j in range(1, 4) if i != j])
# [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]
```

### Flatten a list of lists (very common)

```python
matrix = [[1, 2, 3],
          [4, 5, 6],
          [7, 8, 9]]

flat = [x for row in matrix for x in row]
print(flat)               # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Read it as the nested loop it replaces:

```python
flat = []
for row in matrix:        # first  for
    for x in row:         # second for
        flat.append(x)
```

> ⚠️ The order is **`for row in matrix`** first, then **`for x in row`**. The second clause uses the variable of the first, so the first must come first.

With a filter:

```python
print([x for row in matrix for x in row if x % 2 == 0])      # [2, 4, 6, 8]
```

### Three loops: Pythagorean triples

```python
triples = [(a, b, c)
           for a in range(1, 21)
           for b in range(a, 21)
           for c in range(b, 21)
           if a * a + b * b == c * c]
print(triples)
# [(3, 4, 5), (5, 12, 13), (6, 8, 10), (8, 15, 17), (9, 12, 15), (12, 16, 20)]
```

> Tip: when the comprehension is long, **split it over several lines** (one clause per line) as above.

---

## 7. Nested List Comprehension (Lists of Lists)

A **nested list comprehension** is a list comprehension **inside another list comprehension**. It builds a **list of lists** (a matrix / 2D list).

```python
[ [ inner_expression  for inner_item in inner_iterable ]   for outer_item in outer_iterable ]
```

- The **inner** comprehension builds **one row**.
- The **outer** comprehension **repeats it** for each row.

### Build a matrix

```python
matrix = [[j for j in range(1, 4)] for i in range(3)]
print(matrix)
# [[1, 2, 3], [1, 2, 3], [1, 2, 3]]
```

Equivalent nested loop:

```python
matrix = []
for i in range(3):
    row = []
    for j in range(1, 4):
        row.append(j)
    matrix.append(row)
```

### Multiplication table

```python
table = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print(table)
# [[1, 2, 3], [2, 4, 6], [3, 6, 9]]

for row in table:
    print(row)
# [1, 2, 3]
# [2, 4, 6]
# [3, 6, 9]
```

### Matrix of zeros (and the famous trap)

```python
zeros = [[0] * 3 for _ in range(3)]
print(zeros)             # [[0, 0, 0], [0, 0, 0], [0, 0, 0]]

zeros[0][0] = 1
print(zeros)             # [[1, 0, 0], [0, 0, 0], [0, 0, 0]]   ✅ only one row changed
```

Compare with repetition `*`:

```python
bad = [[0] * 3] * 3      # the SAME inner list repeated three times
bad[0][0] = 1
print(bad)               # [[1, 0, 0], [1, 0, 0], [1, 0, 0]]   ❌ all rows changed
```

> Always build matrices with a **nested comprehension**, not `[[0]*n]*m`.

### Identity matrix (with if-else)

```python
identity = [[1 if i == j else 0 for j in range(3)] for i in range(3)]
print(identity)          # [[1, 0, 0], [0, 1, 0], [0, 0, 1]]
```

### Work on an existing matrix

```python
m = [[1, 2, 3],
     [4, 5, 6]]
```

| Task                         | Code                                           | Result                       |
|------------------------------|------------------------------------------------|------------------------------|
| Change every element         | `[[x * 2 for x in row] for row in m]`          | `[[2, 4, 6], [8, 10, 12]]`   |
| Keep only some elements in each row | `[[x for x in row if x % 2 == 0] for row in m]` | `[[2], [4, 6]]`         |
| Keep only some rows          | `[row for row in m if sum(row) > 6]`           | `[[4, 5, 6]]`                |
| Sum of each row              | `[sum(row) for row in m]`                      | `[6, 15]`                    |
| First column                 | `[row[0] for row in m]`                        | `[1, 4]`                     |
| Maximum of each row          | `[max(row) for row in m]`                      | `[3, 6]`                     |
| Copy the matrix (rows too)   | `[row[:] for row in m]`                        | independent copy             |

### Transpose (rows ↔ columns)

```python
m = [[1, 2, 3],
     [4, 5, 6]]

t = [[row[i] for row in m] for i in range(3)]
print(t)                       # [[1, 4], [2, 5], [3, 6]]
```

How it works: for each column index `i` (outer), collect `row[i]` from every row (inner).

Shorter with `zip`:

```python
print([list(r) for r in zip(*m)])      # [[1, 4], [2, 5], [3, 6]]
print([sum(col) for col in zip(*m)])   # [5, 7, 9]   column sums
```

### Diagonals

```python
m3 = [[1, 2, 3],
      [4, 5, 6],
      [7, 8, 9]]

print([m3[i][i] for i in range(3)])        # [1, 5, 9]   main diagonal
print([m3[i][2 - i] for i in range(3)])    # [3, 5, 7]   other diagonal
```

### Convert text to a matrix of numbers

```python
lines = ['1 2 3', '4 5 6']
grid = [[int(x) for x in line.split()] for line in lines]
print(grid)                 # [[1, 2, 3], [4, 5, 6]]
```

```python
print([[c for c in w] for w in ['ab', 'cd']])       # [['a', 'b'], ['c', 'd']]
```

### A triangle of stars

```python
rows = [['*'] * i for i in range(1, 4)]
print(rows)                 # [['*'], ['*', '*'], ['*', '*', '*']]

print(['*' * i for i in range(1, 6)])     # ['*', '**', '***', '****', '*****']
```

### Three levels: a 3D list

```python
cube = [[[0] * 2 for _ in range(2)] for _ in range(2)]
print(cube)       # [[[0, 0], [0, 0]], [[0, 0], [0, 0]]]
```

### Advanced: matrix multiplication

```python
A = [[1, 2],
     [3, 4]]
B = [[5, 6],
     [7, 8]]

C = [[sum(a * b for a, b in zip(row, col)) for col in zip(*B)] for row in A]
print(C)          # [[19, 22], [43, 50]]
```

- `row` takes each row of `A`
- `col` takes each column of `B` (`zip(*B)` gives the columns)
- the innermost part is the dot product of the row and the column

### Advanced: Pascal's triangle

```python
rows = [[1]]
for _ in range(4):
    prev = rows[-1]
    rows.append([1] + [prev[i] + prev[i + 1] for i in range(len(prev) - 1)] + [1])

for r in rows:
    print(r)
# [1]
# [1, 1]
# [1, 2, 1]
# [1, 3, 3, 1]
# [1, 4, 6, 4, 1]
```

---

## 8. Flat vs Nested: Do Not Mix Them Up

Both forms use the same ingredients, but they give **different shapes**.

| Feature                | **Flat** (multiple `for`)                       | **Nested** (comprehension inside)                     |
|------------------------|-------------------------------------------------|-------------------------------------------------------|
| Brackets               | **one** pair `[ ... ]`                          | **two or more** pairs `[ [ ... ] ... ]`               |
| Result shape           | a **single list**                               | a **list of lists**                                   |
| Replaces               | nested loops that append to **one** list        | a loop that appends a **row list** to a list          |
| Order of the `for`s    | outer `for` first, inner `for` second           | inner comprehension is the **expression** of the outer |

```python
matrix = [[1, 2, 3], [4, 5, 6]]

flat   = [x for row in matrix for x in row]       # [1, 2, 3, 4, 5, 6]
nested = [[x * 2 for x in row] for row in matrix] # [[2, 4, 6], [8, 10, 12]]
```

### Same words, different structure

```python
# FLAT: 6 numbers
[i * j for i in range(1, 3) for j in range(1, 4)]
# [1, 2, 3, 2, 4, 6]

# NESTED: 2 rows of 3 numbers
[[i * j for j in range(1, 4)] for i in range(1, 3)]
# [[1, 2, 3], [2, 4, 6]]
```

### How to decide

> Ask: **"Do I want one list or a list of lists?"**
> One list → **multiple `for`** in one pair of brackets.
> A list of lists → **a comprehension inside a comprehension**.

### Tip for reading nested comprehensions

Read from the **inside out**: first understand the inner `[ ... ]` (one row), then what the outer `for` does with it (repeat for every row).

---

## 9. Set, Dictionary and Generator Comprehensions

The same idea works with other brackets.

| Kind                  | Brackets      | Example                                  | Result                         |
|-----------------------|---------------|------------------------------------------|--------------------------------|
| **List**              | `[ ]`         | `[x * x for x in range(4)]`              | `[0, 1, 4, 9]`                 |
| **Set**               | `{ }`         | `{x % 3 for x in range(10)}`             | `{0, 1, 2}` (duplicates gone)  |
| **Dictionary**        | `{k: v ...}`  | `{x: x * x for x in range(1, 4)}`        | `{1: 1, 2: 4, 3: 9}`           |
| **Generator**         | `( )`         | `(x * x for x in range(4))`              | a lazy generator object        |

```python
print({c for c in 'hello'})                       # {'h', 'e', 'l', 'o'}  (order may vary)
print({w: len(w) for w in ['hi', 'python']})      # {'hi': 2, 'python': 6}
print(sum(x * x for x in range(5)))               # 30   (generator: no list is built)
print(max(len(w) for w in ['a', 'abc', 'ab']))    # 3
```

> There is **no "tuple comprehension"**. `(x for x in ...)` creates a **generator**. For a tuple use `tuple(x for x in ...)`.

**List vs generator:** a list comprehension builds the **whole list in memory**. A generator produces **one value at a time**, which saves memory for big data or when you only need a total (`sum`, `max`, `any`, `all`).

---

## 10. Advanced Topics

### 1. Variables do not leak out

In Python 3 the loop variable of a comprehension exists **only inside** it.

```python
x = 10
squares = [x * x for x in range(3)]
print(x)              # 10    (outer x is untouched)
```

### 2. In-place update with slice assignment

A comprehension creates a **new** list. To update an **existing** list object (so other references see it):

```python
L = [1, 2, 3, 4]
alias = L
L[:] = [x * 2 for x in L]
print(alias)          # [2, 4, 6, 8]    same list, new contents
```

### 3. Walrus operator `:=` (Python 3.8+)

Compute a value **once** and reuse it in the condition and the result.

```python
print([y for x in [1, 2, 3, 4] if (y := x * x) > 5])      # [9, 16]
```

### 4. Late binding with lambdas

```python
funcs = [lambda: i for i in range(3)]
print([f() for f in funcs])            # [2, 2, 2]   ← all see the final i

funcs = [lambda i=i: i for i in range(3)]
print([f() for f in funcs])            # [0, 1, 2]   ✅ bind the current value
```

### 5. Comprehension vs `map()` and `filter()`

```python
L = [1, 2, 3, 4, 5, 6]

a = [x * x for x in L if x % 2 == 0]
b = list(map(lambda x: x * x, filter(lambda x: x % 2 == 0, L)))
print(a == b)         # True   both give [4, 16, 36]
```

Comprehensions are usually clearer than `map`/`filter` with `lambda`.

### 6. Speed

For building a list, a comprehension is typically **faster** than a loop calling `append()`, because the looping and appending happen inside Python's optimised code. Do not rewrite code **only** for speed; choose the clearest style first.

### 7. Flatten one level of a mixed list

```python
nested = [1, [2, 3], 4, [5, [6]]]
flat = [y for x in nested for y in (x if isinstance(x, list) else [x])]
print(flat)           # [1, 2, 3, 4, 5, [6]]    (only one level is flattened)
```

For any depth use a recursive function.

---

## 11. When NOT to Use a Comprehension

| Situation                                           | Better choice                              |
|-----------------------------------------------------|--------------------------------------------|
| You only want **side effects** (printing, writing)  | a normal `for` loop                        |
| The logic needs **many steps** or several statements| a loop or a helper function                |
| **More than two `for` clauses**, or complex conditions | a loop (comprehension becomes unreadable) |
| You need `break` or `continue`                      | a loop (comprehensions cannot use them)    |
| The result is huge and used only once               | a **generator expression**                 |
| You want to count, sum or find max only             | `sum(...)`, `max(...)` with a generator    |

### Anti-pattern: using a comprehension for its side effects

```python
[print(x) for x in range(3)]       # ❌ also builds a useless list [None, None, None]

for x in range(3):                 # ✅
    print(x)
```

### Readability test

If you cannot explain the comprehension in **one sentence**, or you must read it three times, **rewrite it as a loop**.

```python
# Hard to read
r = [[y for y in x if y % 2] for x in [[a * b for b in range(5)] for a in range(5)] if sum(x) > 10]

# Clearer
rows = [[a * b for b in range(5)] for a in range(5)]
big_rows = [row for row in rows if sum(row) > 10]
r = [[y for y in row if y % 2] for row in big_rows]
```

---

## 12. Mini Programs

**1. Squares and cubes**

```python
print([(n, n ** 2, n ** 3) for n in range(1, 5)])
# [(1, 1, 1), (2, 4, 8), (3, 9, 27), (4, 16, 64)]
```

**2. Sum of digits**

```python
print(sum(int(d) for d in str(1345)))      # 13
```

**3. Count vowels**

```python
print(len([c for c in 'Education' if c.lower() in 'aeiou']))     # 5
```

**4. Celsius to Fahrenheit**

```python
celsius = [0, 20, 37, 100]
print([c * 9 / 5 + 32 for c in celsius])     # [32.0, 68.0, 98.6, 212.0]
```

**5. Names of passed students**

```python
marks = {'Asha': 82, 'Ravi': 30, 'Meena': 55}
print([n for n, m in marks.items() if m >= 35])      # ['Asha', 'Meena']
```

**6. Remove punctuation**

```python
text = 'Hello, World!'
print(''.join(c for c in text if c.isalnum() or c == ' '))     # Hello World
```

**7. Chessboard pattern**

```python
board = [['#' if (r + c) % 2 == 0 else '.' for c in range(4)] for r in range(4)]
for row in board:
    print(''.join(row))
# #.#.
# .#.#
# #.#.
# .#.#
```

**8. Sum of every row and every column**

```python
m = [[1, 2, 3], [4, 5, 6]]
print([sum(r) for r in m])              # [6, 15]
print([sum(c) for c in zip(*m)])        # [5, 7, 9]
```

**9. Flatten and sort**

```python
nested = [[3, 1], [2], [5, 4]]
print(sorted(x for sub in nested for x in sub))      # [1, 2, 3, 4, 5]
```

**10. Multiplication tables 1 to 3 (each as a row)**

```python
for row in [[f'{i}x{j}={i * j}' for j in range(1, 4)] for i in range(1, 4)]:
    print(row)
# ['1x1=1', '1x2=2', '1x3=3']
# ['2x1=2', '2x2=4', '2x3=6']
# ['3x1=3', '3x2=6', '3x3=9']
```

---

## 13. Cheat Sheet

```text
BASIC         [expr for x in iterable]
FILTER        [expr for x in iterable if condition]                 ← if at the END
CHOOSE        [A if condition else B for x in iterable]             ← if-else at the FRONT
BOTH          [A if c1 else B for x in iterable if c2]

FLAT (2 for)  [expr for a in A for b in B]            one list, outer for first
FLATTEN       [x for row in matrix for x in row]
NESTED        [[expr for j in J] for i in I]          list of lists (rows)
TRANSPOSE     [[row[i] for row in m] for i in range(len(m[0]))]    or  list(zip(*m))

OTHER BRACKETS  {x for ...} set     {k: v for ...} dict     (x for ...) generator

LOOP → COMPREHENSION:  1) what you append goes FIRST   2) add the for line   3) wrap in [ ]
REMEMBER      new list each time · variable stays inside · no break/continue · keep it readable
```

---

## 14. Common Mistakes

| Mistake                                              | Fix                                                        |
|------------------------------------------------------|------------------------------------------------------------|
| Writing "compression" instead of **comprehension**  | the word is *comprehension*                                |
| `[x for x in L if x > 0 else 0]` (SyntaxError)       | `[x if x > 0 else 0 for x in L]`                           |
| Putting a filter `if` at the front                   | filter `if` goes **at the end**                            |
| Wrong order of two `for` clauses (`for x in row for row in m`) | outer first: `for row in m for x in row`         |
| Using a flat comprehension and expecting rows        | use a nested one: `[[...] for ...]`                        |
| `[[0] * 3] * 3` for a matrix                         | `[[0] * 3 for _ in range(3)]`                              |
| Using a comprehension just to print                  | use a normal loop                                          |
| Forgetting it returns a **new** list                 | assign it: `L = [..]` or `L[:] = [..]`                     |
| Trying `break` or `continue` inside                  | not allowed; use a loop (or a condition)                   |
| Over-nesting (3+ loops, long conditions)             | split into steps or use loops                              |
| `(x for x in L)` expecting a tuple                   | it is a generator; use `tuple(...)`                        |
| Expecting the loop variable to exist afterwards      | it does not leak out of the comprehension                  |
| Lambdas in a comprehension all giving the same value | bind with a default: `lambda i=i: i`                       |
| Modifying the list you are looping over              | build a new list instead                                   |
| Building a huge list only to `sum()` it              | `sum(x for x in ...)` (generator, no brackets)             |

---

## 15. Practice Questions

Predict the output first.

```python
# 1
print([x * 3 for x in range(4)])

# 2
print([x for x in range(10) if x % 3 == 0])

# 3
print(['a' if x > 2 else 'b' for x in range(5)])

# 4
print([(x, y) for x in [1, 2] for y in 'ab'])

# 5
print([[i + j for j in range(3)] for i in range(2)])

# 6
m = [[1, 2], [3, 4]]
print([x for row in m for x in row])

# 7
print([row[0] for row in m])

# 8
print([[row[i] for row in m] for i in range(2)])

# 9
print([len(w) for w in 'the quick brown fox'.split()])

# 10
print([x for x in 'hello' if x not in 'aeiou'])

# 11
x = 10
print([x for x in range(3)], x)

# 12
print({x % 3 for x in range(7)})

# 13
print(sum(x for x in range(1, 5)))

# 14
print([[0] * 2 for _ in range(2)])

# 15
print([x if x > 0 else 0 for x in [-1, 2, -3, 4]])

# 16
print([[x for x in row if x > 1] for row in [[1, 2], [3, 1]]])

# 17
print([i * j for i in range(1, 3) for j in range(1, 4)])

# 18
print([[i * j for j in range(1, 4)] for i in range(1, 3)])
```

<details>
<summary>Answers</summary>

1. `[0, 3, 6, 9]`
2. `[0, 3, 6, 9]`
3. `['b', 'b', 'b', 'a', 'a']`
4. `[(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]`
5. `[[0, 1, 2], [1, 2, 3]]`
6. `[1, 2, 3, 4]`
7. `[1, 3]`
8. `[[1, 3], [2, 4]]` (the transpose)
9. `[3, 5, 5, 3]`
10. `['h', 'l', 'l']`
11. `[0, 1, 2] 10` (the outer `x` is unchanged)
12. `{0, 1, 2}`
13. `10`
14. `[[0, 0], [0, 0]]`
15. `[0, 2, 0, 4]`
16. `[[2], [3]]`
17. `[1, 2, 3, 2, 4, 6]` (flat: one list of 6 numbers)
18. `[[1, 2, 3], [2, 4, 6]]` (nested: two rows)

</details>

### Mini challenges

1. Create a list of the first 10 odd numbers.
2. From a list of words, build a list of those that start with a vowel, in uppercase.
3. Given a list of numbers, create a list that replaces negatives with `0` and doubles the positives.
4. Create a list of all two-digit numbers whose digit sum is 9.
5. Build a 5x5 multiplication table as a nested list and print it neatly.
6. Create a list of all `(x, y)` points with `0 <= x, y <= 3` where `x + y` is even.
7. Flatten a list of lists of strings and convert every string to uppercase.
8. Transpose a 3x4 matrix using a nested comprehension, then again using `zip`.
9. Build a 6x6 checkerboard of `0` and `1` using a nested comprehension.
10. Given a matrix, create a new matrix where each element is replaced by its square, keeping the rows.
11. Count how many elements of a matrix are greater than 5 (use a generator inside `sum`).
12. Rewrite one of your older `for`-loop programs as a comprehension, and decide which version is clearer.
