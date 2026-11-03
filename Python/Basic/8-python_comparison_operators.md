# Python Comparison, Short-Circuit, Bitwise, Chaining & Identity Operators
### Beginner → Advanced Notes

---

## Table of Contents
1. [Relational (Comparison) Operators](#1-relational-comparison-operators)
2. [String Comparison & Lexicographic Order](#2-string-comparison--lexicographic-order)
3. [Relational Operators on All Data Types](#3-relational-operators-on-all-data-types)
4. [Short-Circuit Evaluation (`and`, `or`)](#4-short-circuit-evaluation-and-or)
5. [Non-Boolean Conditions](#5-non-boolean-conditions)
6. [Bitwise Operators](#6-bitwise-operators)
7. [Chaining Comparison](#7-chaining-comparison)
8. [`is` and `is not`](#8-is-and-is-not)
9. [Quick Cheat Sheet](#9-quick-cheat-sheet)
10. [Practice Questions](#10-practice-questions)

---

## 1. Relational (Comparison) Operators

### Beginner level

Relational operators **compare two values** and always return a Boolean (`True` or `False`).

| Operator | Meaning                  | Example    | Result  |
|----------|--------------------------|------------|---------|
| `<`      | less than                | `3 < 5`    | `True`  |
| `<=`     | less than or equal to    | `5 <= 5`   | `True`  |
| `>`      | greater than             | `3 > 5`    | `False` |
| `>=`     | greater than or equal to | `4 >= 5`   | `False` |
| `==`     | equal to                 | `5 == 5`   | `True`  |
| `!=`     | not equal to             | `5 != 3`   | `True`  |

```python
a = 10
b = 20

print(a < b)    # True
print(a == b)   # False
print(a != b)   # True
```

> ⚠️ **Common mistake:** `=` is **assignment**, `==` is **comparison**.

---

## 2. String Comparison & Lexicographic Order

### Beginner level

Strings are compared in **lexicographic order**, also called **dictionary order**. That means they are arranged the way words appear in a dictionary.

```python
print('cat' < 'catch')   # True  ('cat' is a prefix of 'catch', so it comes first)
print('apple' < 'banana')  # True  ('a' comes before 'b')
```

### How it works (step by step)

1. Compare the **first characters** of both strings.
2. If they are different, the comparison is decided right there.
3. If they are the same, move to the **next characters**.
4. If one string ends first (it is a prefix), the **shorter string is smaller**.

### Intermediate level — Unicode / ASCII values

Python does not compare "alphabet positions". It compares each character's **Unicode code point** (same as ASCII for English letters). Check it with `ord()`:

```python
print(ord('0'))   # 48
print(ord('A'))   # 65
print(ord('a'))   # 97
```

So the order of characters is:

```
digits  <  UPPERCASE  <  lowercase

0 < 1 < 2 < 3 < 4 < ... < 9 < A < B < C < D < ... < Z < a < b < c < d < ... < z
```

| Comparison        | Result  | Reason                                    |
|-------------------|---------|-------------------------------------------|
| `'cat' < 'catch'` | `True`  | prefix is smaller                         |
| `'data' > 'Data'` | `True`  | `'d'` (100) > `'D'` (68)                  |
| `'2nd' < 'Byte'`  | `True`  | `'2'` (50) < `'B'` (66)                   |
| `'A' < 'a'`       | `True`  | 65 < 97                                   |

### Worked examples

```python
s1 = 'software'
s2 = 'hardware'
print(s1 > s2)    # True   -> 's' (115) > 'h' (104)

s3 = 'python'
s4 = 'pycharm'
print(s3 < s4)    # False  -> see below
```

**Why is `s3 < s4` `False`?**
`'p' == 'p'` and `'y' == 'y'`, so Python moves on to the third letter: `'t'` (116) vs `'c'` (99). Since `'t' > 'c'`, `'python' > 'pycharm'`.

> ✅ So `s3 > s4` is `True`, and `s3 < s4` is `False`.

### Advanced level — case-insensitive comparison

Because uppercase and lowercase differ, normalise before comparing:

```python
print('Apple' == 'apple')                  # False
print('Apple'.lower() == 'apple'.lower())  # True
print('Apple'.casefold() == 'apple')       # True  (best for international text)
```

Sorting is also lexicographic:

```python
words = ['banana', 'Apple', 'cherry']
print(sorted(words))                  # ['Apple', 'banana', 'cherry']  (uppercase first)
print(sorted(words, key=str.lower))   # ['Apple', 'banana', 'cherry']  (case-insensitive)
```

---

## 3. Relational Operators on All Data Types

### Integer & Float

Compared by numeric value. Mixed int/float comparison works.

```python
print(10 < 20)      # True
print(5 == 5.0)     # True
print(3.14 > 3)     # True
```

> ⚠️ Floating point can be inexact: `0.1 + 0.2 == 0.3` → `False`. Use `math.isclose(0.1 + 0.2, 0.3)`.

### String

Lexicographic comparison (see section 2). You **cannot** compare a string with a number using `<` or `>`:

```python
print('10' < 20)   # TypeError: '<' not supported between 'str' and 'int'
print('10' == 10)  # False (no error, just not equal)
```

### Boolean

`bool` is a subclass of `int`:

```
True  -> 1
False -> 0
```

```python
print(True > False)     # True   (1 > 0)
print(True == 1)        # True
print(True + True)      # 2
print(True - False)     # 1      <- an int, NOT True
print(True - True)      # 0      <- an int, NOT True
```

> ✅ Arithmetic on booleans gives **integers**. `True - False` is `1` and `True - True` is `0`. (`1` is truthy and `0` is falsy when used in a condition.)

### Complex

Complex numbers have **no natural ordering**. Only `==` and `!=` work.

```python
a = 3 + 4j
b = 4 + 3j

print(a == b)   # False
print(a != b)   # True
print(a < b)    # TypeError: '<' not supported between 'complex' and 'complex'
```

To order complex numbers, compare something numeric about them, e.g. magnitude:

```python
print(abs(a) < abs(b))   # False (both are 5.0)
print(abs(a) == abs(b))  # True
```

### Summary table

| Data type | `==` `!=` | `<` `<=` `>` `>=` | Basis of comparison          |
|-----------|-----------|--------------------|------------------------------|
| `int`     | ✅        | ✅                 | numeric value                |
| `float`   | ✅        | ✅                 | numeric value                |
| `str`     | ✅        | ✅                 | lexicographic (Unicode)      |
| `bool`    | ✅        | ✅                 | `True`=1, `False`=0          |
| `complex` | ✅        | ❌ `TypeError`     | equality only                |

### Advanced — lists and tuples are also lexicographic

```python
print([1, 2, 3] < [1, 2, 4])   # True
print((1, 5) < (2, 0))         # True
print([1, 2] < [1, 2, 0])      # True  (prefix is smaller)
```

---

## 4. Short-Circuit Evaluation (`and`, `or`)

### Beginner level

Logical operators combine conditions.

**Truth table for `and`** — True only if **both** are True

| cond1 | cond2 | cond1 `and` cond2 |
|-------|-------|-------------------|
| T     | T     | **T**             |
| T     | F     | F                 |
| F     | T     | F                 |
| F     | F     | F                 |

**Truth table for `or`** — True if **at least one** is True

| cond1 | cond2 | cond1 `or` cond2 |
|-------|-------|------------------|
| T     | T     | T                |
| T     | F     | T                |
| F     | T     | T                |
| F     | F     | **F**            |

```python
a = 10
b = 5
c = 3

print(a > b and a > c)   # True  and True  -> True
print(a > b and a < c)   # True  and False -> False
print(a > b or  a > c)   # True  or  ...   -> True
print(a < b or  a > c)   # False or  True  -> True
```

### Intermediate level — What is "short circuit"?

Python **stops evaluating as soon as the result is already known.**

| Expression      | Rule                                                                     |
|-----------------|--------------------------------------------------------------------------|
| `cond1 and cond2` | If `cond1` is **False**, the answer is False → `cond2` is **skipped**  |
| `cond1 or cond2`  | If `cond1` is **True**, the answer is True → `cond2` is **skipped**    |

```python
def check(name):
    print("checking", name)
    return True

print(False and check("A"))   # check() is NEVER called -> False
print(True  or  check("B"))   # check() is NEVER called -> True
print(True  and check("C"))   # prints "checking C" -> True
```

### Why does it matter? (Practical uses)

**1. Avoid errors**

```python
x = 0
if x != 0 and 10 / x > 2:   # safe: division skipped when x is 0
    print("big")
```

**2. Avoid expensive work**

```python
if user_is_logged_in and expensive_database_check():
    ...
```

**3. Check existence before use**

```python
if my_list and my_list[0] == 'a':   # no IndexError on empty list
    ...
```

---

## 5. Non-Boolean Conditions

### Beginner level — Truthy and Falsy

Any value can be used in a condition.

```
Falsy  (counts as False):  0, 0.0, 0j, '' (empty string), [], (), {}, set(), None, False
Truthy (counts as True):   everything else (1, -1, 10, -15, 7, 'hi', [0], ...)
```

> ✅ Any **non-zero** number is True, including negatives.

```python
if 5:
    print("runs")      # runs

if 0:
    print("never")     # does not run
```

### Intermediate level — `and` / `or` return a VALUE, not just True/False

Python's `and` and `or` return **one of the operands**.

**`and` rule:** if the first is falsy → return the **first**; otherwise return the **second**.

```python
print(5 and 10)   # T and T -> 10   (returns the second)
print(5 and 0)    # T and F -> 0    (returns the second)
print(0 and 10)   # F and ? -> 0    (returns the first, short-circuit)
print('' and 10)  # ''
```

**`or` rule:** if the first is truthy → return the **first**; otherwise return the **second**.

```python
print(5 or 10)    # 5     (first is truthy, short-circuit)
print(0 or 10)    # 10
print(0 or 0)     # 0
print('' or 'default')   # 'default'
```

| Expression | Returns | Why                                    |
|------------|---------|----------------------------------------|
| `5 and 10` | `10`    | first truthy → give second             |
| `5 and 0`  | `0`     | first truthy → give second             |
| `0 and 10` | `0`     | first falsy → stop, give first         |
| `5 or 10`  | `5`     | first truthy → stop, give first        |
| `0 or 10`  | `10`    | first falsy → give second              |

### Advanced — common idioms

```python
# default value
name = user_input or "Guest"

# chain: first truthy value
result = a or b or c        # first non-empty one

# inline guard
value = obj and obj.attribute   # None/0/'' stays as is

# `not` always returns a real bool
print(not 5)     # False
print(not 0)     # True
```

---

## 6. Bitwise Operators

### Beginner level — Binary numbers

Computers store numbers in binary (0s and 1s).

| Decimal | Binary |
|---------|--------|
| 0       | 0      |
| 1       | 1      |
| 2       | 10     |
| 3       | 11     |
| 4       | 100    |
| 8       | 1000   |
| 10      | 1010   |

Convert a number to a binary string with `format()` (or `bin()`):

```python
a = 10
print(format(a, 'b'))    # '1010'

a = 14
print(format(a, 'b'))    # '1110'

print(format(25, 'b'))   # '11001'
print(format(8, 'b'))    # '1000'

print(bin(10))           # '0b1010'  (with 0b prefix)
print(int('1010', 2))    # 10        (binary -> decimal)
```

### Intermediate level — The 6 bitwise operators

| Operator | Name        | Rule (bit by bit)                           |
|----------|-------------|---------------------------------------------|
| `&`      | AND         | 1 only if **both** bits are 1               |
| `\|`     | OR          | 1 if **at least one** bit is 1              |
| `^`      | XOR         | 1 if the bits are **different**             |
| `~`      | Complement  | flips every bit (`~x = -(x + 1)`)           |
| `<<`     | Left shift  | shifts bits left (multiplies by 2 per shift)|
| `>>`     | Right shift | shifts bits right (divides by 2 per shift)  |

### Worked example: `a = 10`, `b = 13`

```
a  = 10 ->  1010
b  = 13 ->  1101
            ----
a & b       1000  = 8     (AND)
a | b       1111  = 15    (OR)
a ^ b       0111  = 7     (XOR)
```

```python
a = 10
b = 13

print(a & b)    # 8
print(a | b)    # 15
print(a ^ b)    # 7
print(~a)       # -11
print(a << 1)   # 20   (10 * 2)
print(a << 2)   # 40   (10 * 4)
print(a >> 1)   # 5    (10 // 2)
print(a >> 2)   # 2    (10 // 4)
```

### Truth tables

| A | B | A & B | A \| B | A ^ B |
|---|---|-------|--------|-------|
| 0 | 0 | 0     | 0      | 0     |
| 0 | 1 | 0     | 1      | 1     |
| 1 | 0 | 0     | 1      | 1     |
| 1 | 1 | 1     | 1      | 0     |

### Advanced level — Useful tricks

```python
# Check odd / even
print(7 & 1)    # 1 -> odd
print(8 & 1)    # 0 -> even

# Check if n is a power of two
n = 16
print(n > 0 and (n & (n - 1)) == 0)   # True

# Swap two numbers without a temp variable
a, b = 5, 9
a ^= b
b ^= a
a ^= b
print(a, b)     # 9 5

# Set, clear, toggle, test the k-th bit
n, k = 10, 2
n | (1 << k)        # set bit k
n & ~(1 << k)       # clear bit k
n ^ (1 << k)        # toggle bit k
(n >> k) & 1        # test bit k
```

> ⚠️ **Don't confuse:** `&` / `|` (bitwise, work on bits, **no short-circuit**) vs `and` / `or` (logical, short-circuit).

```python
print(6 & 3)     # 2   (bitwise)
print(6 and 3)   # 3   (logical)
```

---

## 7. Chaining Comparison

### Beginner level

Normally, to check that a number is between two others you write two conditions:

```python
a = 10
b = 5
c = 3

if a > b and b > c:
    print("a is greatest, c is smallest")
```

Python lets you **chain** them, just like in maths:

```python
if a > b > c:
    print("a is greatest, c is smallest")
```

`a > b > c` means exactly `a > b and b > c`.

### More examples

```python
x = 15
print(10 < x < 20)      # True   (range check)
print(1 < 2 < 3 < 4)    # True

a, b, c = 5, 5, 8
print(a == b < c)       # True   same as  a == b and b < c

print(3 < 2 < 10)       # False  (3 < 2 is False)
```

### Intermediate level — how it works

- `a < b < c` is evaluated as `(a < b) and (b < c)`.
- The middle value `b` is evaluated **only once** (useful when it is a function call).
- Chaining **also short-circuits**: if the first comparison is `False`, the rest are skipped.

```python
def f():
    print("f called")
    return 5

print(1 < f() < 10)    # "f called" printed ONCE -> True
```

### Advanced level — gotchas

You can mix **any** comparison operators, even odd-looking ones:

```python
print(1 < 3 > 2)        # True  -> (1 < 3) and (3 > 2)
print(1 == 1 != 2)      # True
print(5 > 3 == True)    # False -> (5 > 3) and (3 == True)  -> 3 == True is False
```

> ⚠️ `a < b > c` does **not** compare `a` with `c`. Only **neighbours** are compared.

> ⚠️ `(5 > 3) == True` (with brackets) is a completely different expression from `5 > 3 == True`.

---

## 8. `is` and `is not`

### Beginner level

| Operator | Question it asks                                   |
|----------|----------------------------------------------------|
| `==`     | Do they have the **same value**?                   |
| `is`     | Are they the **same object** (same memory location)? |
| `is not` | Are they **different objects**?                    |

```python
a = 10
b = a            # b points to the SAME object as a

print(b is a)    # True
print(b is not a)  # False
```

### Intermediate level — Using `id()`

`id()` shows the memory identity of an object. `is` compares these ids.

```python
x = 25
y = 25

print(id(x), id(y))   # same number
print(y is x)         # True
```

**Why is it `True`?**
CPython **caches small integers** (from **-5 to 256**). Both `x` and `y` point to the same cached object, so the same memory is used.

```python
a = 'hello'
b = 'hello'
print(a is b)         # True  (short identifier-like strings are "interned")
```

### `==` vs `is` — the key difference

```python
list1 = [1, 2, 3]
list2 = [1, 2, 3]
list3 = list1

print(list1 == list2)   # True   same value
print(list1 is list2)   # False  different objects
print(list1 is list3)   # True   same object
```

### Advanced level — don't rely on `is` for values

Whether two equal numbers/strings are the *same object* is an **implementation detail** of CPython, not a language guarantee.

```python
a = 1000
b = 1000
print(a == b)    # True
print(a is b)    # may be False (outside the small-int cache; depends on context)

s1 = 'hello world!'
s2 = 'hello world!'
print(s1 is s2)  # may be False (not interned)
```

**Best practices**

| Use `is` / `is not` for                  | Use `==` / `!=` for        |
|------------------------------------------|----------------------------|
| `None`, `True`, `False` checks           | numbers, strings, lists    |
| checking two names refer to same object  | comparing values           |

```python
if value is None:       # ✅ correct
    ...
if value == None:       # ❌ works, but not recommended
    ...
```

Python (3.8+) even warns for `x is 5` or `x is 'abc'` (`SyntaxWarning: "is" with a literal`).

---

## 9. Quick Cheat Sheet

```text
Comparison     <  <=  >  >=  ==  !=
Strings        compared by Unicode: digits < A-Z < a-z; prefix is smaller
Complex        only == and != allowed
Bool           True = 1, False = 0  (True - False = 1, True - True = 0)
Short circuit  False and X -> skip X     True or X -> skip X
and            returns first falsy, else last value
or             returns first truthy, else last value
Falsy values   0, 0.0, 0j, '', [], (), {}, set(), None, False
Bitwise        &  |  ^  ~  <<  >>
Chaining       a < b < c   ==   a < b and b < c
is             same object (identity)     ==  same value
```

---

## 10. Practice Questions

Try to predict the output before running each one.

```python
# 1
print('Zebra' < 'apple')

# 2
print('10' < '9')

# 3
print(True + True + True)

# 4
print(0 or 'Python' and 'Java')

# 5
print(12 & 10, 12 | 10, 12 ^ 10)

# 6
print(1 < 2 < 3 > 2)

# 7
a = [1, 2]
b = [1, 2]
print(a == b, a is b)

# 8
print(5 and 0 or 7)
```

<details>
<summary>Answers</summary>

1. `True` — uppercase `'Z'` (90) < lowercase `'a'` (97)
2. `True` — string comparison: `'1'` < `'9'`
3. `3`
4. `'Java'` — `'Python' and 'Java'` → `'Java'`; `0 or 'Java'` → `'Java'`
5. `8 14 6` — `1100 & 1010 = 1000`, `| = 1110`, `^ = 0110`
6. `True` — `(1<2) and (2<3) and (3>2)`
7. `True False`
8. `7` — `5 and 0` → `0`; `0 or 7` → `7`

</details>
