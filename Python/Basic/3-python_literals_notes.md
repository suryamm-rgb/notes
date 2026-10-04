# Python Literals and Constants

## 1. What are Literals?

A **literal** is a **fixed value written directly in the program**. It is also called a **constant value**, because the value itself never changes.

```python
a = 10                      # 10 is a literal
b = 5                       # 5 is a literal
c = a + b                   # a, b, c are variables (not literals)
d = input('Enter name: ')   # 'Enter name: ' is a string literal
```

- The direct values used in the program (`10`, `5`, `'Enter name: '`) are called **literals**.
- Variables (`a`, `b`, `c`, `d`) are **names** that refer to data. They are not literals.

### Types of Literals

| Literal Type | Example |
|--------------|---------|
| Integer | `201`, `-15`, `0b1010` |
| Float | `9.75`, `12E2` |
| Boolean | `True`, `False` |
| Complex | `4 + 5j` |
| String | `"Alexa"`, `'Alexa'` |
| Special | `None` |
| Collection | `[1, 2]`, `(1, 2)`, `{1, 2}`, `{'a': 1}` |

### Literal vs Variable vs Constant

| Term | Meaning | Example |
|------|---------|---------|
| Literal | A fixed value written directly in code | `10`, `'hello'` |
| Variable | A name that refers to data; the value can change | `age = 10` |
| Constant | A value that should not change | `PI = 3.14` |

> **Note:** Python has **no built-in constant keyword** (unlike `const` in C++/JavaScript or `final` in Java). By convention, constants are written in **UPPERCASE** names (`PI = 3.14159`, `MAX_SIZE = 100`), and programmers agree not to change them.

---

## 2. Integer Literals

Whole numbers, positive or negative, with no decimal point.

```python
a = 201
b = 13524198
c = 13_342_2412     # underscore used for readability
```

### Underscore rules
- The underscore `_` can be used **between digits** to make long numbers readable.
- Python ignores the underscores.
- It **cannot** come at the **start** or **end** of a number, or be doubled (`1__0`).

```python
c = 13_342_2412     # valid  -> 133422412
d = 234_            # wrong  -> SyntaxError (underscore at the end)
e = _342            # wrong  -> treated as a variable name, gives NameError
```

```python
print(1_000_000)    # 1000000
```

### Integer literal formats (number systems)

An integer literal can be written in **four forms**: decimal, binary, octal and hexadecimal.

| Number System | Base | Digits Allowed | Prefix | Example (for 10) |
|---------------|------|----------------|--------|------------------|
| **Decimal** | 10 | `0` to `9` | none | `10` |
| **Binary** | 2 | `0, 1` | `0b` or `0B` | `0b1010` |
| **Octal** | 8 | `0` to `7` | `0o` or `0O` | `0o12` |
| **Hexadecimal** | 16 | `0-9` and `A-F` | `0x` or `0X` | `0xA` |

```python
a = 10        # decimal
b = 0b1010    # binary
c = 0o12      # octal
d = 0xA       # hexadecimal

print(a, b, c, d)    # 10 10 10 10   (Python always prints in decimal)
```

All four literals above represent the **same value**, 10.

#### Digits in each system

- **Decimal** = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
- **Binary** = {0, 1}
- **Octal** = {0, 1, 2, 3, 4, 5, 6, 7}
- **Hexadecimal** = {0-9, A, B, C, D, E, F}, where A=10, B=11, C=12, D=13, E=14, F=15

#### Conversion table (0 to 17)

| Decimal | Binary | Octal | Hexadecimal |
|:-------:|:------:|:-----:|:-----------:|
| 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 |
| 2 | 10 | 2 | 2 |
| 3 | 11 | 3 | 3 |
| 4 | 100 | 4 | 4 |
| 5 | 101 | 5 | 5 |
| 6 | 110 | 6 | 6 |
| 7 | 111 | 7 | 7 |
| 8 | 1000 | 10 | 8 |
| 9 | 1001 | 11 | 9 |
| 10 | 1010 | 12 | A |
| 11 | 1011 | 13 | B |
| 12 | 1100 | 14 | C |
| 13 | 1101 | 15 | D |
| 14 | 1110 | 16 | E |
| 15 | 1111 | 17 | F |
| 16 | 10000 | 20 | 10 |
| 17 | 10001 | 21 | 11 |

> **Correction:** In the original notes, `17 -> 1001` was written for binary. The correct binary of 17 is **10001** (`1001` is 9). Octal of 17 is **21**, which was correct.

#### How to convert decimal to another base
Divide repeatedly by the base and read the remainders **from bottom to top**.

**Example: 17 to binary**

```
17 / 2 = 8  remainder 1
 8 / 2 = 4  remainder 0
 4 / 2 = 2  remainder 0
 2 / 2 = 1  remainder 0
 1 / 2 = 0  remainder 1
```
Reading from bottom to top: **10001**

**Example: 17 to octal** (divide by 8): 17 / 8 = 2 remainder 1, then 2 / 8 = 0 remainder 2, so **21**

**Example: 17 to hex** (divide by 16): 17 / 16 = 1 remainder 1, then 1 / 16 = 0 remainder 1, so **11**

#### Converting using built-in functions

```python
x = 10
print(bin(x))     # 0b1010
print(oct(x))     # 0o12
print(hex(x))     # 0xa

print(int('1010', 2))    # 10  (binary string to decimal)
print(int('12', 8))      # 10
print(int('A', 16))      # 10
```

#### Invalid examples

```python
a = 0b102     # wrong: binary allows only 0 and 1
b = 0o18      # wrong: octal allows only 0 to 7
c = 0xG5      # wrong: G is not a hex digit
d = 012       # wrong in Python 3: use 0o12
```

---

## 3. Float Literals

Numbers **with a decimal point**, or written in **exponent (scientific) form**.

```python
a = 9.75
b = 15.0
c = 12E2          # 12 x 10^2    = 1200.0
d = 125.5e-2      # 125.5 x 10^-2 = 1.255
e = 12_5.6_8      # underscores for readability -> 125.68
```

> **Correction:** `125.5e-2` equals **1.255** (not 0.125). It means 125.5 x 10^-2 = 125.5 / 100 = 1.255.

### Exponent form

```
12E2     = 12 x 10^2    = 1200.0
125.5e-2 = 125.5 x 10^-2 = 1.255
```

- `E` or `e` both work.
- The result of an exponent literal is always a **float** (`12E2` is `1200.0`, not `1200`).

### Underscore rules for floats

```python
e = 12_5.6_8      # valid
f = 5_._6         # wrong -> underscore cannot be next to the decimal point
g = 5._6          # wrong
h = 5_.6          # wrong
```

Underscores are allowed **only between digits**.

---

## 4. Boolean Literals

Only two values: **`True`** and **`False`** (first letter must be capital).

```python
a = True
b = False

a = true       # wrong -> NameError
b = Flalse     # wrong -> NameError (spelling mistake and wrong case)
b = false      # wrong -> NameError
```

- `True` has the integer value `1`, and `False` has the integer value `0`.

```python
print(True + 1)      # 2
print(False + 5)     # 5
```

---

## 5. Complex Literals

Written in the form **`a + bj`**, where `a` is the real part and `b` is the imaginary part.

```python
a = 4 + 5j
b = 12 + 14j
c = 1_2 + 1_4j        # underscores allowed
d = 1.4 + 2.5j        # float parts allowed

print(a, b, c, d)
# (4+5j) (12+14j) (12+14j) (1.4+2.5j)
```

> **Correction:** `12 + 14 j` (with a space before `j`) is a **SyntaxError**. The `j` must be attached to the number: `14j`.

- Use `j` (or `J`), not `i`.
- Technically, only `5j` is the literal (an imaginary literal). `4 + 5j` is an expression that adds an integer and an imaginary number to produce a complex number.
- Both parts are stored as floats internally.

```python
print(a.real)    # 4.0
print(a.imag)    # 5.0
```

---

## 6. String Literals

A sequence of characters enclosed in **quotes**.

```python
name = "Alexa"       # double quotes
name = 'Alexa'       # single quotes
name = '''Alexa'''   # triple quotes
```

| Quotes | Use |
|--------|-----|
| `' '` | Single-line string |
| `" "` | Single-line string (useful when text contains `'`) |
| `''' '''` or `""" """` | **Multi-line** strings and docstrings |

```python
msg = "It's a nice day"          # a single quote inside double quotes

para = '''This is line one.
This is line two.
This is line three.'''
print(para)
```

### Escape sequences

| Sequence | Meaning |
|----------|---------|
| `\n` | New line |
| `\t` | Tab |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |

```python
print("Hello\nWorld")
# Hello
# World
```

### Raw strings and f-strings

```python
path = r"C:\new\test"          # raw string: backslashes are not treated as escapes
name = "Alexa"
print(f"Hello, {name}")         # f-string -> Hello, Alexa
```

---

## 7. Special Literal: `None`

`None` represents **"no value"** or "nothing".

```python
result = None
print(result)           # None
print(type(result))     # <class 'NoneType'>
```

---

## 8. Collection Literals (Overview)

```python
l = [1, 2, 3]                 # list literal
t = (1, 2, 3)                 # tuple literal
s = {1, 2, 3}                 # set literal
d = {'a': 1, 'b': 2}          # dictionary literal
```

---

## 9. Quick Revision Points

- A **literal** is a fixed value written directly in the code (`10`, `9.75`, `True`, `4+5j`, `'Alexa'`).
- Types: integer, float, boolean, complex, string, `None`, plus collections.
- Integer literals come in four forms: **decimal, binary (`0b`), octal (`0o`) and hexadecimal (`0x`)**.
- 10 in different forms: `10`, `0b1010`, `0o12`, `0xA`.
- 17 in binary is `10001`, in octal `21`, and in hex `11`.
- Underscores (`_`) can be used **between digits only**, to make numbers readable.
- Float literals support exponent form: `12E2 = 1200.0` and `125.5e-2 = 1.255`.
- Boolean literals are `True` and `False` (capital first letter).
- Complex literals use `j` attached to the number: `4 + 5j`.
- Strings can use `' '`, `" "` or `''' '''`.
- Python has no true constants; by convention, UPPERCASE names are used for them.
