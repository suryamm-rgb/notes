# Python `str` Class and Its Methods
### Beginner → Advanced Notes

---

## Table of Contents
1. [String as a Class and Object](#1-string-as-a-class-and-object)
2. [Methods of a Class: dir() and help()](#2-methods-of-a-class-dir-and-help)
3. [Find and Index Methods](#3-find-and-index-methods)
4. [Alignment and Padding Methods](#4-alignment-and-padding-methods)
5. [Strip Methods](#5-strip-methods)
6. [Replace, Join and Split Methods](#6-replace-join-and-split-methods)
7. [Prefix and Suffix Methods](#7-prefix-and-suffix-methods)
8. [Partition Methods](#8-partition-methods)
9. [Case Conversion Methods](#9-case-conversion-methods)
10. [Inquiry Methods (Boolean)](#10-inquiry-methods-boolean)
11. [Method Chaining and Immutability](#11-method-chaining-and-immutability)
12. [Mini Programs](#12-mini-programs)
13. [Cheat Sheet](#13-cheat-sheet)
14. [Common Mistakes](#14-common-mistakes)
15. [Practice Questions](#15-practice-questions)

---

## 1. String as a Class and Object

### Beginner level

```python
s1 = 'Hello'
print(type(s1))        # <class 'str'>
```

`type()` shows that `s1` belongs to the class **`str`**.

| Term       | Meaning                                                      | Example                       |
|------------|--------------------------------------------------------------|-------------------------------|
| **Class**  | a **blueprint** that describes data and the operations on it | `str`                         |
| **Object** | an **actual value** created from the class (an *instance*)   | `'Hello'`, `'python'`         |
| **Method** | a **function that belongs to a class**, called with a dot    | `'Hello'.upper()`             |

### Why classes?

A class **groups the related data and the operations on that data together**.

```
class str
 ├── data     : the characters, the length
 └── methods  : find(), index(), endswith(), isalpha(), lower(), upper() ...
```

For the object `s2 = 'python'`:

| Part         | Value                                                  |
|--------------|--------------------------------------------------------|
| **Data**     | string = `'python'`, length = `6`                      |
| **Methods**  | `find()`, `index()`, `endswith()`, `isalpha()`, `lower()`, `upper()` ... |

Every string object has **its own data** but **shares the same methods**.

```python
s1 = 'Hello'
s2 = 'python'
print(s1.upper())      # HELLO
print(s2.upper())      # PYTHON
```

### Calling a method

```
object . method ( arguments )
 s2    . find   ( 'th' )
```

```python
s2 = 'python'
print(s2.find('th'))      # 2
print(s2.upper())         # PYTHON
```

### Intermediate level

- `isinstance(s2, str)` → `True`.
- Calling through the class gives the same result: `str.upper(s2)` is the same as `s2.upper()`. Python passes the object as the first argument (`self`).
- Even `len(s2)` works through a method behind the scenes: it calls `s2.__len__()`.

### Advanced: the idea in your own class

```python
class Student:
    def __init__(self, name, marks):
        self.name = name            # data
        self.marks = marks          # data

    def result(self):               # method
        return "pass" if self.marks >= 35 else "fail"

s = Student("Asha", 72)
print(s.name, s.result())           # Asha pass
```

`str` is a ready-made class like this, written for you.

---

## 2. Methods of a Class: dir() and help()

### `dir(str)` lists everything inside the class

```python
print(dir(str))
```

It prints many names. Names with double underscores (`__len__`, `__add__`) are **special (dunder) methods** used by Python itself. To see only the ones you call directly:

```python
public = [m for m in dir(str) if not m.startswith('__')]
print(public)
print(len(public))       # around 47 in recent Python versions
```

### `help()` explains a method

```python
help(str.find)
```

```
find(...)
    S.find(sub[, start[, end]]) -> int
```

### Method groups covered in these notes

| Group                       | Methods                                                         |
|-----------------------------|-----------------------------------------------------------------|
| Find / index / count        | `find`, `rfind`, `index`, `rindex`, `count`                     |
| Alignment and padding       | `ljust`, `rjust`, `center`, `zfill`                             |
| Strip                       | `strip`, `lstrip`, `rstrip`                                     |
| Replace / join / split      | `replace`, `join`, `split`, `rsplit`, `splitlines`              |
| Prefix and suffix           | `startswith`, `endswith`, `removeprefix`, `removesuffix`        |
| Partition                   | `partition`, `rpartition`                                       |
| Case conversion             | `capitalize`, `upper`, `lower`, `title`, `swapcase`, `casefold` |
| Inquiry (return True/False) | `isalpha`, `islower`, `isupper`, `istitle`, `isspace`, `isidentifier`, `isprintable`, `isdigit`, `isdecimal`, `isnumeric`, `isascii`, `isalnum` |

> ✅ **Important:** strings are **immutable**, so **no method changes the original string**. Each method **returns a new value**. Store it if you need it: `s = s.upper()`.

---

## 3. Find and Index Methods

### Syntax

| Method                        | Purpose                                              | If not found       |
|-------------------------------|------------------------------------------------------|--------------------|
| `find(sub, start, end)`       | index of the **first** occurrence of `sub`           | returns **`-1`**   |
| `rfind(sub, start, end)`      | index of the **last** occurrence (searches from right) | returns **`-1`** |
| `index(sub, start, end)`      | same as `find`                                       | raises **`ValueError`** |
| `rindex(sub, start, end)`     | same as `rfind`                                      | raises **`ValueError`** |
| `count(sub, start, end)`      | **how many times** `sub` appears (no overlap)        | returns `0`        |

- `sub` = the substring to look for (one character or many).
- `start`, `end` are **optional**; the search happens in the slice `s[start:end]`. The `end` index is **not included**.
- The search is **case-sensitive**.

### The test string

```python
S = 'hello how are you'
```

```
char :   h  e  l  l  o  ·  h  o  w  ·  a  r  e  ·  y  o  u
index:   0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16
```
(`·` marks a space. The string has 17 characters.)

### `find()`

```python
x = S.find('o')
print(x)                  # 4     first 'o'

x = S.find('how')
print(x)                  # 6

x = S.find('How')
print(x)                  # -1    capital H does not match (case-sensitive)

x = S.find('k')
print(x)                  # -1    not present

y = S.find('o', 5)
print(y)                  # 7     search starts from index 5

z = S.find('o', 5, 7)
print(z)                  # -1    only indexes 5 and 6 are searched (' ' and 'h')
```

### `rfind()`

```python
print(S.rfind('o'))           # 15   last 'o'
print(S.rfind('o', 0, 15))    # 7    index 15 is excluded, so the last 'o' before it is 7
print(S.rfind('k'))           # -1
```

### `index()` and `rindex()`

Same as `find` / `rfind`, but they **raise an error** when the text is missing.

```python
print(S.index('o'))      # 4
print(S.index('e'))      # 1
print(S.rindex('o'))     # 15
print(S.index('k'))      # ValueError: substring not found
```

### `find` vs `index`

| Situation                              | Use                                     |
|----------------------------------------|-----------------------------------------|
| Missing text is **normal**, you will check | `find()` (check `== -1`)            |
| Missing text is a **bug** and should stop the program | `index()` (let it raise)  |

### `count()`

```python
print(S.count('o'))         # 3   (at 4, 7, 15)
print(S.count('o', 5))      # 2   (at 7, 15)
print(S.count('h'))         # 2   (at 0, 6)
print(S.count('l'))         # 2
print('aaaa'.count('aa'))   # 2   (non-overlapping)
```

### Advanced notes

**1. Danger: `-1` is truthy, `0` is falsy**

```python
if S.find('h'):          # ❌ index 0 -> False, so this check is wrong!
    ...
if S.find('k'):          # ❌ -1 -> True, even though 'k' is missing!
    ...
```

Correct ways:

```python
if S.find('h') != -1:    # ✅ explicit
    ...
if 'h' in S:             # ✅ best when you only need yes/no
    ...
```

**2. Find all positions of a substring**

```python
S = 'hello how are you'
pos = S.find('o')
while pos != -1:
    print(pos, end=' ')          # 4 7 15
    pos = S.find('o', pos + 1)
```

**3. Negative `start` / `end`** work like slicing: `S.find('o', -5)` searches the last 5 characters (returns `15`).

---

## 4. Alignment and Padding Methods

These return a string of **at least `width` characters**, adding **fill characters** to reach that width. Default fill is a **space**.

| Method                    | Alignment of the text | Padding added on  |
|---------------------------|-----------------------|-------------------|
| `ljust(width, fillchar)`  | left                  | right             |
| `rjust(width, fillchar)`  | right                 | left              |
| `center(width, fillchar)` | centre                | both sides        |
| `zfill(width)`            | right, filled with `0`| left              |

`fillchar` must be **exactly one character**.

```python
S = 'hello'

print(S.ljust(11, '*'))      # hello******
print(S.rjust(11, '*'))      # ******hello
print(S.center(11, '*'))     # ***hello***
print(S.ljust(11))           # 'hello      '  (spaces)
```

> 📝 In your notes `S.ljust(11, .*)` is not valid. The fill character must be one character inside quotes, such as `'*'` or `'.'`.

### Width smaller than the string

If `width` is less than or equal to the length, the **original string is returned unchanged**.

```python
print('hello'.ljust(3, '*'))     # hello
```

### `zfill(width)`: pad with zeros

```python
print('42'.zfill(5))       # 00042
print('-42'.zfill(5))      # -0042   (sign stays at the front)
print('abc'.zfill(6))      # 000abc
```

### Intermediate: a neat table

```python
items = [("Pen", 10), ("Notebook", 45), ("Bag", 399)]
for name, price in items:
    print(name.ljust(10, '.') + str(price).rjust(5))
```

```
Pen.......   10
Notebook..   45
Bag.......  399
```

### Advanced: f-string alternative

```python
print(f"{'hello':*<11}")     # hello******   (left)
print(f"{'hello':*>11}")     # ******hello   (right)
print(f"{'hello':*^11}")     # ***hello***   (centre)
print(f"{42:05}")            # 00042
```

When the padding is odd (it cannot split equally), `center` puts the extra character on one side.

---

## 5. Strip Methods

Strip methods **remove characters from the ends** (never from the middle).

| Method          | Removes from            |
|-----------------|-------------------------|
| `lstrip(chars)` | the **left** end        |
| `rstrip(chars)` | the **right** end       |
| `strip(chars)`  | **both** ends           |

By default (`chars` not given) they remove **whitespace**: spaces, tabs `\t`, newlines `\n`.

```python
s = '   hello world   '

print(s.lstrip())      # 'hello world   '
print(s.rstrip())      # '   hello world'
print(s.strip())       # 'hello world'    (the space in the middle stays)
```

### With the `chars` argument

```python
print('***hello***'.strip('*'))        # hello
print('***hello***'.lstrip('*'))       # hello***
print('***hello***'.rstrip('*'))       # ***hello
print('\n  hi \t'.strip())             # hi
```

### Intermediate: `chars` is a SET of characters, not a prefix

Python removes **any of those characters**, from the ends, **until it meets a character that is not in the set**.

```python
print('abchelloabc'.strip('abc'))        # hello
print('www.python.com'.strip('wcom.'))   # python
print('xyhixy'.strip('xy'))              # hi
```

So `strip('www.')` does **not** mean "remove the text www.". For removing an exact prefix or suffix use `removeprefix()` / `removesuffix()` (section 7).

### Typical use

```python
name = input("Enter name: ").strip()      # remove accidental spaces
line = "data,1,2\n"
print(line.rstrip('\n'))                  # data,1,2
```

---

## 6. Replace, Join and Split Methods

### `replace(old, new, count)`

Replaces **every** occurrence of `old` with `new`. If `count` is given, only the **first `count`** occurrences change.

```python
s1 = 'a-b-c-d-e'

x = s1.replace('-', '*', 2)
y = s1.replace('-', ',')
print(x)        # a*b*c-d-e
print(y)        # a,b,c,d,e
print(s1)       # a-b-c-d-e   (original unchanged)
```

```python
s2 = 'abc@gmail.com'
s3 = s2.replace('gmail', 'yahoo')
print(s3)       # abc@yahoo.com
```

> 📝 In your notes `replace(-,*,2)` needs quotes: `replace('-', '*', 2)`. And `'gamile mal'` should be two quoted arguments, `'gmail', 'yahoo'`.

Replace with nothing = delete:

```python
print('1,000,000'.replace(',', ''))      # 1000000
```

### `join(iterable)`

Joins the items of an iterable into **one string**. The string **before the dot** is the **separator**.

```python
print('-'.join(['a', 'b', 'c']))        # a-b-c
print(' '.join(['I', 'love', 'Python']))# I love Python
print(''.join(['a', 'b', 'c']))         # abc
```

A string is also an iterable (of characters), so this happens:

```python
s1 = 'xyz'
s2 = 'abc'
s3 = s1.join(s2)
print(s3)           # axyzbxyzc
```

Explanation: `s2` has the characters `'a'`, `'b'`, `'c'`. `s1` (`'xyz'`) is put **between** them: `a` + `xyz` + `b` + `xyz` + `c`.

> ⚠️ `join` works only on **strings**. For numbers convert first:
> ```python
> nums = [1, 2, 3]
> print('-'.join(map(str, nums)))     # 1-2-3
> ' '.join(nums)                      # ❌ TypeError
> ```

### `split(sep, maxsplit)`

Breaks a string into a **list** of pieces.

```python
s1 = 'john smith ajay'
s2 = s1.split()
print(s2)               # ['john', 'smith', 'ajay']

print('a,b,c'.split(','))             # ['a', 'b', 'c']
print('a-b-c-d'.split('-', 2))        # ['a', 'b', 'c-d']   at most 2 splits
```

> 📝 In your notes `s1.spit()` is a typo for `s1.split()`.

**`split()` with no argument vs `split(' ')`**

```python
t = 'a  b'                     # two spaces
print(t.split())               # ['a', 'b']        any amount of whitespace, no empty items
print(t.split(' '))            # ['a', '', 'b']    an empty string appears
```

### `rsplit(sep, maxsplit)`

Same as `split`, but the `maxsplit` count is applied **from the right**.

```python
s = 'a-b-c-d'
print(s.split('-', 2))      # ['a', 'b', 'c-d']
print(s.rsplit('-', 2))     # ['a-b', 'c', 'd']
print(s.rsplit('-', 1))     # ['a-b-c', 'd']
```

Without `maxsplit` both give the same result.

### `splitlines(keepends)`

Splits at **line breaks** (`\n`, `\r\n`, ...).

```python
text = 'line1\nline2\nline3'
print(text.splitlines())          # ['line1', 'line2', 'line3']
print(text.splitlines(True))      # ['line1\n', 'line2\n', 'line3']
```

### Split + join = the usual workflow

```python
sentence = 'how are you'
words = sentence.split()
print(len(words))                     # 3
print(' '.join(words[::-1]))          # you are how
print('_'.join(words))                # how_are_you
```

---

## 7. Prefix and Suffix Methods

### `startswith(prefix, start, end)` and `endswith(suffix, start, end)`

Return **`True` / `False`**.

```python
s1 = 'rossum@gmail.com'

print(s1.startswith('rossum'))      # True
print(s1.startswith('Rossum'))      # False  (case-sensitive)
print(s1.endswith('.com'))          # True
print(s1.endswith('.org'))          # False
```

**With `start` and `end`** (they test the slice `s[start:end]`):

```python
print('hello'.startswith('ll', 2))      # True   (from index 2 the text is 'llo')
print('hello'.endswith('ell', 0, 4))    # True   ('hell' ends with 'ell')
```

**Several options at once: pass a tuple**

```python
filename = 'photo.jpg'
print(filename.endswith(('.jpg', '.png', '.gif')))        # True

url = 'https://example.com'
print(url.startswith(('http://', 'https://')))            # True
```

### `removeprefix(prefix)` and `removesuffix(suffix)` (Python 3.9+)

Remove an **exact** text from the start or end. If it is not there, the string is returned unchanged.

```python
print('TestCase'.removeprefix('Test'))             # Case
print('rossum@gmail.com'.removesuffix('.com'))     # rossum@gmail
print('hello'.removeprefix('xyz'))                 # hello
```

> 📝 Your note spelled it `remove suffix(suffix)`; the method name is one word: `removesuffix`.

### `strip` vs `removeprefix` / `removesuffix`

```python
print('abcabc-data'.lstrip('abc'))           # -data      (all of a, b, c removed)
print('abcabc-data'.removeprefix('abc'))     # abc-data   (exact text removed once)
```

---

## 8. Partition Methods

### `partition(sep)`

Splits the string at the **first** occurrence of `sep` and returns a **tuple of 3 parts**:

```
(before, separator, after)
```

```python
s = 'rossum@gmail.com'
print(s.partition('@'))             # ('rossum', '@', 'gmail.com')

user, sep, domain = s.partition('@')
print(user)                         # rossum
print(domain)                       # gmail.com
```

If the separator is **not found**: `(whole string, '', '')`.

```python
print('hello'.partition('@'))       # ('hello', '', '')
```

### `rpartition(sep)`

Splits at the **last** occurrence.

```python
path = 'a.b.c'
print(path.partition('.'))          # ('a', '.', 'b.c')
print(path.rpartition('.'))         # ('a.b', '.', 'c')

filename = 'report.final.pdf'
name, dot, ext = filename.rpartition('.')
print(ext)                          # pdf
```

If not found: `('', '', whole string)`.

```python
print('hello'.rpartition('@'))      # ('', '', 'hello')
```

### `partition` vs `split`

| Feature                 | `partition`                   | `split`                          |
|-------------------------|-------------------------------|----------------------------------|
| Returns                 | tuple of **exactly 3** items  | list of **any length**           |
| Keeps the separator     | ✅ yes                        | ❌ no                            |
| Splits at               | first (or last) match only    | every match (unless `maxsplit`)  |
| When separator missing  | `(s, '', '')`                 | `[s]`                            |

---

## 9. Case Conversion Methods

| Method         | Description                                     | Example                          | Result                  |
|----------------|-------------------------------------------------|----------------------------------|-------------------------|
| `capitalize()` | first letter upper, **rest lower**              | `'hELLO wORLD'.capitalize()`     | `'Hello world'`         |
| `upper()`      | all upper                                       | `'Hello'.upper()`                | `'HELLO'`               |
| `lower()`      | all lower                                       | `'Hello'.lower()`                | `'hello'`               |
| `title()`      | first letter of **each word** upper             | `'hello world'.title()`          | `'Hello World'`         |
| `swapcase()`   | upper ↔ lower                                   | `'Hello World'.swapcase()`       | `'hELLO wORLD'`         |
| `casefold()`   | **aggressive lower**, for caseless comparison   | `'Straße'.casefold()`            | `'strasse'`             |

```python
s = 'python programming'
print(s.capitalize())     # Python programming
print(s.upper())          # PYTHON PROGRAMMING
print(s.title())          # Python Programming
print(s.swapcase())       # PYTHON PROGRAMMING
```

### Intermediate: case-insensitive comparison

```python
a = 'Python'
b = 'PYTHON'
print(a == b)                          # False
print(a.lower() == b.lower())          # True
print(a.casefold() == b.casefold())    # True  (best choice)
```

### Advanced: `lower()` vs `casefold()`

```python
print('Straße'.lower())       # straße    (ß stays)
print('Straße'.casefold())    # strasse   (ß becomes ss)
print('STRASSE'.casefold() == 'Straße'.casefold())    # True
```

For plain English text both behave the same. `casefold()` matters for international text (German, Greek, ...).

### Gotchas

```python
print("they're here".title())      # They'Re Here   (apostrophe confuses title())
print('hELLO'.capitalize())        # Hello           (rest is made lowercase)
```

---

## 10. Inquiry Methods (Boolean)

These methods **ask a question** about the string and return **`True` or `False`**.

### Group 1: letters, case, spaces, identifiers

| Method           | Returns `True` when...                                          | Example → Result                            |
|------------------|-----------------------------------------------------------------|---------------------------------------------|
| `isalpha()`      | **all** characters are letters and there is at least one        | `'hello'` → True, `'hello1'` → False, `'hello world'` → False |
| `islower()`      | has at least one cased letter and **all cased letters are lowercase** | `'hello'` → True, `'Hello'` → False, `'abc1'` → True, `'123'` → False |
| `isupper()`      | has at least one cased letter and **all cased letters are uppercase** | `'HELLO'` → True, `'Hello'` → False   |
| `istitle()`      | each word starts with a capital, the rest lowercase             | `'Hello World'` → True, `'Hello world'` → False |
| `isspace()`      | **all** characters are whitespace (space, `\t`, `\n`), not empty | `'  \t'` → True, `''` → False, `' a '` → False |
| `isidentifier()` | a valid Python variable name                                    | `'my_var'` → True, `'2var'` → False, `'my var'` → False |
| `isprintable()`  | all characters are printable (no `\n`, `\t`, control characters) | `'hello world'` → True, `'hello\n'` → False, `''` → True |

```python
print('hello'.isalpha())        # True
print('hello123'.isalpha())     # False
print('hello world'.isalpha())  # False  (space is not a letter)
print(''.isalpha())             # False  (empty string)

print('hello'.islower())        # True
print('HELLO'.isupper())        # True
print('Hello World'.istitle())  # True
print('   '.isspace())          # True
print('my_var1'.isidentifier()) # True
print('1abc'.isidentifier())    # False
print('Hello\n'.isprintable())  # False
```

> ⚠️ `isidentifier()` only checks the **form**. Keywords such as `'class'` also return `True`. Use `keyword.iskeyword('class')` to detect those.

### Group 2: digits, numbers, ASCII, alphanumeric

| Method          | Returns `True` when...                                          |
|-----------------|-----------------------------------------------------------------|
| `isdecimal()`   | all characters are **decimal digits** (0-9 and digits of other scripts) |
| `isdigit()`     | decimal digits **plus** special digit forms such as superscripts (`²`) |
| `isnumeric()`   | digits **plus** fractions, roman numerals, Chinese numerals (`½`, `Ⅷ`, `三`) |
| `isascii()`     | all characters have a code point below 128 (empty string is `True`) |
| `isalnum()`     | all characters are letters or numbers (`isalpha()` or `isdecimal()` or `isdigit()` or `isnumeric()`), at least one |

**The digit family: from strictest to most flexible**

```
isdecimal()  ⊂  isdigit()  ⊂  isnumeric()
```

| String      | `isdecimal()` | `isdigit()` | `isnumeric()` |
|-------------|---------------|-------------|---------------|
| `'123'`     | True          | True        | True          |
| `'²'`       | False         | True        | True          |
| `'½'`       | False         | False       | True          |
| `'三'`      | False         | False       | True          |
| `'12.5'`    | False         | False       | False         |
| `'-5'`      | False         | False       | False         |
| `''`        | False         | False       | False         |

```python
print('123'.isdigit())          # True
print('12.5'.isdigit())         # False   (the dot is not a digit)
print('-5'.isdigit())           # False   (the minus sign is not a digit)
print('\u00b2'.isdigit())       # True    ('²')
print('\u00b2'.isdecimal())     # False
print('\u00bd'.isnumeric())     # True    ('½')

print('hello'.isascii())        # True
print('café'.isascii())         # False
print('abc123'.isalnum())       # True
print('abc 123'.isalnum())      # False   (space)
print('abc_123'.isalnum())      # False   (underscore)
```

### Which one should I use?

| Task                                                   | Use                              |
|--------------------------------------------------------|----------------------------------|
| Check that user typed only letters                     | `isalpha()`                      |
| Check that input is a whole number made of 0-9 digits  | `isdecimal()` (safest before `int()`) |
| Check for username (letters + digits)                  | `isalnum()`                      |
| Check for valid variable name                          | `isidentifier()`                 |
| Check the text is plain English / ASCII                | `isascii()`                      |
| Float or negative number check                         | none of these: use `try: float(s)` |

```python
s = input("Enter age: ")
if s.isdecimal():
    age = int(s)
else:
    print("Please enter digits only")
```

---

## 11. Method Chaining and Immutability

Because every method returns a **new string**, you can call another method on the result. This is **method chaining**. It runs **left to right**.

```python
s = '  Hello World  '
print(s.strip().lower().replace('o', '0'))     # hell0 w0rld
print(s)                                       # '  Hello World  '  (unchanged)
```

Step by step:

| Step | Expression                        | Result              |
|------|-----------------------------------|---------------------|
| 1    | `s.strip()`                       | `'Hello World'`     |
| 2    | `.lower()`                        | `'hello world'`     |
| 3    | `.replace('o', '0')`              | `'hell0 w0rld'`     |

To keep the result, assign it:

```python
s = s.strip()
```

Common beginner mistake:

```python
name = 'asha'
name.upper()          # computed, then thrown away
print(name)           # asha   (still lowercase)
name = name.upper()   # ✅
print(name)           # ASHA
```

---

## 12. Mini Programs

**1. Split an email address**

```python
email = 'rossum@gmail.com'
user, _, domain = email.partition('@')
print(user, domain)           # rossum gmail.com
```

**2. Count words**

```python
sentence = 'the quick brown fox'
print(len(sentence.split()))      # 4
```

**3. Clean a name**

```python
print(' john SMITH '.strip().title())     # John Smith
```

**4. Check a file type**

```python
file = 'photo.PNG'
if file.lower().endswith(('.png', '.jpg')):
    print('image')
```

**5. Zero-padded IDs**

```python
for i in (1, 22, 333):
    print('ID' + str(i).zfill(4))
# ID0001
# ID0022
# ID0333
```

**6. Reverse the words**

```python
print(' '.join('how are you'.split()[::-1]))    # you are how
```

**7. Palindrome ignoring case and spaces**

```python
s = 'Never odd or even'
t = s.replace(' ', '').lower()
print(t == t[::-1])        # True
```

**8. Simple password rules**

```python
pwd = 'Abc12345'
has_digit = any(c.isdigit() for c in pwd)
has_upper = any(c.isupper() for c in pwd)
has_lower = any(c.islower() for c in pwd)
print(len(pwd) >= 8 and has_digit and has_upper and has_lower)    # True
```

---

## 13. Cheat Sheet

| Task                              | Method                          | Returns          |
|-----------------------------------|---------------------------------|------------------|
| Position of first match           | `find`, `index`                 | int              |
| Position of last match            | `rfind`, `rindex`               | int              |
| Number of matches                 | `count`                         | int              |
| Pad right / left / both / zeros   | `ljust`, `rjust`, `center`, `zfill` | str          |
| Remove ends                       | `strip`, `lstrip`, `rstrip`     | str              |
| Substitute text                   | `replace(old, new, count)`      | str              |
| Break into list                   | `split`, `rsplit`, `splitlines` | list             |
| Combine list into string          | `sep.join(iterable)`            | str              |
| Test beginning / end              | `startswith`, `endswith`        | bool             |
| Remove exact prefix / suffix      | `removeprefix`, `removesuffix`  | str              |
| Split in 3 parts                  | `partition`, `rpartition`       | tuple            |
| Change case                       | `capitalize`, `upper`, `lower`, `title`, `swapcase`, `casefold` | str |
| Ask about content                 | `isalpha`, `isdigit`, `isalnum`, `isspace`, `islower`, `isupper`, `istitle`, `isidentifier`, `isprintable`, `isdecimal`, `isnumeric`, `isascii` | bool |

---

## 14. Common Mistakes

| Mistake                                         | Fix                                                |
|-------------------------------------------------|----------------------------------------------------|
| Typos: `isapha`, `findI`, `fing`, `spit`, `startsiwth`, `rnage` | `isalpha`, `find`, `find`, `split`, `startswith` |
| Forgetting quotes: `replace(-,*,2)`             | `replace('-', '*', 2)`                             |
| `ljust(11, .*)`                                 | fill character is one quoted character: `'*'`      |
| Not saving the result: `s.upper()`              | `s = s.upper()`                                    |
| `if s.find('x'):` to test presence              | `if 'x' in s:` or `if s.find('x') != -1:`          |
| Using `index()` on text that may be missing     | use `find()` or `in`                               |
| Expecting `strip('abc')` to remove the text `abc` only | it removes any of the characters a, b, c from the ends |
| Expecting `end` index to be included            | `end` is excluded, like slicing                    |
| Confusing `join`: `s1.join(s2)`                 | the string **before the dot** is the separator     |
| `' '.join([1, 2, 3])`                           | `' '.join(map(str, [1, 2, 3]))`                    |
| `'12.5'.isdigit()` to detect numbers            | use `try: float(s)`                                |
| Case-sensitive search misses: `find('How')`     | `s.lower().find('how')`                            |

---

## 15. Practice Questions

Predict the output first.

```python
S = 'hello how are you'

# 1
print(S.find('o', 5))

# 2
print(S.rfind('o'))

# 3
print(S.count('o'))

# 4
print('a-b-c-d'.replace('-', '', 2))

# 5
print('-'.join('abc'))

# 6
print('a-b-c-d'.rsplit('-', 1))

# 7
print('user@mail.com'.partition('@')[0])

# 8
print('  hi  '.strip() + '!')

# 9
print('42'.zfill(5))

# 10
print('Hello'.swapcase())

# 11
print('\u00b2'.isdigit(), '\u00b2'.isdecimal())

# 12
print('hello world'.title().istitle())

# 13
print('hello'.center(11, '*'))

# 14
print('abc'.index('z'))
```

<details>
<summary>Answers</summary>

1. `7`
2. `15`
3. `3`
4. `abc-d` (the first two `-` are removed)
5. `a-b-c`
6. `['a-b-c', 'd']`
7. `user`
8. `hi!`
9. `00042`
10. `hELLO`
11. `True False`
12. `True`
13. `***hello***`
14. `ValueError: substring not found`

</details>

### Mini challenges

1. Read a sentence and print how many times each vowel appears (use `count`).
2. Find **all** positions of a letter in a string using `find()` with a `start` value.
3. Print a bill: item names left-aligned with dots, prices right-aligned.
4. Take a full name as input and print it in title case, with extra spaces removed.
5. Given `'2024-05-17'`, split it into year, month and day, and join them as `'17/05/2024'`.
6. Check whether an entered string is a valid variable name and not a Python keyword.
7. Validate an email: it must contain exactly one `@`, and end with `.com` or `.in`.
8. Given a file path such as `'C:/docs/report.final.pdf'`, print the file name and the extension using `rpartition`.
