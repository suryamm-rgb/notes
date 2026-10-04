# Python Data Types

## 1. Classification of Data Types

| Category | Types | Example |
|----------|-------|---------|
| **Numeric** | `int`, `float`, `bool`, `complex` | `10`, `10.4`, `True`, `7 + 9j` |
| **Sequence** | `list`, `tuple`, `str` | `[1,2,3]`, `(3,4,5)`, `'surya'` |
| **Set** | `set` | `{2,3,4,5}` |
| **Mapping (Dictionary)** | `dict` | `{'name': 'john', 'roll': 125}` |

- **Numeric types** store **a single value**, not a collection of values.
- **Sequence, set and dictionary types** store a **collection of values**.

```python
x = 10
print(type(x))     # <class 'int'>
```

---

## 2. Overview of Each Type

### 2.1 Numeric

```python
x = 10          # int     -> integer value
y = 10.4        # float   -> floating-point value
z = True        # bool    -> True or False
c = 7 + 9j      # complex -> real + imaginary part
```

### 2.2 Sequence
A **collection of values that are index based** (each item has a position, starting from 0).

```python
l = [1, 2, 3, 4]       # list   -> mutable
k = (3, 4, 5, 6)       # tuple  -> immutable
a = 'surya'            # string -> collection of characters (immutable)
```

### 2.3 Set
- **No index**, **unordered**, only **unique (distinct)** values.
- **Duplicates are not allowed**.

```python
s = {2, 3, 4, 5, 3, 2}
print(s)        # {2, 3, 4, 5}  (duplicates removed)
```

### 2.4 Dictionary
A collection of **(key, value) pairs**.

```python
d = {'name': 'john', 'roll': 125, 'dept': 'cse'}
print(d['name'])    # john
```

> Keys of a dictionary must be written as valid values (usually strings in quotes). `{name: 'john'}` without quotes would make Python look for a variable called `name`.

### Mutable vs Immutable

| Mutable (can be changed after creation) | Immutable (cannot be changed) |
|------------------------------------------|-------------------------------|
| `list`, `set`, `dict` | `int`, `float`, `bool`, `complex`, `str`, `tuple` |

---

## 3. Numeric Types in Detail

Numeric types: **`int`, `float`, `bool`, `complex`**

---

### 3.1 `int` (Integer)

- Whole numbers **without a decimal point**.
- Supports **positive and negative** values.
- Variables are created by **declaration and initialization** together.

```python
x = 101
y = 101
z = -198
```

#### Flexible data size
Python integers have **no fixed size limit**. The memory used grows with the size of the number (unlike C or Java, where `int` is fixed at 4 bytes).

```python
import sys

x = 101
y = 12345678901234567890       # a very large number

print(x)
print(y)
print(sys.getsizeof(x))        # 28 bytes
print(sys.getsizeof(y))        # 36 bytes (bigger number, more memory)
```

> Exact byte values depend on the Python version and system (the values above are for 64-bit CPython). `sys.getsizeof()` returns the size of an object in **bytes**.

#### Different number systems

```python
print(0b1010)     # binary  -> 10
print(0o17)       # octal   -> 15
print(0xFF)       # hex     -> 255
```

#### `int` is immutable
The value of an int object **cannot be changed**. When we assign a new value, Python creates a **new object** and the variable points to it.

```python
x = 101
print(id(x))      # e.g. 140234...
x = 102
print(id(x))      # different id -> a new object was created
```

#### Summary: `int`
- Declaration and initialization together
- Positive and negative values
- Flexible data size (unlimited range)
- Memory size grows with the value
- Immutable

---

### 3.2 `float` (Floating-Point)

- Numbers **with a decimal point**.
- Used for fractional or real numbers.

```python
a = 29.56
b = 0.0774
print(type(a))     # <class 'float'>
```

- Python floats are 64-bit (double precision): `sys.getsizeof(a)` gives 24 bytes.

#### Floating-point (scientific) representation

A float can also be written in **exponent form** using `E` (or `e`), meaning "times 10 to the power".

**Example 1: large number**

```
12500 = 125 x 100
      = 125 x 10^2
      = 125E2
```

```python
a = 125E2
print(a)       # 12500.0
```

**Example 2: decimal number**

```
23.45 = 2345 / 100
      = 2345 / 10^2
      = 2345 x 10^-2
      = 2345E-2
```

```python
b = 2345E-2
print(b)       # 23.45
```

| Normal form | Scientific form |
|-------------|-----------------|
| `12500.0` | `125E2` or `1.25e4` |
| `23.45` | `2345E-2` or `2.345e1` |
| `0.00052` | `5.2e-4` |

#### Precision problem (important)

```python
print(0.1 + 0.2)     # 0.30000000000000004
```

Floats are stored in binary, so some decimals cannot be represented exactly. Use `round()` or the `decimal` module when exact values matter (for example, money).

---

### 3.3 `bool` (Boolean)

- Has only **two values**: `True` and `False`.
- Declaration and initialization:

```python
x = True
y = False
```

#### Case-sensitive: first letter must be capital

```python
z = true     # wrong -> NameError: name 'true' is not defined
z = false    # wrong -> NameError: name 'false' is not defined
```

```python
x = True
print(x)            # True
```

#### Integral values
`bool` is a subclass of `int`, so it has integer values:

| Boolean | Integer value |
|---------|---------------|
| `True` | `1` |
| `False` | `0` |

```python
x = True
y = False
print(x, y)         # True False
print(int(x))       # 1
print(int(y))       # 0
print(True + True)  # 2
```

#### Used in conditions
Boolean values come from comparison and logical operations, and are used in `if`, `while` and so on.

```python
a = 10
b = 5
print(a + b)     # 15
print(a > b)     # True
print(a == b)    # False

if a > b:
    print("a is greater")
```

#### Truthy and Falsy values

| Falsy (treated as `False`) | Truthy (treated as `True`) |
|----------------------------|----------------------------|
| `0`, `0.0`, `0j` | any non-zero number |
| `''` (empty string) | any non-empty string |
| `[]`, `()`, `{}`, `set()` | non-empty collections |
| `None` | most other objects |

```python
print(bool(0))        # False
print(bool("hi"))     # True
print(bool([]))       # False
```

---

### 3.4 `complex`

#### Mathematics refresher

A complex number has the form:

```
a + ib
```

- `a` = **real** part
- `b` = **imaginary** part
- `i` = square root of -1 (the imaginary unit)

**Example:** 5 + √-9

```
5 + √-9
= 5 + √(-1 x 9)
= 5 + √-1 x √9
= 5 + i x 3
= 5 + 3i
```

#### Complex numbers in Python

Python uses **`j`** instead of `i`, and the form is **`a + bj`** (the number comes before `j`).

```python
c = 7 + 9j          # correct
c = 7 + i9          # wrong
```

```python
c = 5 + 3j
print(c)             # (5+3j)
print(type(c))       # <class 'complex'>
print(c.real)        # 5.0
print(c.imag)        # 3.0
print(c.conjugate()) # (5-3j)
print(abs(c))        # 5.83... (magnitude)
```

Using the `complex()` function:

```python
c = complex(5, 3)    # 5+3j
```

Arithmetic:

```python
a = 2 + 3j
b = 1 + 2j
print(a + b)     # (3+5j)
print(a * b)     # (-4+7j)
```

> Complex numbers are used in engineering, signal processing and scientific computing.

---

## 4. Sequence Types (Quick Overview)

| Feature | `list` | `tuple` | `str` |
|---------|--------|---------|-------|
| Brackets | `[ ]` | `( )` | `' '` or `" "` |
| Mutable | Yes | No | No |
| Ordered / indexed | Yes | Yes | Yes |
| Duplicates | Allowed | Allowed | Allowed |
| Example | `[1,2,3]` | `(1,2,3)` | `'abc'` |

```python
l = [10, 20, 30]
print(l[0])        # 10   (index starts at 0)
print(l[-1])       # 30   (negative index = from the end)
l[1] = 99          # allowed: list is mutable
print(l)           # [10, 99, 30]

k = (10, 20, 30)
# k[1] = 99        # TypeError: tuple is immutable

s = 'surya'
print(s[0])        # s
print(s[1:4])      # ury  (slicing)

single = (5,)      # a one-element tuple needs a trailing comma
```

---

## 5. Set Type (Quick Overview)

```python
s = {2, 3, 4, 5}
s.add(6)           # add an item
s.remove(2)        # remove an item
print(s)           # {3, 4, 5, 6}

empty = set()      # {} creates an empty dict, not a set
```

- Unordered, so there is **no indexing** (`s[0]` gives an error).
- Elements must be **unique** and **immutable** (numbers, strings, tuples).
- Supports set operations: union `|`, intersection `&`, difference `-`.

---

## 6. Dictionary Type (Quick Overview)

```python
d = {'name': 'john', 'roll': 125, 'dept': 'cse'}

print(d['roll'])          # 125
d['dept'] = 'ece'         # update
d['city'] = 'delhi'       # add new pair
print(d.keys())           # dict_keys([...])
print(d.values())         # dict_values([...])
```

- Stores data as **key : value** pairs.
- **Keys must be unique and immutable** (str, int, tuple).
- Values can be of any type and can repeat.
- Since Python 3.7, dictionaries remember insertion order.

---

## 7. Summary Table of All Data Types

| Type | Example | Ordered | Mutable | Duplicates |
|------|---------|---------|---------|------------|
| `int` | `10` | n/a | No | n/a |
| `float` | `10.4` | n/a | No | n/a |
| `bool` | `True` | n/a | No | n/a |
| `complex` | `7+9j` | n/a | No | n/a |
| `str` | `'abc'` | Yes | No | Yes |
| `list` | `[1,2,3]` | Yes | Yes | Yes |
| `tuple` | `(1,2,3)` | Yes | No | Yes |
| `set` | `{1,2,3}` | No | Yes | No |
| `dict` | `{'a':1}` | Yes (3.7+) | Yes | Keys: No, Values: Yes |

---

## 8. Quick Revision Points

- Python data types: **numeric, sequence, set, dictionary**.
- Numeric types hold a **single value**: `int`, `float`, `bool`, `complex`.
- `int` has unlimited size, supports positive and negative values, and is immutable.
- `float` has a decimal point and can be written in scientific form (`125E2`, `2345E-2`).
- `bool` is `True` or `False` (capital first letter), with integer values 1 and 0, and is used in conditions.
- `complex` is written as `a + bj` (use `j`, not `i`), with `.real` and `.imag` parts.
- `list` is mutable, `tuple` and `str` are immutable; all three are indexed.
- `set` is unordered and unique with no index; `dict` stores key:value pairs.
- Use `type()` for the type, `id()` for identity and `sys.getsizeof()` for memory size.
