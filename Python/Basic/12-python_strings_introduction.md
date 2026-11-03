# Python Strings: Introduction, Indexing, Slicing & Operations
### Beginner → Advanced Notes

---

## Table of Contents
1. [What is a String?](#1-what-is-a-string)
2. [String Literals](#2-string-literals)
3. [String Representation (Unicode, escape, raw, f-string)](#3-string-representation)
4. [String Length](#4-string-length)
5. [Traversing a String](#5-traversing-a-string)
6. [String Indexing](#6-string-indexing)
7. [Strings are Immutable](#7-strings-are-immutable)
8. [String Slicing](#8-string-slicing)
9. [Operations on Strings](#9-operations-on-strings)
10. [String Methods (Preview)](#10-string-methods-preview)
11. [Common Mistakes](#11-common-mistakes)
12. [Practice Questions](#12-practice-questions)

---

## 1. What is a String?

### Beginner level

A **string** is a **collection (sequence) of characters** enclosed in **single quotes** or **double quotes**.

```python
s1 = "hello"
s2 = 'Python 3'
print(type(s1))      # <class 'str'>
```

A string can hold letters, digits, spaces and symbols: `"Hi 123 !"`.

### Key points

| Point                          | Meaning                                                     |
|--------------------------------|-------------------------------------------------------------|
| String is a **sequence**       | characters are stored **one after another** in order        |
| It is **indexed**              | each character has a position number                        |
| Python supports **negative index** | `-1` is the last character, `-2` the one before, ...    |
| It is **immutable**            | you cannot change a character in place                      |
| Data type name                 | `str`                                                       |

> 📝 Your note said "array in sequence of array". Precisely: Python has **no separate array/char type** for text. A string is a sequence of characters, and each single character is itself a string of length 1 (`'h'`). It behaves like an array for indexing and slicing, but unlike a list it cannot be modified.

### Example: positive and negative index

```python
s1 = "hello"
print(s1[0])      # h
print(s1[-1])     # o
```

```
 h    e    l    l    o
 0    1    2    3    4      <- positive index (left to right)
-5   -4   -3   -2   -1      <- negative index (right to left)
```

> ⚠️ In your notes `s1 = hello` needs quotes (`s1 = "hello"`), otherwise Python looks for a variable named `hello`. Also `prints1[-1])` should be `print(s1[-1])`.

---

## 2. String Literals

A **literal** is the way you write a fixed value in code.

### Four ways to write a string

```python
s1 = 'Hello'          # single quotes
s2 = "Hello"          # double quotes
s3 = '''Hello'''      # triple single quotes
s4 = """Hello"""      # triple double quotes
```

All four are the **same string** `Hello`. `s1 == s2 == s3 == s4` is `True`.

| Quote type        | Typical use                                       |
|-------------------|---------------------------------------------------|
| `'...'`, `"..."`  | normal one-line strings                           |
| `'''...'''`, `"""..."""` | **multi-line** strings, docstrings         |

### Multi-line strings

```python
poem = """Roses are red,
Violets are blue,
Python is fun."""
print(poem)
```

Triple quotes **keep the line breaks** exactly as typed.

### The apostrophe problem

```python
s5 = 'Clark's'         # ❌ invalid: the quote ends at "Clark"
```

Python thinks the string ended at the second `'`. Three fixes:

```python
s5 = "Clark's"         # ✅ 1. use the other kind of quote outside
s5 = 'Clark\'s'        # ✅ 2. escape the quote with a backslash
s5 = '''Clark's'''     # ✅ 3. use triple quotes
```

Similarly for double quotes inside:

```python
msg = 'He said "hi"'
msg = "He said \"hi\""
```

### Intermediate level: implicit concatenation

Literals placed side by side are joined automatically.

```python
s = "Hello " "World"
print(s)                # Hello World

long_text = ("This is a very long line "
             "split across two lines.")
```

---

## 3. String Representation

### Characters are stored as Unicode

Every character has a number (its **Unicode code point**). Use `ord()` and `chr()`:

```python
print(ord('A'))     # 65
print(ord('a'))     # 97
print(chr(66))      # B
print("नमस्ते")      # works: Python 3 strings are Unicode
```

This is why `'A' < 'a'` (65 < 97) when strings are compared.

### Escape sequences

A backslash `\` followed by a letter makes a **special character**.

| Escape | Meaning        | Example                      |
|--------|----------------|------------------------------|
| `\n`   | new line       | `print("A\nB")` → A, then B on the next line |
| `\t`   | tab            | `print("A\tB")` → `A    B`  |
| `\\`   | a backslash    | `print("C:\\temp")` → `C:\temp` |
| `\'`   | single quote   | `'It\'s'`                    |
| `\"`   | double quote   | `"Say \"hi\""`               |

### Raw strings

Prefix `r` to treat backslashes as normal characters (useful for file paths and regular expressions).

```python
print("C:\new\table")      # \n and \t are interpreted -> messy output
print(r"C:\new\table")     # C:\new\table
```

### f-strings (formatted strings)

```python
name = "Asha"
age = 21
print(f"My name is {name} and I am {age}")
print(f"Next year: {age + 1}")
print(f"{3.14159:.2f}")            # 3.14
```

### `print()` vs `repr()` (Advanced)

```python
s = "Hello\nWorld"
print(s)           # shows two lines
print(repr(s))     # 'Hello\nWorld'  -> shows the exact characters, with quotes
```

`str()` gives a friendly form for people, `repr()` gives an exact form for programmers (the interactive shell shows `repr`).

---

## 4. String Length

`len()` returns the **number of characters** (spaces and symbols count too).

```python
s1 = "Hello World"
print(len(s1))        # 11   (5 letters + 1 space + 5 letters)
print(len(""))        # 0    (empty string)
```

### Important relationship

| Item                  | Value            |
|-----------------------|------------------|
| First index           | `0`              |
| Last index            | `len(s) - 1`     |
| Last character (short way) | `s[-1]`     |

```python
s = "Python"
print(s[len(s) - 1])   # n
print(s[-1])           # n
```

---

## 5. Traversing a String

**Traversing** means visiting **each character** one by one.

### Method 1: for-each loop (simplest)

```python
s1 = "hello"
for x in s1:
    print(x)
```

```
h
e
l
l
o
```

### Method 2: using the index with `range(len(...))`

```python
s1 = "hello"
for i in range(len(s1)):
    print(s1[i])
```

> ⚠️ Your note had `for i in range(s1[i])`. That fails because `i` does not exist yet and `s1[i]` is a character, not a number. `range()` needs a **number**, so use `range(len(s1))`.

### Method 3: with `enumerate` (index + character)

```python
for i, ch in enumerate("hello"):
    print(i, ch)
# 0 h
# 1 e
# ...
```

### Method 4: `while` loop

```python
s1 = "hello"
i = 0
while i < len(s1):
    print(s1[i])
    i += 1
```

### Backwards

```python
for ch in reversed("hello"):
    print(ch)               # o l l e h

for i in range(len("hello") - 1, -1, -1):
    print("hello"[i])       # o l l e h
```

### Practical examples

```python
# count vowels
s = "Education"
count = 0
for ch in s:
    if ch.lower() in "aeiou":
        count += 1
print(count)       # 5

# count a character
print("banana".count("a"))     # 3 (built-in method)
```

---

## 6. String Indexing

### Beginner level

**Indexing** means getting **one character** using its position in square brackets.

```python
s1 = "Hello World"
print(s1[0])       # H
print(s1[-2])      # l
```

Index map for `"Hello World"` (the space is a character too):

```
 H    e    l    l    o         W    o    r    l    d
 0    1    2    3    4    5    6    7    8    9    10      <- positive
-11  -10  -9   -8   -7   -6   -5   -4   -3   -2   -1       <- negative
```

| Expression  | Result | Reason                    |
|-------------|--------|---------------------------|
| `s1[0]`     | `'H'`  | first character           |
| `s1[4]`     | `'o'`  |                           |
| `s1[5]`     | `' '`  | the space                 |
| `s1[6]`     | `'W'`  |                           |
| `s1[10]`    | `'d'`  | last character            |
| `s1[-1]`    | `'d'`  | last character            |
| `s1[-2]`    | `'l'`  | second from the end       |
| `s1[-11]`   | `'H'`  | first character           |

### IndexError

Using an index outside the valid range gives an error.

```python
print(s1[11])      # IndexError: string index out of range
print(s1[-12])     # IndexError
```

Valid range: `-len(s)` to `len(s) - 1` (here `-11` to `10`).

### Intermediate level

- Indexing returns a **string of length 1**, not a special char type: `type(s1[0])` is `str`.
- Index must be an **integer**: `s1[1.5]` → `TypeError`.
- Indexing an empty string always fails: `""[0]` → `IndexError`.

---

## 7. Strings are Immutable

**Immutable** = once a string is created, its characters **cannot be changed**.

```python
s1 = "hello"
s1[0] = "j"        # ❌ TypeError: 'str' object does not support item assignment
```

### How to "change" a string

Create a **new** string and assign it back.

```python
s1 = "hello"
s1 = "j" + s1[1:]          # 'jello'
s1 = s1.replace("j", "h")  # 'hello'
```

### Proof that the old string is untouched

```python
s1 = "Hello"
s2 = s1                    # both names point to the same string
s1 = s1 + " World"         # creates a NEW string
print(s1)                  # Hello World
print(s2)                  # Hello  (unchanged)
```

String methods never modify the original:

```python
s = "hello"
s.upper()                  # returns 'HELLO' but...
print(s)                   # hello  (unchanged)
s = s.upper()              # you must store the result
```

### Advanced: why is it designed this way?

| Benefit                                  | Explanation                                      |
|------------------------------------------|--------------------------------------------------|
| **Safe sharing**                         | many variables can point to one string safely    |
| **Hashable**                             | strings can be dictionary keys and set members   |
| **Optimisation (interning)**             | Python can reuse identical short strings         |

Cost: building a string by repeating `+=` inside a loop creates many temporary strings. For large work, collect pieces in a list and use `"".join(parts)`.

---

## 8. String Slicing

### Beginner level

**Slicing** extracts a **part (substring)** of a string.

```python
seq[start:stop:step]
```

Works for any **sequence**: string, list, tuple.

| Part    | Meaning                                  | Default   |
|---------|------------------------------------------|-----------|
| `start` | index where the slice **begins** (included) | `0`    |
| `stop`  | index where it **ends** (**not included**) | `len(s)`|
| `step`  | gap between characters                   | `1`       |

> ✅ Same rule as `range()`: the **stop** index is **never included**.

### Forms of slicing

```python
s1 = "Hello World"
```

| Expression      | Result           | Explanation                                |
|-----------------|------------------|--------------------------------------------|
| `s1[6]`         | `'W'`            | single index (not a slice)                 |
| `s1[1:7]`       | `'ello W'`       | indices 1,2,3,4,5,6                        |
| `s1[0:5]`       | `'Hello'`        | first five                                 |
| `s1[:5]`        | `'Hello'`        | start omitted → 0                          |
| `s1[6:]`        | `'World'`        | stop omitted → till the end                |
| `s1[:]`         | `'Hello World'`  | whole string (a copy)                      |
| `s1[::]`        | `'Hello World'`  | same as above                              |
| `s1[0:11:2]`    | `'HloWrd'`       | indices 0,2,4,6,8,10                       |
| `s1[::2]`       | `'HloWrd'`       | every 2nd character                        |
| `s1[1::2]`      | `'el ol'`        | indices 1,3,5,7,9                          |

Walk through `s1[0:11:2]`:

```
index :  0  1  2  3  4  5  6  7  8  9  10
char  :  H  e  l  l  o     W  o  r  l  d
pick  :  H     l     o     W     r     d      -> "HloWrd"
```

### Negative indexes in slices

| Expression      | Result           | Explanation                           |
|-----------------|------------------|---------------------------------------|
| `s1[-5:]`       | `'World'`        | last 5 characters                     |
| `s1[:-1]`       | `'Hello Worl'`   | everything except the last character  |
| `s1[-5:-1]`     | `'Worl'`         | indices 6 to 9                        |
| `s1[3:8]`       | `'lo Wo'`        | indices 3 to 7                        |

### Negative step: going backwards

When `step` is negative, the slice moves **right to left**, so `start` should be **greater than** `stop`.

| Expression        | Result            | Explanation                          |
|-------------------|-------------------|--------------------------------------|
| `s1[::-1]`        | `'dlroW olleH'`   | **reverse the whole string**         |
| `s1[8:2:-1]`      | `'roW ol'`        | indices 8,7,6,5,4,3                  |
| `s1[-1:-6:-1]`    | `'dlroW'`         | last 5 characters, reversed          |

### Intermediate level: safe slicing

Unlike indexing, **slicing never raises `IndexError`**. Out-of-range values are trimmed.

```python
print(s1[2:100])    # 'llo World'
print(s1[50:60])    # ''  (empty string)
print(s1[5:2])      # ''  (start after stop with positive step)
```

`step` cannot be `0`: `s1[::0]` → `ValueError`.

### Advanced level

```python
# palindrome check
word = "madam"
print(word == word[::-1])        # True

# swap first and last character
s = "hello"
print(s[-1] + s[1:-1] + s[0])    # oellh

# remove first and last character
print(s[1:-1])                   # ell

# every 3rd character
print("abcdefghi"[::3])          # adg

# slice object (reusable slice)
first3 = slice(0, 3)
print("Python"[first3])          # Pyt
print("Hello"[first3])           # Hel
```

A slice always creates a **new string**; the original is never changed.

### Quick memory trick

```
s[start : stop : step]
     ^       ^      ^
  included  NOT    gap
            included
```

---

## 9. Operations on Strings

### Operators

| Operation       | Operator      | Example                      | Result            |
|-----------------|---------------|------------------------------|-------------------|
| Concatenation   | `+`           | `"Hello" + " World"`         | `'Hello World'`   |
| Repetition      | `*`           | `"ab" * 3`                   | `'ababab'`        |
| Membership      | `in`          | `"ell" in "Hello"`           | `True`            |
| Non-membership  | `not in`      | `"z" not in "Hello"`         | `True`            |
| Comparison      | `<  <=  >  >=  ==  !=` | `"cat" < "catch"`   | `True`            |
| Indexing        | `s[i]`        | `"Hello"[1]`                 | `'e'`             |
| Slicing         | `s[a:b:c]`    | `"Hello"[1:4]`               | `'ell'`           |
| Length          | `len(s)`      | `len("Hello")`               | `5`               |

### Concatenation `+`

```python
first = "Hello"
last = "World"
print(first + " " + last)      # Hello World
```

> ⚠️ Both sides must be strings: `"Age: " + 21` → `TypeError`. Convert first: `"Age: " + str(21)` or use an f-string: `f"Age: {21}"`.

### Repetition `*`

```python
print("ab" * 3)        # ababab
print("-" * 20)        # --------------------
print("x" * 0)         # ''  (empty)
print("x" * -2)        # ''  (negative count gives empty)
```

### Membership `in`

```python
print("ell" in "Hello")        # True  (works for substrings, not only single characters)
print("abc" in "Hello")        # False
print("H" in "Hello")          # True
print("h" in "Hello")          # False (case matters)
```

### Comparison

Strings are compared **character by character** using Unicode values (lexicographic / dictionary order).

```python
print("apple" < "banana")      # True
print("Data" < "data")         # True   ('D' = 68, 'd' = 100)
print("cat" < "catch")         # True   (prefix is smaller)
print("abc" == "abc")          # True
```

### Built-in functions that work on strings

```python
s = "python"
print(len(s))              # 6
print(max(s))              # 'y'  (largest Unicode value)
print(min(s))              # 'h'
print(sorted(s))           # ['h', 'n', 'o', 'p', 't', 'y']
print("".join(sorted(s)))  # 'hnopty'
print(list(s))             # ['p', 'y', 't', 'h', 'o', 'n']
print(str(123) + "4")      # '1234'
print(int("123") + 4)      # 127
```

### Advanced: efficient building of strings

```python
# slow for huge loops: creates a new string every time
result = ""
for i in range(5):
    result += str(i)

# better: collect and join once
parts = []
for i in range(5):
    parts.append(str(i))
result = "".join(parts)       # '01234'
```

---

## 10. String Methods (Preview)

Methods are functions that belong to a string, called with a dot: `s.method()`. They **return new strings**; the original stays the same.

| Group          | Method                  | Example                          | Result                |
|----------------|-------------------------|----------------------------------|-----------------------|
| Case           | `upper()`               | `"hello".upper()`                | `'HELLO'`             |
|                | `lower()`               | `"HELLO".lower()`                | `'hello'`             |
|                | `title()`               | `"hello world".title()`          | `'Hello World'`       |
|                | `capitalize()`          | `"hello world".capitalize()`     | `'Hello world'`       |
|                | `swapcase()`            | `"Hello".swapcase()`             | `'hELLO'`             |
| Clean          | `strip()`               | `"  hi  ".strip()`               | `'hi'`                |
| Search         | `find(x)`               | `"hello".find("l")`              | `2` (or `-1` if absent)|
|                | `index(x)`              | `"hello".index("e")`             | `1` (error if absent) |
|                | `count(x)`              | `"hello".count("l")`             | `2`                   |
|                | `startswith(x)`         | `"hello".startswith("he")`       | `True`                |
|                | `endswith(x)`           | `"hello".endswith("lo")`         | `True`                |
| Change         | `replace(a, b)`         | `"hello".replace("l", "L")`      | `'heLLo'`             |
| Split / join   | `split(sep)`            | `"a,b,c".split(",")`             | `['a', 'b', 'c']`     |
|                | `sep.join(list)`        | `"-".join(["a", "b"])`           | `'a-b'`               |
| Check type     | `isdigit()`             | `"123".isdigit()`                | `True`                |
|                | `isalpha()`             | `"abc".isalpha()`                | `True`                |
|                | `isalnum()`             | `"abc1".isalnum()`               | `True`                |
|                | `isupper()` / `islower()` | `"ABC".isupper()`              | `True`                |

(Each method gets a full explanation in the next section.)

---

## 11. Common Mistakes

| Mistake                                   | Fix                                              |
|-------------------------------------------|--------------------------------------------------|
| `s1 = hello` (no quotes)                  | `s1 = "hello"`                                   |
| `'Clark's'`                               | `"Clark's"` or `'Clark\'s'`                      |
| `s[0] = 'J'` to change a character        | strings are immutable: `s = 'J' + s[1:]`         |
| `range(s1[i])`                            | `range(len(s1))`                                 |
| Using index `len(s)`                      | last index is `len(s) - 1`                       |
| Expecting `s[a:b]` to include `b`         | stop is **excluded**                             |
| Calling `s.upper()` and not saving result | `s = s.upper()`                                  |
| `"Age " + 21`                             | `"Age " + str(21)`                               |
| Forgetting a space counts as a character  | `len("Hello World")` is 11, not 10               |
| Mixing up `'h'` and `'H'`                 | strings are **case-sensitive**                   |
| Typos like `prints1[-1])`                 | `print(s1[-1])`                                  |

---

## 12. Practice Questions

Predict the output first.

```python
s = "Python"

# 1
print(s[1:4])

# 2
print(s[-3:])

# 3
print(s[::-1])

# 4
print(s[::2])

# 5
print(s[10:])

# 6
print(len(s))

# 7
print("ab" * 3 + "c")

# 8
print("Hello World"[3:8])

# 9
print("a" in "cat", "A" in "cat")

# 10
t = "hello"
t[0] = "j"
```

<details>
<summary>Answers</summary>

1. `yth`
2. `hon`
3. `nohtyP`
4. `Pto`
5. `` (empty string, slicing never raises an error)
6. `6`
7. `abababc`
8. `lo Wo`
9. `True False`
10. `TypeError: 'str' object does not support item assignment`

</details>

### Mini challenges

1. Print each character of a string with its index (positive and negative).
2. Count the vowels and consonants in a string.
3. Reverse a string without using `[::-1]` (use a loop).
4. Check whether a string is a palindrome.
5. Print the first and last characters of a string.
6. Print every character at an even index.
7. Take a name as input and print its initials, e.g. `"Rahul Kumar Sharma"` → `RKS`.
8. Check whether two strings are anagrams (hint: compare `sorted()` versions).
