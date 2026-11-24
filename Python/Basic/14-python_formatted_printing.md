# Python Formatted Printing
### ASCII, Unicode, Escape Sequences, `print()` and C-Style Printing
### Beginner → Advanced Notes

---

## Table of Contents
1. [ASCII Code](#1-ascii-code)
2. [ord() and chr()](#2-ord-and-chr)
3. [Unicode and UTF Encodings](#3-unicode-and-utf-encodings)
4. [Escape Sequences](#4-escape-sequences)
5. [Raw Strings](#5-raw-strings)
6. [The print() Function](#6-the-print-function)
7. [C-Style Printing (the % Operator)](#7-c-style-printing-the--operator)
8. [Preview: str.format() and f-strings](#8-preview-strformat-and-f-strings)
9. [Mini Programs](#9-mini-programs)
10. [Cheat Sheet](#10-cheat-sheet)
11. [Common Mistakes](#11-common-mistakes)
12. [Practice Questions](#12-practice-questions)

---

## 1. ASCII Code

### Beginner level

**ASCII** = **A**merican **S**tandard **C**ode for **I**nformation **I**nterchange.

Computers store only numbers, so every character is given a **number (code)**. ASCII is the oldest and simplest such table.

| Fact                        | Value                                          |
|-----------------------------|------------------------------------------------|
| Bits used                   | **7 bits**                                     |
| Number of codes             | 2⁷ = **128** (codes **0 to 127**)              |
| Extended ASCII (8 bits)     | 256 codes (0 to 255)                           |

> 📝 Your note said "0 → 217". The standard ASCII range is **0 → 127** (7 bits). The 8-bit "extended" versions go up to 255.

### The codes you must remember

| Characters      | Codes        | Notes                                    |
|-----------------|--------------|------------------------------------------|
| `0` to `9`      | **48 to 57** | `'0'` = 48, `'9'` = 57                   |
| `A` to `Z`      | **65 to 90** | `'A'` = 65, `'Z'` = 90                   |
| `a` to `z`      | **97 to 122**| `'a'` = 97, `'z'` = 122                  |
| space           | 32           |                                          |

Order: **digits < uppercase < lowercase**. This is why `'Z' < 'a'` is `True` (90 < 97).

### The full layout

| Range     | What it contains                                 |
|-----------|--------------------------------------------------|
| 0 – 31    | control characters (not printable): newline = 10, tab = 9, ... |
| 32        | space                                            |
| 33 – 47   | punctuation: `! " # $ % & ' ( ) * + , - . /`     |
| 48 – 57   | digits `0` – `9`                                 |
| 58 – 64   | punctuation: `: ; < = > ? @`                     |
| 65 – 90   | `A` – `Z`                                        |
| 91 – 96   | punctuation: `[ \ ] ^ _ `` ` ``                  |
| 97 – 122  | `a` – `z`                                        |
| 123 – 126 | punctuation: `{ \| } ~`                          |
| 127       | DEL (control character)                          |

### Bicameral languages

A **bicameral** script has **two cases** (uppercase and lowercase).

| Type           | Meaning              | Examples                            |
|----------------|----------------------|-------------------------------------|
| **Bicameral**  | two cases            | English, Greek, Cyrillic (Russian)  |
| **Unicameral** | only one case        | Hindi (Devanagari), Arabic, Chinese, Tamil |

In ASCII, the lowercase code is always the uppercase code **+ 32**:

```
'A' = 65     'a' = 97      97 - 65 = 32
'Z' = 90     'z' = 122    122 - 90 = 32
```

### Intermediate level: useful tricks

```python
ch = 'q'

print(ord(ch) - 32)               # 81  -> code of 'Q'
print(chr(ord(ch) - 32))          # Q   (lower to upper)
print(chr(ord('G') + 32))         # g   (upper to lower)
print(ord('7') - ord('0'))        # 7   (digit character to number)
```

Check what kind of character it is using codes (this is **chaining comparison**):

```python
ch = 'M'
if 65 <= ord(ch) <= 90:
    print("uppercase letter")
elif 97 <= ord(ch) <= 122:
    print("lowercase letter")
elif 48 <= ord(ch) <= 57:
    print("digit")
```

Or compare the characters directly:

```python
if 'A' <= ch <= 'Z':
    print("uppercase letter")
```

### Advanced level: bit trick

Codes of upper and lower letters differ only in **bit 5** (value 32), so XOR with 32 toggles the case:

```python
print(chr(ord('a') ^ 32))     # A
print(chr(ord('A') ^ 32))     # a
```

Works **only for letters**.

---

## 2. ord() and chr()

| Function  | Direction                    | Example             | Result  |
|-----------|------------------------------|---------------------|---------|
| `ord(c)`  | **character → code number**  | `ord('A')`          | `65`    |
| `chr(n)`  | **code number → character**  | `chr(65)`           | `'A'`   |

```python
print(ord('A'))       # 65
print(ord('a'))       # 97
print(ord('0'))       # 48
print(chr(90))        # Z
print(chr(122))       # z
print(hex(ord('A')))  # 0x41
```

`ord()` accepts **exactly one character**:

```python
print(ord('AB'))      # TypeError: ord() expected a character, but string of length 2 found
```

### Print the alphabet with codes

```python
for code in range(65, 91):
    print(code, chr(code))
```

```
65 A
66 B
...
90 Z
```

### Works beyond ASCII (Unicode)

```python
print(ord('€'))       # 8364
print(ord('अ'))       # 2309
print(ord('😀'))      # 128512
print(chr(8364))      # €
```

---

## 3. Unicode and UTF Encodings

### Beginner level

ASCII has room for only 128 characters, which is enough for English but not for the world's languages.

**Unicode** (universal code) is **one big table that combines the characters of all languages** (and symbols, emoji, ...). Each character gets a unique number called a **code point**, written like `U+0041`.

| Fact                       | Value                                    |
|----------------------------|------------------------------------------|
| Code point range           | `U+0000` to `U+10FFFF`                   |
| Characters defined         | well over 150,000                        |
| First 128 code points      | **identical to ASCII**                   |
| Python 3 `str`             | a sequence of **Unicode code points**    |

```python
print('A', 'अ', '中', 'α', '€', '😀')
print(len('😀'))          # 1   (one character, one code point)
```

### Encodings: UTF-8, UTF-16, UTF-32

A code point is just a number. An **encoding** decides **how that number is stored as bytes** in memory or in a file.

| Encoding   | Code unit | Bytes per character                           | Notes                                   |
|------------|-----------|-----------------------------------------------|-----------------------------------------|
| **UTF-8**  | 8 bits    | **1 to 4 bytes** (variable)                   | most popular; ASCII-compatible; web standard |
| **UTF-16** | 16 bits   | **2 or 4 bytes** (variable)                   | used by Windows, Java, JavaScript       |
| **UTF-32** | 32 bits   | **always 4 bytes** (fixed)                    | simple but wastes space                 |

> 📝 Your note said "8 bit UTF-8, 16 bit UTF-16, 32 bit UTF-32". That describes the **code unit size**. Only UTF-32 is truly fixed-width. UTF-8 and UTF-16 use more than one unit for bigger characters.

### How UTF-8 decides the size

| Code point range      | Bytes | Examples                  |
|-----------------------|-------|---------------------------|
| `U+0000 – U+007F`     | 1     | `A`, `z`, `5` (same as ASCII) |
| `U+0080 – U+07FF`     | 2     | `é`, `ñ`, `α`             |
| `U+0800 – U+FFFF`     | 3     | `€`, `अ`, `中`            |
| `U+10000 – U+10FFFF`  | 4     | `😀`                      |

### Intermediate level: encode and decode

```python
print('A'.encode('utf-8'))        # b'A'
print('é'.encode('utf-8'))        # b'\xc3\xa9'
print('€'.encode('utf-8'))        # b'\xe2\x82\xac'
print('😀'.encode('utf-8'))       # b'\xf0\x9f\x98\x80'

print(b'\xe2\x82\xac'.decode('utf-8'))     # €
```

Sizes in bytes:

```python
for ch in ['A', 'é', '€', '😀']:
    print(ch,
          len(ch.encode('utf-8')),
          len(ch.encode('utf-16-le')),
          len(ch.encode('utf-32-le')))
```

| Character | UTF-8 | UTF-16 | UTF-32 |
|-----------|-------|--------|--------|
| `A`       | 1     | 2      | 4      |
| `é`       | 2     | 2      | 4      |
| `€`       | 3     | 2      | 4      |
| `😀`      | 4     | 4      | 4      |

### Advanced level

- **`str` vs `bytes`:** `str` = text (code points), `bytes` = raw data. Convert with `encode()` / `decode()`.
- **Errors:** `'é'.encode('ascii')` → `UnicodeEncodeError`. Handle with `errors='ignore'` or `errors='replace'`.
- **Mojibake:** decoding bytes with the wrong encoding gives garbage like `Ã©`.
- Python source files are **UTF-8 by default**. Always pass `encoding='utf-8'` to `open()` for text files.
- Look up names: `import unicodedata; unicodedata.name('α')` → `'GREEK SMALL LETTER ALPHA'`.

---

## 4. Escape Sequences

### Beginner level

An **escape sequence** is a **backslash `\` followed by a character**. Together they represent **one special character**.

```python
print('Hello\nWorld')
```

```
Hello
World
```

An escape sequence is **one character** in memory: `len('a\nb')` is `3`.

### Table of escape sequences

| Escape      | Meaning                          | ASCII code | Notes                                |
|-------------|----------------------------------|------------|--------------------------------------|
| `\n`        | **new line** (line feed)         | 10         | moves to the next line               |
| `\r`        | **carriage return**              | 13         | cursor back to start of the line     |
| `\t`        | **horizontal tab**               | 9          | jumps to the next tab stop           |
| `\f`        | form feed                        | 12         | page break; terminal-dependent       |
| `\v`        | vertical tab                     | 11         | terminal-dependent                   |
| `\b`        | backspace                        | 8          | cursor moves back one position       |
| `\a`        | alert (bell)                     | 7          | may beep, or nothing                 |
| `\\`        | a backslash `\`                  | 92         |                                      |
| `\'`        | single quote `'`                 | 39         |                                      |
| `\"`        | double quote `"`                 | 34         |                                      |
| `\ooo`      | character with **octal** value (1 to 3 digits) | |  `\101` is `A`                       |
| `\xhh`      | character with **2-digit hex** value | |  `\x41` is `A`                       |
| `\uxxxx`    | Unicode character, **4-digit hex** | |  `\u0905` is `अ`                     |
| `\Uxxxxxxxx`| Unicode character, **8-digit hex** | |  `\U0001F600` is `😀`                |
| `\N{name}`  | Unicode character by **name**    |            | `\N{COPYRIGHT SIGN}` is `©`          |

> 📝 Corrections to your notes: the octal form is `\ooo` (not `\o`); the double-quote escape is `\"` (not `\''`); and hex codes begin with a **backslash** (`\xhh`, `\uxxxx`), not a forward slash.

### The common ones in action

```python
print('Hello\nWorld')            # new line
print('Name:\tAsha')             # tab
print('It\'s fine')              # It's fine
print("She said \"hi\"")         # She said "hi"
print('C:\\Users\\Asha')         # C:\Users\Asha
```

### `\t`: tab stops

A tab moves to the **next multiple of 8** (in most terminals).

```python
print('hello\tworld')    # hello   world      (3 spaces: from column 5 to column 8)
print('a\tb')            # a       b          (7 spaces)
```

### `\r`: carriage return

The cursor returns to the **start of the line**. Later characters **overwrite** earlier ones.

```python
print('abcde\rXY')       # XYcde   (in a terminal)
```

Used for live progress counters (see the mini programs).

### The terminal-dependent ones: `\f`, `\v`, `\b`, `\a`

```python
print('step1\fstep2')
print('hello\tworld')
print('hello\vworld')
print('hello\bworld')
print('hello\aworld')
```

> 📝 In your notes the last line had mismatched quotes (`"hello\aworld'`). Start and end quotes must match.

What you see depends on **where you run the code**:

| Escape | Real terminal / console                                   | IDLE, Jupyter, some editors                |
|--------|-----------------------------------------------------------|--------------------------------------------|
| `\f`   | often moves **down one line** without going back to the left (staircase effect) | may show a symbol (like `♀`) or a box |
| `\v`   | same as `\f`                                              | may show a symbol or box                   |
| `\b`   | cursor moves back one place, so `hello\bworld` appears as `hellworld` | may show a box, or print both  |
| `\a`   | the computer may **beep**; nothing is shown               | usually nothing or a symbol                |

Use `repr()` to see exactly what is stored, whatever the screen shows:

```python
print(repr('hello\bworld'))     # 'hello\x08world'
print(repr('step1\fstep2'))     # 'step1\x0cstep2'
```

### Numeric escapes

```python
print('\x41')                 # A       (hex 41 = 65)
print('\101')                 # A       (octal 101 = 65)
print('\x48\x69')             # Hi
print('\u0041')               # A
print('\u0905')               # अ
print('\U0001F600')           # 😀
print('\N{COPYRIGHT SIGN}')   # ©
print('\N{GREEK SMALL LETTER ALPHA}')   # α
```

| Form          | Digits needed    | Example      |
|---------------|------------------|--------------|
| `\xhh`        | exactly **2** hex| `\x41`       |
| `\uxxxx`      | exactly **4** hex| `\u20ac` (€) |
| `\Uxxxxxxxx`  | exactly **8** hex| `\U0001F600` |
| `\ooo`        | 1 to 3 octal     | `\101`       |

### Advanced level

**Line continuation:** a backslash at the very end of a source line joins it with the next.

```python
s = "hello \
world"
print(s)               # hello world
```

**Invalid escapes:** an unknown escape such as `'\d'` is kept as typed, but Python warns (`SyntaxWarning: invalid escape sequence`). Use a raw string instead.

**ANSI colour codes** use the escape character `\x1b` (code 27):

```python
print('\x1b[31mRed text\x1b[0m normal text')    # works in most modern terminals
```

**Triple quotes** keep real line breaks, so `\n` is not needed:

```python
print("""Line 1
Line 2""")
```

---

## 5. Raw Strings

Prefix a string with **`r`** to switch off escape processing. Every backslash is kept as a normal character.

```python
print('C:\new\table')       # \n and \t are interpreted -> messy output
print(r'C:\new\table')      # C:\new\table
print(len(r'\n'))           # 2   (backslash and n)
print(len('\n'))            # 1
```

Typical uses: Windows file paths and regular expressions (`r'\d+'`).

> ⚠️ A raw string **cannot end with a single backslash**: `r'abc\'` is a syntax error. Use `r'abc' + '\\'` instead.

---

## 6. The print() Function

### Beginner level

`print()` displays values on the screen.

```python
print('Hello')
print(10, 3.5, True)
print('Total:', 25)
```

### Signature (full form)

```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

| Parameter  | Meaning                                              | Default           |
|------------|------------------------------------------------------|-------------------|
| `*objects` | **one or more values** to print (any type)           | none (prints a blank line) |
| `sep`      | **separator** put **between** the values             | `' '` (one space) |
| `end`      | text added **after the last** value                  | `'\n'` (new line) |
| `file`     | where to send the output                             | `sys.stdout` (the screen) |
| `flush`    | force the output to appear immediately               | `False`           |

> 📝 Corrections: the default separator is **one space** `' '` (not an empty string), the last value is `False` (capital F), and `sys.stdout` needs `import sys` first. `sep`, `end`, `file`, `flush` are **keyword arguments**: you must write the name (`sep='-'`).

### `sep`: separator between values

```python
print('a', 'b', 'c')               # a b c
print('a', 'b', 'c', sep='-')      # a-b-c
print('a', 'b', 'c', sep='')       # abc
print('a', 'b', 'c', sep='\n')     # each on its own line
print(10, 20, 30, sep=', ')        # 10, 20, 30
print('2024', '05', '17', sep='/') # 2024/05/17
```

### `end`: what comes at the end of the line

By default each `print()` ends with a **new line**:

```python
print('hello')
print('world')
```

```
hello
world
```

Change `end` to stay on the **same line**:

```python
print('hello', end=' ')
print('world')                      # hello world

print('hello', end='')
print('world')                      # helloworld

for i in range(1, 6):
    print(i, end=' ')               # 1 2 3 4 5
print()                             # moves to the next line
```

### `sep` and `end` together

```python
print('a', 'b', 'c', sep='-', end='!\n')       # a-b-c!
```

### Blank line

```python
print()           # prints an empty line
print('')         # same
```

### Intermediate level

**Unpacking a list or string with `*`:**

```python
nums = [1, 2, 3]
print(nums)               # [1, 2, 3]
print(*nums)              # 1 2 3
print(*nums, sep=', ')    # 1, 2, 3
print(*'abc', sep='-')    # a-b-c
```

**`file`: send output somewhere else**

```python
import sys

print('Normal message')                      # screen (stdout)
print('Error message', file=sys.stderr)      # error stream

with open('log.txt', 'w') as f:
    print('Saved to file', file=f)           # writes into log.txt
```

**`flush`: show output immediately**

Python usually **buffers** output (collects it before showing it). `flush=True` pushes it out at once. This matters for progress displays and long-running programs.

```python
import time

for i in range(0, 101, 25):
    print(f'\rLoading {i}%', end='', flush=True)
    time.sleep(0.5)
print()
```

### Advanced level

- `print()` **returns `None`**: `x = print('hi')` makes `x` equal to `None`.
- Each value is converted with `str()`. User classes can control this through `__str__`.
- `sys.stdout.write('hi\n')` is similar to print, but takes **only a string**, does **not add** a newline, and returns the number of characters written.
- Capture print output in a variable:

```python
import io, contextlib

buf = io.StringIO()
with contextlib.redirect_stdout(buf):
    print('captured')
print(buf.getvalue())      # captured\n
```

- In Python 2, `print` was a statement (`print "hi"`). In Python 3 it is a **function**, so brackets are required.

---

## 7. C-Style Printing (the % Operator)

### Beginner level

The `%` operator formats strings the way `printf` does in C.

```python
"format string" % (value1, value2, ...)
```

The **format string** contains **placeholders** that start with `%`. Each placeholder is replaced by a value, in order.

```python
name = 'Asha'
age = 21
print('My name is %s and I am %d years old' % (name, age))
```

```
My name is Asha and I am 21 years old
```

- `%s` → replaced by `name`
- `%d` → replaced by `age`
- The values go in a **tuple** after the `%` operator. For a single value, brackets are optional: `'Hi %s' % name`.

### Format specifiers (conversion types)

| Specifier | Meaning                            | Example                       | Result            |
|-----------|------------------------------------|-------------------------------|-------------------|
| `%d`, `%i`| signed **integer**                 | `'%d' % 42`                   | `'42'`            |
| `%f`      | **float** (6 decimals by default)  | `'%f' % 3.14159`              | `'3.141590'`      |
| `%s`      | **string** (any object via `str()`)| `'%s' % 'hi'`                 | `'hi'`            |
| `%r`      | `repr()` of the object             | `'%r' % 'hi'`                 | `"'hi'"`          |
| `%c`      | a **single character** (from a code or a 1-char string) | `'%c' % 65`     | `'A'`             |
| `%x`      | integer in **hex** (lowercase)     | `'%x' % 255`                  | `'ff'`            |
| `%X`      | integer in **hex** (uppercase)     | `'%X' % 255`                  | `'FF'`            |
| `%o`      | integer in **octal**               | `'%o' % 8`                    | `'10'`            |
| `%e`      | **scientific** notation            | `'%e' % 12345.678`            | `'1.234568e+04'`  |
| `%E`      | scientific, uppercase              | `'%E' % 12345.678`            | `'1.234568E+04'`  |
| `%g`      | shorter of `%f` / `%e`             | `'%g' % 123.456`              | `'123.456'`       |
| `%%`      | a literal percent sign             | `'%d%%' % 50`                 | `'50%'`           |

> There is **no `%b`** for binary in the `%` style. Use `format(n, 'b')` or `bin(n)`.

### Width, alignment, padding and precision

General form of a placeholder:

```
%[flags][width][.precision]type
```

| Part         | Meaning                                                    |
|--------------|------------------------------------------------------------|
| `width`      | **minimum** number of characters (pads if shorter)         |
| `.precision` | decimals for floats, **maximum characters** for strings    |
| flag `-`     | **left-align** (default is right-align for numbers/strings)|
| flag `0`     | pad numbers with **zeros** instead of spaces               |
| flag `+`     | always show the **sign**                                   |
| flag space   | a space in front of positive numbers                       |
| flag `#`     | alternate form (`0x` for hex, `0o` for octal)              |

#### Integers

```python
print('%d'    % 42)       # 42
print('%5d'   % 42)       #    42      (width 5, right-aligned)
print('%-5d'  % 42)       # 42         (left-aligned; 3 trailing spaces)
print('%05d'  % 42)       # 00042
print('%+d'   % 42)       # +42
print('%+d'   % -42)      # -42
print('%d'    % 3.9)      # 3           (truncates, does not round)
```

#### Floats

```python
print('%f'     % 3.14159)     # 3.141590
print('%.2f'   % 3.14159)     # 3.14         (2 decimals, rounded)
print('%.0f'   % 2.7)         # 3
print('%8.3f'  % 3.14159)     #    3.142     (width 8, 3 decimals)
print('%-8.3f' % 3.14159)     # 3.142        (left-aligned, trailing spaces)
print('%08.3f' % 3.14159)     # 0003.142     (zero padded)
print('%.1e'   % 12345.678)   # 1.2e+04
```

#### Strings

```python
print('%s'     % 'hello')      # hello
print('%10s'   % 'hello')      #      hello      (right-aligned in 10)
print('%-10s'  % 'hello')      # hello           (left-aligned in 10)
print('%.3s'   % 'python')     # pyt             (precision cuts the string)
print('%s'     % 3.5)          # 3.5             (%s accepts any type)
```

#### Characters, hex, octal

```python
print('%c' % 65)            # A
print('%c' % 'Z')           # Z
print('%x' % 255)           # ff
print('%X' % 255)           # FF
print('%#x' % 255)          # 0xff
print('%o' % 8)             # 10
print('%#o' % 8)            # 0o10
```

#### Width taken from the arguments

```python
print('%*d' % (6, 42))          #     42   (width 6 supplied as an argument)
print('%-*d]' % (6, 42))        # 42    ]
```

### Several values

```python
print('%s is %d years old and scored %.1f%%' % ('Asha', 21, 88.456))
# Asha is 21 years old and scored 88.5%
```

Always match the **number** of placeholders with the number of values.

### A neat table

```python
print('%-10s %5s %8s' % ('Name', 'Qty', 'Price'))
print('%-10s %5d %8.2f' % ('Pen', 10, 5.5))
print('%-10s %5d %8.2f' % ('Notebook', 3, 45.0))
```

```
Name         Qty    Price
Pen           10     5.50
Notebook       3    45.00
```

### Intermediate level: dictionary-style placeholders

Use `%(key)type` and pass a **dictionary**.

```python
student = {'name': 'Asha', 'marks': 92}
print('%(name)s scored %(marks)d' % student)     # Asha scored 92
```

### Errors to know

```python
print('%d' % 'five')           # TypeError: %d format: a real number is required, not str
print('%s %s' % ('a',))        # TypeError: not enough arguments for format string
print('%s' % ('a', 'b'))       # TypeError: not all arguments converted during string formatting
```

### Advanced level: the tuple trap

If your **single value is itself a tuple**, wrap it in another tuple.

```python
point = (3, 4)
print('Point: %s' % (point,))      # Point: (3, 4)
print('Point: %s' % point)         # TypeError (Python thinks you gave 2 values)
```

### `%` style vs the newer styles

| Style         | Example                                     | Status                          |
|---------------|---------------------------------------------|---------------------------------|
| `%` (C-style) | `'%s is %d' % (name, age)`                  | old but still supported; common in older code and logging |
| `str.format()`| `'{} is {}'.format(name, age)`              | newer                           |
| f-string      | `f'{name} is {age}'`                        | **recommended** (Python 3.6+)   |

---

## 8. Preview: str.format() and f-strings

(Covered in detail in the next section; here is a quick comparison.)

```python
name = 'Asha'
price = 1234567.891
```

| Task                  | `%` style            | `format()`                      | f-string                    |
|-----------------------|----------------------|---------------------------------|-----------------------------|
| insert values         | `'%s'  % name`       | `'{}'.format(name)`             | `f'{name}'`                 |
| 2 decimals            | `'%.2f' % 3.14159`   | `'{:.2f}'.format(3.14159)`      | `f'{3.14159:.2f}'` → `3.14` |
| width 10, right       | `'%10s' % name`      | `'{:>10}'.format(name)`         | `f'{name:>10}'`             |
| left-align            | `'%-10s' % name`     | `'{:<10}'.format(name)`         | `f'{name:<10}'`             |
| centre                | not available        | `'{:^10}'.format(name)`         | `f'{name:^10}'`             |
| zero pad              | `'%05d' % 42`        | `'{:05d}'.format(42)`           | `f'{42:05d}'` → `00042`     |
| binary                | not available        | `'{:b}'.format(10)`             | `f'{10:b}'` → `1010`        |
| thousands separator   | not available        | `'{:,}'.format(1234567)`        | `f'{1234567:,}'` → `1,234,567` |
| percentage            | not available        | `'{:.1%}'.format(0.256)`        | `f'{0.256:.1%}'` → `25.6%`  |

More f-string features:

```python
x = 5
print(f'{x=}')               # x=5     (debugging helper, Python 3.8+)
print(f'{255:#x}')           # 0xff
print(f'{"hi":*^9}')         # ***hi****  (centre; the extra fill goes on the right)
print(f'{{literal braces}}') # {literal braces}
```

---

## 9. Mini Programs

**1. ASCII table for capital letters**

```python
for code in range(65, 91):
    print('%d -> %s' % (code, chr(code)))
```

**2. Convert to uppercase without `upper()`**

```python
def to_upper(s):
    result = ''
    for ch in s:
        if 'a' <= ch <= 'z':
            result += chr(ord(ch) - 32)
        else:
            result += ch
    return result

print(to_upper('hello World 1'))     # HELLO WORLD 1
```

**3. Caesar cipher (shift by 3)**

```python
text = 'abc xyz'
out = ''
for ch in text:
    if 'a' <= ch <= 'z':
        out += chr((ord(ch) - 97 + 3) % 26 + 97)
    else:
        out += ch
print(out)           # def abc
```

**4. Multiplication table using `%`**

```python
n = 3
for i in range(1, 6):
    print('%d x %d = %2d' % (n, i, n * i))
```

```
3 x 1 =  3
3 x 2 =  6
3 x 3 =  9
3 x 4 = 12
3 x 5 = 15
```

**5. A bill with aligned columns**

```python
items = [('Pen', 2, 5.0), ('Notebook', 3, 45.5), ('Bag', 1, 399.0)]
print('%-10s %3s %9s' % ('Item', 'Qty', 'Amount'))
total = 0
for name, qty, price in items:
    amount = qty * price
    total += amount
    print('%-10s %3d %9.2f' % (name, qty, amount))
print('%-10s %3s %9.2f' % ('Total', '', total))
```

**6. Print a list on one line, comma separated**

```python
nums = [10, 20, 30]
print(*nums, sep=', ')           # 10, 20, 30
```

**7. A simple box with Unicode characters**

```python
text = 'Hello'.center(11)
print('\u250c' + '\u2500' * 11 + '\u2510')
print('\u2502' + text + '\u2502')
print('\u2514' + '\u2500' * 11 + '\u2518')
```

```
┌───────────┐
│   Hello   │
└───────────┘
```

**8. Overwriting progress line with `\r`**

```python
import time
for pct in range(0, 101, 20):
    print('\rProgress: %3d%%' % pct, end='', flush=True)
    time.sleep(0.3)
print()
```

---

## 10. Cheat Sheet

```text
ASCII          7 bits, codes 0-127.  '0'-'9' = 48-57, 'A'-'Z' = 65-90, 'a'-'z' = 97-122
Case gap       lowercase = uppercase + 32
ord / chr      ord('A') = 65      chr(65) = 'A'
Unicode        one code point for every character of every language (U+0000 to U+10FFFF)
UTF-8          1-4 bytes per char     UTF-16: 2 or 4     UTF-32: always 4

Escapes        \n newline   \t tab   \r carriage return   \b backspace   \a bell
               \f form feed   \v vertical tab   \\ backslash   \' \" quotes
               \ooo octal   \xhh hex   \uxxxx   \Uxxxxxxxx   \N{name}
Raw string     r'C:\new\table'   (backslashes kept)

print()        print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

% formatting  %d int   %f float   %s string   %c char   %x %X hex   %o octal
               %e %E scientific   %g general   %r repr   %% percent sign
               %[flags][width][.precision]type
               flags:  -  left   0 zero-pad   + sign   # alternate form
```

---

## 11. Common Mistakes

| Mistake                                          | Fix                                                |
|--------------------------------------------------|----------------------------------------------------|
| Saying ASCII goes up to 217                      | standard ASCII is 0 to 127 (7 bits)                |
| `ord('AB')`                                      | `ord()` takes exactly one character                |
| `\o`, `\''`, `/xhh` for escapes                  | `\ooo`, `\"`, `\xhh` (always a backslash)          |
| Mismatched quotes: `"hello\aworld'`              | use the same quote at both ends                    |
| `print('C:\new\table')`                          | use a raw string `r'C:\new\table'` or `\\`         |
| `r'abc\'`                                        | raw strings cannot end with one backslash          |
| Expecting `sep=''` to be the default             | default is `sep=' '`                               |
| `flush = false`                                  | `flush=False` (capital F)                          |
| Using `sys.stdout` without `import sys`          | add `import sys`                                   |
| `print('a', sep='-')` hoping it adds dashes      | `sep` only works between **two or more** values    |
| Forgetting `print()` after a loop that used `end=' '` | add `print()` to move to the next line        |
| `'%d' % 3.9` expecting `4`                       | `%d` truncates; use `'%.0f' % 3.9` to round        |
| Wrong number of values for `%` placeholders      | match counts; use a tuple                          |
| `'%s' % my_tuple`                                | wrap it: `'%s' % (my_tuple,)`                      |
| Trying `%b` for binary                           | use `format(n, 'b')` or `bin(n)`                   |
| Forgetting `%%` for a literal percent sign       | write `'%d%%' % 50`                                |
| Expecting `\f`, `\v`, `\b`, `\a` to look the same everywhere | they depend on the terminal; test with `repr()` |

---

## 12. Practice Questions

Predict the output first.

```python
# 1
print('a', 'b', 'c', sep='-')

# 2
print('hi', end='!')
print('there')

# 3
print(ord('a') - ord('A'))

# 4
print(chr(ord('A') + 2))

# 5
print(len('a\tb\n'))

# 6
print('%05.1f' % 3.14159)

# 7
print('%-6s]%6s]' % ('ab', 'cd'))

# 8
print('%d%%' % 75)

# 9
print(len('😀'), len('😀'.encode('utf-8')))

# 10
print(r'a\nb')

# 11
print(1, 2, 3, sep='')

# 12
print('%x %o %c' % (255, 8, 66))

# 13
print('%.3s' % 'python')

# 14
print('%d' % 3.9)
```

<details>
<summary>Answers</summary>

1. `a-b-c`
2. `hi!there`
3. `32`
4. `C`
5. `4` (a, tab, b, newline)
6. `003.1` (width 5, one decimal, zero padded)
7. `ab    ]    cd]`
8. `75%`
9. `1 4`
10. `a\nb` (raw string keeps the backslash and the `n`)
11. `123`
12. `ff 10 B`
13. `pyt`
14. `3` (truncated, not rounded)

</details>

### Mini challenges

1. Print the ASCII codes of all lowercase letters, five per line, using `end` and `%`.
2. Convert a string to **swapcase** using only `ord()` and `chr()`.
3. Input a character and report whether it is uppercase, lowercase, digit or special, using ASCII ranges.
4. Print a multiplication table (1 to 10) of any number with aligned columns using `%`.
5. Print the same number in decimal, octal, hex and binary using `%d`, `%o`, `%x` and `format(n, 'b')`.
6. Print a table of products (name, quantity, price) with left-aligned names and right-aligned prices, then a total line.
7. Show the UTF-8, UTF-16 and UTF-32 byte sizes of a word containing English, Hindi and emoji characters.
8. Write a countdown (10 to 0) that overwrites the same line using `\r` and `flush=True`.
