# Base Conversion and Type Conversion in Python

## Part 1: Number Systems and Base Conversion

### 1.1 Number Systems

| Number System | Base | Digits | Prefix | Example (10) |
|---------------|:----:|--------|:------:|:------------:|
| Decimal | 10 | 0-9 | none | `10` |
| Binary | 2 | 0, 1 | `0b` | `0b1010` |
| Octal | 8 | 0-7 | `0o` | `0o12` |
| Hexadecimal | 16 | 0-9, A-F | `0x` | `0xA` |

- **Base** = the number of digits available in that number system.
- Humans normally use **decimal**; computers work in **binary**.

---

### 1.2 Base Conversion Functions (Built-in)

| Function | Converts | Returns |
|----------|----------|---------|
| `bin(int)` | integer to **binary** | string, prefix `0b` |
| `oct(int)` | integer to **octal** | string, prefix `0o` |
| `hex(int)` | integer to **hexadecimal** | string, prefix `0x` |
| `int(str, base)` | string of any base to **decimal** | integer |

> **Important:** `bin()`, `oct()` and `hex()` return a **string**, not a number.

```python
print(bin(10))     # 0b1010
print(oct(10))     # 0o12
print(hex(10))     # 0xa
```

> **Correction:** `hex(10)` gives `'0xa'` in **lowercase**, not `0xA`. Python's output for hex digits is always lowercase.

#### Example: decimal number to other bases

```python
a = 10                  # decimal number

binary = bin(a)
octal = oct(a)
hexa = hex(a)

print(binary)           # 0b1010
print(octal)            # 0o12
print(hexa)             # 0xa

print(type(binary))     # <class 'str'>  -> result is a string
```

#### Removing the prefix

```python
a = 10
print(bin(a)[2:])       # 1010   (slice off '0b')
print(oct(a)[2:])       # 12
print(hex(a)[2:])       # a

# using format()
print(format(a, 'b'))   # 1010
print(format(a, 'o'))   # 12
print(format(a, 'x'))   # a
print(format(a, 'X'))   # A  (uppercase hex)
```

#### Negative numbers

```python
print(bin(-10))     # -0b1010
```

#### Works only with integers

```python
bin(10.5)           # TypeError: 'float' object cannot be interpreted as an integer
```

---

### 1.3 Converting a String to Decimal: `int(str, base)`

`int()` can convert a **string** written in any base (2 to 36) into a **decimal integer**. The second argument is the **base** of the string.

```python
print(int('1010', 2))        # 10   (binary string)
print(int('12', 8))          # 10   (octal string)
print(int('A', 16))          # 10   (hex string)

# the prefix is also accepted if it matches the base
print(int('0b1010', 2))      # 10
print(int('0o12', 8))        # 10
print(int('0xA', 16))        # 10
```

- If no base is given, the default base is **10**.
- If the string has a prefix and you pass **base 0**, Python detects the base automatically.

```python
print(int('0b1010', 0))      # 10
print(int('0xff', 0))        # 255
```

#### Round trip

```python
n = 25
b = bin(n)                   # '0b11001'
print(int(b, 2))             # 25  -> back to decimal
```

---

## Part 2: Type Conversion

### 2.1 What is Type Conversion?

Converting a value from **one data type to another**.

| Kind | Meaning | Example |
|------|---------|---------|
| **Implicit** | Python converts automatically | `10 + 2.5` gives `12.5` (int becomes float) |
| **Explicit** (type casting) | The programmer converts using a function | `int(16.59)` |

### 2.2 Conversion Functions

| Function | Converts to | Data type |
|----------|-------------|-----------|
| `int()` | Integer | `int` |
| `float()` | Floating-point | `float` |
| `bool()` | Boolean | `bool` |
| `complex()` | Complex | `complex` |
| `str()` | String | `str` |

> **Warning:** Never name a variable `str`, `int`, `float`, etc. (for example `str = 'Alexa'`). It hides the built-in function, and later calls like `str(10)` or `int(str)` will fail.

---

### 2.3 `int()`: convert to integer

| From | Example | Result | Note |
|------|---------|--------|------|
| float | `int(16.59)` | `16` | decimal part is **truncated** (not rounded) |
| bool | `int(True)` | `1` | `True` = 1, `False` = 0 |
| str | `int('125')` | `125` | string must contain a valid integer |
| str (other base) | `int('0b1010', 2)` | `10` | give the base |
| str (other base) | `int('0xA', 16)` | `10` | give the base |
| complex | `int(3+4j)` | **TypeError** | not allowed |

```python
f = 16.48
x = int(f)
print(x)                # 16

print(int(-16.59))      # -16  (truncates towards zero)

b = True
x = int(b)
print(x)                # 1

print(int('125'))       # 125
print(int(' 125 '))     # 125  (spaces around are ignored)
```

#### Invalid conversions

```python
int('Alexa')       # ValueError: invalid literal for int()
int('12.5')        # ValueError: a float string cannot go directly to int
int(3+4j)          # TypeError: complex cannot be converted to int
```

To convert `'12.5'` to int, convert in two steps: `int(float('12.5'))` gives `12`.

---

### 2.4 `float()`: convert to float

| From | Example | Result |
|------|---------|--------|
| int | `float(125)` | `125.0` |
| bool | `float(True)` | `1.0` |
| str | `float('12.45')` | `12.45` |
| complex | `float(3+4j)` | **TypeError** |

```python
print(float(125))         # 125.0
print(float(True))        # 1.0
print(float(False))       # 0.0
print(float('12.45'))     # 12.45
print(float('10'))        # 10.0
print(float('1e3'))       # 1000.0
```

```python
float('abc')      # ValueError
```

---

### 2.5 `bool()`: convert to boolean

Rule: **zero and empty values give `False`; everything else gives `True`.**

| From | Example | Result |
|------|---------|--------|
| int | `bool(10)` | `True` |
| int | `bool(0)` | `False` |
| float | `bool(-1.23)` | `True` (negative is non-zero) |
| float | `bool(0.0)` | `False` |
| bool | `bool(True)` | `True` |
| complex | `bool(3+4j)` | `True` |
| complex | `bool(0j)` | `False` |
| str | `bool("False")` | **`True`** (non-empty string) |
| str | `bool("")` | `False` (empty string) |

```python
print(bool(10))         # True
print(bool(-1.23))      # True
print(bool(True))       # True
print(bool(3+4j))       # True
print(bool("False"))    # True  <- surprising! the string is not empty
print(bool(""))         # False
print(bool(0))          # False
```

> **Remember:** `bool("False")` is `True` because any **non-empty string** is truthy. The text inside does not matter.

#### Falsy values in Python
`0`, `0.0`, `0j`, `False`, `None`, `''`, `[]`, `()`, `{}`, `set()`

---

### 2.6 `complex()`: convert to complex

| From | Example | Result |
|------|---------|--------|
| int | `complex(10)` | `(10+0j)` |
| float | `complex(-12.5)` | `(-12.5+0j)` |
| bool | `complex(True)` | `(1+0j)` |
| bool | `complex(False)` | `0j` |
| str | `complex('3+4j')` | `(3+4j)` |
| two numbers | `complex(3, 4)` | `(3+4j)` |

```python
print(complex(10))        # (10+0j)
print(complex(-12.5))     # (-12.5+0j)
print(complex(True))      # (1+0j)
print(complex(False))     # 0j
print(complex('3+4j'))    # (3+4j)
print(complex(3, 4))      # (3+4j)  -> real, imaginary
```

- The imaginary part is `0` when only one value is given.
- A string must be written **without spaces** around `+`: `complex('3 + 4j')` raises `ValueError`.
- Use `j`, not `i`: `3+4i` is invalid.

---

### 2.7 `str()`: convert to string

Any value can be converted into a string.

| From | Example | Result |
|------|---------|--------|
| int | `str(10)` | `'10'` |
| float | `str(-12E-3)` | `'-0.012'` |
| bool | `str(False)` | `'False'` |
| complex | `str(3+4j)` | `'(3+4j)'` |
| str | `str('Alexa')` | `'Alexa'` (unchanged) |

```python
print(str(10))          # 10
print(str(-12E-3))      # -0.012
print(str(False))       # False
print(str(3+4j))        # (3+4j)
print(str('Alexa'))     # Alexa

print(type(str(10)))    # <class 'str'>
```

> **Correction:** `-12E-3` means -12 x 10^-3 = **-0.012** (not -0.0012).

Why `str()` matters: you cannot join a string and a number directly.

```python
age = 25
print("Age: " + age)         # TypeError
print("Age: " + str(age))    # Age: 25
```

---

### 2.8 Conversion Summary Matrix

| From \ To | `int()` | `float()` | `bool()` | `complex()` | `str()` |
|-----------|:-------:|:---------:|:--------:|:-----------:|:-------:|
| **int** | n/a | Yes | Yes | Yes | Yes |
| **float** | Yes (truncates) | n/a | Yes | Yes | Yes |
| **bool** | Yes | Yes | n/a | Yes | Yes |
| **complex** | **No** | **No** | Yes | n/a | Yes |
| **str** | Only if valid integer text | Only if valid number text | Yes (non-empty = True) | Only if valid complex text | n/a |

---

## Part 3: Quick Revision Points

- **Bases:** decimal 10, binary 2, octal 8, hexadecimal 16.
- `bin()`, `oct()`, `hex()` convert an **integer** to a **string** with the prefix `0b`, `0o`, `0x`.
- `bin(10)` is `'0b1010'`, `oct(10)` is `'0o12'`, `hex(10)` is `'0xa'` (lowercase).
- `int(string, base)` converts a string in any base back to a decimal integer: `int('1010', 2)` is `10`.
- Type conversion functions: `int()`, `float()`, `bool()`, `complex()`, `str()`.
- `int(16.59)` is `16` (truncates); `int(True)` is `1`.
- `int('Alexa')` and `int('12.5')` give `ValueError`.
- `complex` cannot be converted to `int` or `float` (TypeError).
- `bool()` gives `False` only for zero or empty values; `bool("False")` is `True`.
- `complex(10)` is `(10+0j)`; `complex(False)` is `0j`.
- `str()` works on everything; `str(3+4j)` gives `'(3+4j)'`.
- Do not use built-in names like `str` as variable names.
