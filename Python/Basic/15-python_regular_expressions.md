# Python Regular Expressions (`re` module)
### Beginner → Advanced Notes

---

## Table of Contents
1. [What is Pattern Matching?](#1-what-is-pattern-matching)
2. [The re Module](#2-the-re-module)
3. [Functions of the re Module](#3-functions-of-the-re-module)
4. [Quantifiers](#4-quantifiers)
5. [Special Characters](#5-special-characters)
6. [Sequences (Escape Sequences)](#6-sequences-escape-sequences)
7. [Practical Patterns (Name, Variable, Time, Domain, Date, Password, Email)](#7-practical-patterns)
8. [Groups and Alternation](#8-groups-and-alternation)
9. [Greedy vs Lazy Matching](#9-greedy-vs-lazy-matching)
10. [Flags](#10-flags)
11. [Advanced: Lookaround and Backreferences](#11-advanced-lookaround-and-backreferences)
12. [Mini Programs](#12-mini-programs)
13. [Cheat Sheet](#13-cheat-sheet)
14. [Common Mistakes](#14-common-mistakes)
15. [Practice Questions](#15-practice-questions)

---

## 1. What is Pattern Matching?

### Beginner level

A **regular expression (regex)** is a **pattern** that describes the *shape* of a piece of text, instead of the exact text.

| Exact text                  | Pattern (idea)                                      |
|-----------------------------|-----------------------------------------------------|
| `31/12/1970`                | "two digits, slash, two digits, slash, four digits" |
| `my.id1@gmail.com`          | "some word characters, `@`, a domain, `.com`"       |
| `abC_1234`                  | "at least 8 letters, digits or underscores"         |

**Pattern matching** = checking whether a string follows a pattern, or finding the parts of a string that follow it.

### Uses of regular expressions

| Use                                  | Meaning                                              | Example                                   |
|--------------------------------------|------------------------------------------------------|-------------------------------------------|
| **Text processing** (search & replace)| find text by pattern and change it                  | replace all extra spaces by one space     |
| **Data validation**                  | check that data is in the **proper format**          | is this a valid email, phone, PIN, date?  |
| **Extracting information**           | pull the needed parts out of a **large text**        | all email addresses in a web page         |
| **Splitting**                        | cut text at patterns                                 | split at commas, semicolons or spaces     |
| **Generating / describing strings**  | a pattern describes a whole **set of strings**       | `[eimt]{4}` describes 256 different strings |

### Regex as a "string generator" (set of strings)

A pattern stands for **every string that matches it**. For example `[eimt]{4}` means "any 4 characters, each chosen from e, i, m, t":

```python
from itertools import product

words = [''.join(p) for p in product('eimt', repeat=4)]
print(len(words))                    # 256  (4 x 4 x 4 x 4)
print('item' in words)               # True
print('eeee' in words)               # True
print(words[:5])                     # ['eeee', 'eeei', 'eeem', 'eeet', 'eeie']
```

The real words `item`, `emit`, `mite`, `time` are four of those 256 strings.

---

## 2. The re Module

### Beginner level

A **module** is a ready-made **library** (a file of code) that you **import**. Regular expressions live in the built-in module **`re`**.

```python
import re
```

### Other ways to import

```python
import re as regex          # use a nickname: regex.match(...)
from re import match        # import only what you need: match(...)
from re import *            # import everything (not recommended)
```

> ⚠️ `from re import *` dumps names like `compile`, `match`, `search`, `split` into your program. This can **hide built-in functions** (for example `compile`) and makes code hard to read. Prefer `import re`.

### Always write patterns as raw strings

Prefix the pattern with **`r`** so backslashes reach the regex engine untouched.

```python
re.search(r'\d+', 'abc 123')       # ✅ raw string
re.search('\d+', 'abc 123')         # ⚠️ works, but gives a SyntaxWarning (invalid escape sequence)
re.search('\bcat\b', 'a cat')       # ❌ '\b' is a BACKSPACE character in a normal string
re.search(r'\bcat\b', 'a cat')      # ✅ '\b' means "word boundary"
```

---

## 3. Functions of the re Module

### Overview

| Function                                      | Purpose                                          | Returns                          |
|-----------------------------------------------|--------------------------------------------------|----------------------------------|
| `compile(pattern, flags=0)`                   | prepare a pattern for reuse                      | Pattern object                   |
| `match(pattern, string, flags=0)`             | match at the **beginning** of the string         | Match object or `None`           |
| `fullmatch(pattern, string, flags=0)`         | the **whole string** must match                  | Match object or `None`           |
| `search(pattern, string, flags=0)`            | find the **first** match **anywhere**            | Match object or `None`           |
| `findall(pattern, string, flags=0)`           | find **all** matches                             | **list** of strings              |
| `finditer(pattern, string, flags=0)`          | all matches, one by one                          | iterator of Match objects        |
| `split(pattern, string, maxsplit=0, flags=0)` | split the string at matches                      | list                             |
| `sub(pattern, repl, string, count=0, flags=0)`| **replace** matches                              | new string                       |
| `subn(pattern, repl, string, count=0, flags=0)` | like `sub`, also gives the number of replacements | tuple `(string, count)`      |
| `escape(text)`                                | escape special characters in plain text          | string                           |

Grouped by job:

| Job                         | Functions                          |
|-----------------------------|------------------------------------|
| **Validation**              | `match`, `fullmatch`               |
| **Search and extract**      | `search`, `findall`, `finditer`    |
| **Split / replace**         | `split`, `sub`, `subn`             |
| **Reuse**                   | `compile`                          |

> 📝 The parameter of `split` is `maxsplit` (not `maxslip`).

### `match()`: matches only at the start

```python
import re

print(re.match('abc', 'abcef'))      # <re.Match object; span=(0, 3), match='abc'>
print(re.match('abc', 'xabc'))       # None   (does not start with abc)
```

### `fullmatch()`: the entire string must match

```python
print(re.fullmatch('python', 'python'))          # <re.Match object; span=(0, 6), match='python'>
print(re.fullmatch('python', 'python').group())  # 'python'
print(re.fullmatch('abc', 'abcef'))              # None   (extra characters 'ef')
```

### `search()`: first match anywhere

```python
m = re.search('very', 'python is very easy')
print(m)               # <re.Match object; span=(10, 14), match='very'>
print(m.span())        # (10, 14)
```

### `findall()`: every match as a list

```python
print(re.findall('can', 'can you can a can as a canner'))
# ['can', 'can', 'can', 'can']      (the last one is inside 'canner')
```

### The three "matchers" compared

| String           | `match('abc', s)` | `fullmatch('abc', s)` | `search('abc', s)` |
|------------------|-------------------|------------------------|--------------------|
| `'abc'`          | ✅ (0, 3)         | ✅ (0, 3)              | ✅ (0, 3)          |
| `'abcef'`        | ✅ (0, 3)         | ❌ `None`              | ✅ (0, 3)          |
| `'xabc'`         | ❌ `None`         | ❌ `None`              | ✅ (1, 4)          |

```
match      : pattern must be at the START
fullmatch  : pattern must be the WHOLE string
search     : pattern may be ANYWHERE
```

### The Match object

A successful `match`, `fullmatch` or `search` returns a **Match object**. A failure returns **`None`**.

```python
m = re.search(r'\d+', 'abc 123 xyz')

print(m.group())    # '123'    the matched text
print(m.start())    # 4
print(m.end())      # 7        (end is NOT included)
print(m.span())     # (4, 7)
print(m.string)     # 'abc 123 xyz'
```

Because a Match object is always **truthy** and `None` is **falsy**, you can test directly:

```python
if re.fullmatch(r'\d{6}', '560001'):
    print('valid PIN code')
```

> ⚠️ Calling `.group()` on `None` raises `AttributeError`. Check first: `m = re.search(...)`, then `if m: ...`.

### `finditer()`: all matches with positions

```python
for m in re.finditer(r'\d+', 'a1b22c333'):
    print(m.group(), m.span())
# 1 (1, 2)
# 22 (3, 5)
# 333 (6, 9)
```

### `split()`

```python
print(re.split(r'\s+', 'a  b   c'))             # ['a', 'b', 'c']
print(re.split(r'[,;]', 'a,b;c'))               # ['a', 'b', 'c']
print(re.split(r'-', 'a-b-c-d', maxsplit=2))    # ['a', 'b', 'c-d']
```

### `sub()` and `subn()`: search and replace

```python
print(re.sub(r'\d+', '#', 'a1b22c333'))          # a#b#c#
print(re.sub(r'\s+', ' ', 'a   b \n  c'))        # a b c
print(re.sub('a', 'X', 'banana', count=2))       # bXnXna
print(re.subn('a', 'X', 'banana'))               # ('bXnXnX', 3)
```

Using groups in the replacement (`\1`, `\2`):

```python
print(re.sub(r'(\w+)@(\w+)', r'\2 at \1', 'rossum@gmail'))    # gmail at rossum
```

Using a function as the replacement (Advanced):

```python
print(re.sub(r'\d+', lambda m: str(int(m.group()) * 2), 'a1b22'))   # a2b44
```

### `escape()`: treat text literally

```python
print(re.escape('a.b*c'))            # a\.b\*c
print(re.findall(re.escape('1+1'), '1+1=2'))     # ['1+1']
```

### `compile()`: reuse a pattern

```python
pat = re.compile(r'\d+')            # compile once
print(pat.findall('a1b22c333'))     # ['1', '22', '333']
print(pat.search('abc 45').group()) # '45'
print(pat.sub('#', 'a1b22'))        # a#b#
```

A compiled **Pattern object** has the same methods (`match`, `fullmatch`, `search`, `findall`, `finditer`, `split`, `sub`) **without** the pattern argument. Benefits: cleaner code, one place to keep the pattern, and slightly faster in big loops. (Python also caches recently used patterns, so the speed gain is small.)

---

## 4. Quantifiers

A **quantifier** says **how many times** the thing before it can repeat.

| Quantifier | Meaning                          | Applies to |
|------------|----------------------------------|------------|
| `+`        | **1 or more**                    | the item right before it |
| `*`        | **0 or more**                    | " |
| `?`        | **0 or 1** (optional)            | " |
| `{m}`      | **exactly m** times              | " |
| `{m,n}`    | **from m to n** times            | " |
| `{m,}`     | m or more (n is infinity)        | " |
| `{,n}`     | 0 up to n (m defaults to 0)      | " |

> 📝 In your notes `?` was described as "0 or more". The correct meaning is **0 or 1** (`*` is the "0 or more" one).

The item can be a **single character**, a **group** `( )` or a **set** `[ ]`.

### `+` : one or more

```python
re.fullmatch('a+', 'a')            # ✅
re.fullmatch('a+', 'aaaaaaa')      # ✅
re.fullmatch('a+', '')             # ❌ None (needs at least one)
```

Group repeated:

```python
re.fullmatch('(ab)+', 'ab')               # ✅
re.fullmatch('(ab)+', 'ababababababab')   # ✅
re.fullmatch('(ab)+', 'aaaabbb')          # ❌ (not the unit "ab" repeated)
re.fullmatch('(ab)+', 'aba')              # ❌
```

Set repeated: `[abc]+` = one or more characters, **each** being a, b or c.

```python
re.fullmatch('[abc]+', 'abcaaabbbbcccc')  # ✅
re.fullmatch('[abc]+', 'abcd')            # ❌ ('d' is not in the set)
```

### `*` : zero or more

```python
for s in ['a', 'ab', '', 'c', 'aabbcc']:
    print(repr(s), bool(re.fullmatch('[abc]*', s)))
# 'a'  True
# 'ab' True
# ''   True   (zero repetitions is allowed)
# 'c'  True
# 'aabbcc' True
```

### `?` : zero or one (optional)

```python
print(re.fullmatch('(ab)?', ''))        # <re.Match object; span=(0, 0), match=''>
print(re.fullmatch('(ab)?', 'ab'))      # <re.Match object; span=(0, 2), match='ab'>
print(re.fullmatch('(ab)?', 'abab'))    # None   (at most ONE "ab")
print(re.fullmatch('(ab)?', 'abc'))     # None   (fullmatch needs the whole string)
```

Practical use: an optional part.

```python
re.fullmatch(r'colou?r', 'color')       # ✅
re.fullmatch(r'colou?r', 'colour')      # ✅
```

### `{m}` : exactly m

```python
pattern = '[eimt]{4}'
```

This means **4 characters, each one chosen from e, i, m, t**. A character **may repeat**.

| String   | Matches? | Why                              |
|----------|----------|----------------------------------|
| `item`   | ✅       | 4 characters from the set        |
| `emit`   | ✅       |                                  |
| `mite`   | ✅       |                                  |
| `time`   | ✅       |                                  |
| `eimt`   | ✅       |                                  |
| `eeee`   | ✅       | repeating is allowed             |
| `iiii`   | ✅       |                                  |
| `mmmm`   | ✅       |                                  |
| `tim`    | ❌       | only 3 characters                |
| `items`  | ❌       | 5 characters (and `s` not in set)|

> 📝 Your note said "only take once". It is not so: each position can be **any** character from the set, so repeats like `eeee` are fine. To forbid repeats you need other tools (lookahead or checking `len(set(s)) == 4`).

### `{m,n}` : from m to n

```python
re.fullmatch('[eimt]{4,8}', 'time')          # ✅ 4 characters
re.fullmatch('[eimt]{4,8}', 'timetime')      # ✅ 8 characters
re.fullmatch('[eimt]{4,8}', 'timetimet')     # ❌ 9 characters
re.fullmatch('[eimt]{4,8}', 'tim')           # ❌ 3 characters
```

### More examples (fixing the quotes from the notes)

```python
import re

print(re.fullmatch('(ab)?', 'ab'))
print(re.fullmatch('(ab)?', ''))
print(re.fullmatch('(ab)*', 'abababababab'))
print(re.findall('[abc]+', '123 abc 987 bbcc@cbac'))
# ['abc', 'bbcc', 'cbac']
```

### Quantifier summary

| Pattern    | Matches                          | Does not match           |
|------------|----------------------------------|--------------------------|
| `a+`       | `a`, `aa`, `aaa`                 | empty, `b`               |
| `a*`       | empty, `a`, `aa`                 | `b`                      |
| `a?`       | empty, `a`                       | `aa`                     |
| `a{3}`     | `aaa`                            | `aa`, `aaaa`             |
| `a{2,4}`   | `aa`, `aaa`, `aaaa`              | `a`, `aaaaa`             |
| `a{2,}`    | `aa`, `aaa`, `aaaa...`           | `a`                      |

---

## 5. Special Characters

| Symbol    | Meaning                                              |
|-----------|------------------------------------------------------|
| `[...]`   | **set** of possible characters (one character)       |
| `[^...]`  | **all characters except** those in the brackets      |
| `.`       | **any one character** except a new line              |
| `^`       | **beginning** of the string                          |
| `$`       | **end** of the string                                |
| `R\|S`    | **R or S** (alternation)                             |
| `( )`     | group                                                |
| `\`       | escape: turn a special character into a plain one    |

Complete list of the symbols so far: `?  +  *  {m}  [...]  [^...]  .  ^  $  R|S`

### `[...]` : a set

```python
'[abc]'        # a or b or c
'[a-c]'        # same, written as a range
```

| Pattern          | Meaning                              |
|------------------|--------------------------------------|
| `[a-z]`          | any one **lowercase** letter         |
| `[A-Z]`          | any one **uppercase** letter         |
| `[0-9]`          | any one **digit**                    |
| `[a-zA-Z]`       | any letter                           |
| `[a-zA-Z0-9]`    | any letter or digit (alphanumeric)   |
| `[a-z]+`         | a lowercase "dictionary word"        |
| `[A-Z][a-z]+`    | a capitalised word like `Hello`      |

```python
re.fullmatch('[a-z]+', 'hello')          # ✅
re.fullmatch('[a-z]+', 'Hello')          # ❌ (capital H)
re.fullmatch('[A-Z][a-z]+', 'Hello')     # ✅
re.fullmatch('[A-Z][a-z]+', 'hello')     # ❌
re.fullmatch('[a-zA-Z0-9]+', 'abc123')   # ✅
```

Special characters **lose their special meaning inside `[ ]`**:

```python
re.fullmatch('[.$*]+', '.$*')       # ✅ these are plain characters inside a set
```

> ⚠️ **`[A-z]` is a trap.** The range goes from code 65 to 122, which also includes `[ \ ] ^ _` and the backtick. So `[A-z]+` matches `A_b`. Always write `[A-Za-z]`.

```python
print(bool(re.fullmatch('[A-z]+', 'A_b')))       # True   (surprise!)
print(bool(re.fullmatch('[A-Za-z]+', 'A_b')))    # False
```

### `[^...]` : negated set

The `^` **right after `[`** means "anything **except** these".

```python
re.fullmatch('[^a-z]+', 'ABC123')       # ✅ no lowercase letters at all
re.fullmatch('[^a-z]+', 'abc')          # ❌
re.fullmatch('[^0-9]+', 'hello!')       # ✅ no digits
```

So `[^a-z]+` = "a string made only of uppercase letters, digits or special characters".

### `.` : any character except new line

```python
re.fullmatch('.+', 'abc 123 !@#')       # ✅ lowercase, digits, space, special
re.fullmatch('a.c', 'abc')              # ✅
re.fullmatch('a.c', 'a9c')              # ✅
re.fullmatch('a.c', 'ac')               # ❌ (needs exactly one character between)
re.fullmatch('.+', 'a\nb')              # ❌ (new line is not matched by .)
re.fullmatch('.+', 'a\nb', re.DOTALL)   # ✅ with the DOTALL flag
```

### `.` vs `\.` (answer to your doubt)

| Pattern  | Meaning                                  | Matches `'.......'`? | Matches `'abc'`? |
|----------|------------------------------------------|----------------------|------------------|
| `.+`     | one or more of **any character**         | ✅                   | ✅               |
| `\.+`    | one or more of the **literal dot** `.`   | ✅                   | ❌               |

`\.` means "a real dot". Without the backslash, `.` is a wildcard.

```python
re.fullmatch(r'\.+', '.......')     # ✅ only dots
re.fullmatch(r'\.+', 'abc')         # ❌
re.fullmatch(r'a\.c', 'a.c')        # ✅
re.fullmatch(r'a\.c', 'abc')        # ❌ the dot must be a real dot
re.fullmatch(r'a.c',  'abc')        # ✅ . matches b
```

The characters that need a backslash to be taken literally: `. ^ $ * + ? { } [ ] \ | ( )`

### `^` : beginning of the string

```python
re.search('^http', 'http://udemy.com')     # ✅ starts with http
re.search('^http', 'see http://udemy.com') # ❌
re.match('^http.+', 'https://udemy.com')   # ✅ starts with http, then anything
```

> `match()` and `fullmatch()` already start at the beginning, so `^` is needed mainly with `search()` and `findall()`.

### `$` : end of the string

```python
re.search('com$', 'udemy.com')                 # ✅ ends with com
re.search('com$', 'udemy.com.in')              # ❌
re.fullmatch('.+com$', 'udemy.com')            # ✅ anything, ending with com
```

> With `fullmatch()` the `$` is not needed, because the whole string must match anyway.

### `R|S` : alternation (OR)

```python
re.fullmatch('cat|dog', 'dog')        # ✅
re.fullmatch('cat|dog', 'cow')        # ❌
re.fullmatch('(com|org|net)', 'org')  # ✅
```

`|` has the **lowest priority**: `ab|cd` means `ab` **or** `cd`, not `a(b|c)d`. Use brackets to limit it.

```python
re.fullmatch('gr(a|e)y', 'gray')      # ✅
re.fullmatch('gr(a|e)y', 'grey')      # ✅
```

---

## 6. Sequences (Escape Sequences)

Shortcuts for common sets of characters. Always write them in **raw strings** (`r'\d'`).

| Sequence | Meaning                              | Equivalent set            |
|----------|--------------------------------------|---------------------------|
| `\d`     | a **digit**                          | `[0-9]`                   |
| `\D`     | a **non-digit**                      | `[^0-9]`                  |
| `\w`     | a **word character**                 | `[a-zA-Z0-9_]`            |
| `\W`     | a **non-word character**             | `[^a-zA-Z0-9_]`           |
| `\s`     | **whitespace**                       | `[ \t\n\r\f\v]`           |
| `\S`     | **non-whitespace**                   | `[^ \t\n\r\f\v]`          |
| `\b`     | **word boundary** (edge of a word)   | position, no character    |
| `\B`     | not a word boundary                  | position                  |
| `\A`     | **start** of the string              | like `^` (without multi-line effect) |
| `\Z`     | **end** of the string                | like `$` (strict)         |
| `\t \n \r \f` | tab, new line, carriage return, form feed | the real characters |

> 📝 Corrections to your notes:
> - `\D` is **non-digit** `[^0-9]` (not `[a-zA-Z0-9]`).
> - `\w` also includes the **underscore** `_` (so it is `[a-zA-Z0-9_]`).
> - `\W` is non-word, i.e. everything that is **not** `\w`.

```python
print(re.findall(r'\d', 'a1b22'))         # ['1', '2', '2']
print(re.findall(r'\D+', 'a1b22'))        # ['a', 'b']
print(re.findall(r'\w+', 'hi_there, you!'))   # ['hi_there', 'you']
print(re.findall(r'\W', 'a-b c'))         # ['-', ' ']
print(re.split(r'\s+', 'a \t b\nc'))      # ['a', 'b', 'c']
print(re.findall(r'\S+', ' a  bb '))      # ['a', 'bb']
```

### `\b` : word boundary

```python
print(re.findall(r'\bcat\b', 'cat concat cat.'))     # ['cat', 'cat']  (not the one inside concat)
print(re.findall(r'cat', 'cat concat cat.'))         # ['cat', 'cat', 'cat']
print(re.findall(r'\b\w{4}\b', 'this is some text')) # ['this', 'some', 'text']
```

### `\A` and `\Z` vs `^` and `$`

```python
print(bool(re.search(r'com$',  'a.com\n')))    # True   ($ also matches just before a final new line)
print(bool(re.search(r'com\Z', 'a.com\n')))    # False  (\Z is the strict end)
```

> ⚠️ Use capital **`\Z`**. Older Python versions raise an error for lowercase `\z` (newer versions accept it as a synonym), so `\Z` is the safe choice. In your email pattern use `\Z` (or just use `fullmatch`).

### Unicode note

For normal text (`str`) patterns, `\d`, `\w`, `\s` also match **non-English** digits and letters (like `٣` or `अ`). Add `re.ASCII` to restrict them to ASCII:

```python
print(bool(re.fullmatch(r'\w+', 'नमस्ते')))               # True
print(bool(re.fullmatch(r'\w+', 'नमस्ते', re.ASCII)))     # False
```

---

## 7. Practical Patterns

Each pattern below is used with `re.fullmatch()` so the **entire string** must follow it.

### 1. Full name: `Abdul Bari`

```python
pattern = r'[A-Z][a-z]+ [A-Z][a-z]+'
print(bool(re.fullmatch(pattern, 'Abdul Bari')))    # True
print(bool(re.fullmatch(pattern, 'abdul bari')))    # False
```

> 📝 Your version used `[A-z]`. As shown earlier, that range also allows `[ \ ] ^ _` and backtick, so use `[A-Z]`.

### 2. Variable name: `item1`

Rule: starts with a letter or underscore, followed by letters, digits or underscores.

```python
pattern = r'[a-zA-Z_][a-zA-Z0-9_]*'
# the same idea, shorter:  r'[a-zA-Z_]\w*'

for s in ['item1', '_x', 'total_2', '1item', 'my-var']:
    print(s, bool(re.fullmatch(pattern, s)))
# item1 True / _x True / total_2 True / 1item False / my-var False
```

> 📝 Your note had `[a-zA-z_]` (a lowercase `z` at the end of the second range), which is a typo for `[a-zA-Z_]`.
> This checks only the **form**; it does not reject Python keywords like `class`.

### 3. Time: `11:30`

Your draft: `[012][0-9:[0-5][0-9]` (the colon sits inside the bracket by mistake).

```python
simple = r'[012][0-9]:[0-5][0-9]'
strict = r'([01][0-9]|2[0-3]):[0-5][0-9]'

print(bool(re.fullmatch(simple, '11:30')))   # True
print(bool(re.fullmatch(simple, '29:59')))   # True   ← wrong, 29 is not an hour
print(bool(re.fullmatch(strict, '29:59')))   # False
print(bool(re.fullmatch(strict, '23:59')))   # True
```

Hours: `[01][0-9]` covers 00–19, `2[0-3]` covers 20–23. Minutes: `[0-5][0-9]`.

### 4. Domain name: `udemy.com`

```python
pattern = r'[a-zA-Z0-9]+\.(com|org|net)'
print(bool(re.fullmatch(pattern, 'udemy.com')))    # True
print(bool(re.fullmatch(pattern, 'udemy.in')))     # False
print(bool(re.fullmatch(pattern, 'udemycom')))     # False (the dot is required)
```

With the `$` as in your notes: `r'[a-zA-Z0-9]+\.(com|org|net)$'` works with `match()` / `search()`. With subdomains and hyphens: `r'([a-zA-Z0-9-]+\.)+(com|org|net)'` matches `www.my-site.com`.

### 5. Date: `31/12/1970`

Your draft: `'\d{2}/\d{2}/|d{4}'` has `|d` instead of `\d`.

```python
simple = r'\d{2}/\d{2}/\d{4}'
strict = r'(0[1-9]|[12][0-9]|3[01])/(0[1-9]|1[0-2])/\d{4}'

print(bool(re.fullmatch(simple, '31/12/1970')))   # True
print(bool(re.fullmatch(simple, '99/99/9999')))   # True   (format only!)
print(bool(re.fullmatch(strict, '99/99/9999')))   # False
print(bool(re.fullmatch(strict, '31/12/1970')))   # True
```

Regex checks the **format**. It cannot know that `31/02/2024` does not exist. For real dates, use `datetime.strptime`.

### 6. Password: `abC_1234`

Your draft: `[\w_]{8,}`. Since `\w` already includes `_`, it is simply:

```python
pattern = r'\w{8,}'      # 8 or more letters, digits or underscores
print(bool(re.fullmatch(pattern, 'abC_1234')))   # True
print(bool(re.fullmatch(pattern, 'abc123')))     # False (too short)
```

This checks **length and allowed characters only**. For a strong password (at least one lowercase, one uppercase, one digit, one special character) use **lookaheads** (section 11):

```python
strong = r'(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[\W_]).{8,}'
print(bool(re.fullmatch(strong, 'abC_1234')))    # True
print(bool(re.fullmatch(strong, 'abcdefgh')))    # False
```

### 7. Email: `my.id1@gmail.com`

Your draft: `\w+\.?\w+ @\w+\.{com|org)\z` has these problems: a **space** before `@`, `{com|org)` mixes `{` and `)` (it must be `(com|org)`), and `\z` should be `\Z`.

```python
pattern = r'\w+\.?\w+@\w+\.(com|org)'        # fixed version of your pattern
print(bool(re.fullmatch(pattern, 'my.id1@gmail.com')))   # True
print(bool(re.fullmatch(pattern, 'a@gmail.com')))        # False (needs 2+ characters before @)
```

A more practical pattern:

```python
pattern = r'[\w.+-]+@[\w-]+(\.[\w-]+)+'
for s in ['my.id1@gmail.com', 'a@x.co.in', 'bad@', '@bad.com', 'no at.com']:
    print(s, bool(re.fullmatch(pattern, s)))
# my.id1@gmail.com True / a@x.co.in True / bad@ False / @bad.com False / no at.com False
```

> Real-world email rules are very complex. Use regex for a **quick sanity check**, and confirm by sending a verification email.

### 8. Other common formats

| Format               | Pattern                                | Example match     |
|----------------------|----------------------------------------|-------------------|
| PIN code (6 digits)  | `r'\d{6}'`                             | `560001`          |
| Indian mobile number | `r'[6-9]\d{9}'`                        | `9876543210`      |
| Mobile with +91      | `r'(\+91[\s-]?)?[6-9]\d{9}'`           | `+91 9876543210`  |
| Hex colour           | `r'#[0-9a-fA-F]{6}'`                   | `#1a2B3c`         |
| Integer (with sign)  | `r'[+-]?\d+'`                          | `-45`             |
| Decimal number       | `r'[+-]?\d+(\.\d+)?'`                  | `3.14`            |
| Only letters         | `r'[A-Za-z]+'`                         | `Hello`           |
| URL start            | `r'https?://\S+'`                      | `https://a.com/x` |

---

## 8. Groups and Alternation

### Grouping with `( )`

Brackets do three jobs: **group** characters for a quantifier, **limit** an `|`, and **capture** the matched part.

```python
m = re.fullmatch(r'(\d{2})/(\d{2})/(\d{4})', '31/12/1970')

print(m.group())      # '31/12/1970'   whole match (same as group(0))
print(m.group(1))     # '31'
print(m.group(2))     # '12'
print(m.group(3))     # '1970'
print(m.groups())     # ('31', '12', '1970')
```

### Named groups `(?P<name>...)`

```python
m = re.fullmatch(r'(?P<day>\d{2})/(?P<month>\d{2})/(?P<year>\d{4})', '31/12/1970')
print(m.group('day'))        # '31'
print(m['year'])             # '1970'
print(m.groupdict())         # {'day': '31', 'month': '12', 'year': '1970'}
```

### Non-capturing group `(?:...)`

Groups for structure, **without** remembering the text.

```python
print(re.findall(r'(ab)+', 'ababab ab'))     # ['ab', 'ab']       only the group's last text!
print(re.findall(r'(?:ab)+', 'ababab ab'))   # ['ababab', 'ab']   the whole match
```

### `findall()` and groups

| Pattern has...        | `findall()` returns                         |
|-----------------------|---------------------------------------------|
| no groups             | list of the **whole matches**               |
| one group             | list of that **group's text**               |
| two or more groups    | list of **tuples**                          |

```python
print(re.findall(r'(\w+)@(\w+)\.com', 'a@x.com b@y.com'))
# [('a', 'x'), ('b', 'y')]
```

---

## 9. Greedy vs Lazy Matching

Quantifiers are **greedy** by default: they take **as much as possible**. Add **`?`** after a quantifier to make it **lazy** (take as little as possible).

| Greedy | Lazy   |
|--------|--------|
| `*`    | `*?`   |
| `+`    | `+?`   |
| `?`    | `??`   |
| `{m,n}`| `{m,n}?` |

```python
text = '<a><b>'
print(re.findall(r'<.+>', text))       # ['<a><b>']       greedy: one big match
print(re.findall(r'<.+?>', text))      # ['<a>', '<b>']   lazy: smallest matches
print(re.findall(r'<[^>]+>', text))    # ['<a>', '<b>']   negated set (often the best choice)
```

---

## 10. Flags

Flags change how a pattern behaves. Pass them as the last argument, or combine with `|`.

| Flag                  | Short | Effect                                                        |
|-----------------------|-------|---------------------------------------------------------------|
| `re.IGNORECASE`       | `re.I`| ignore upper/lower case                                       |
| `re.MULTILINE`        | `re.M`| `^` and `$` match at the start and end of **each line**       |
| `re.DOTALL`           | `re.S`| `.` also matches a new line                                   |
| `re.VERBOSE`          | `re.X`| allow spaces and `#` comments inside the pattern              |
| `re.ASCII`            | `re.A`| `\w \d \s \b` match ASCII only                                |

```python
print(re.findall('python', 'Python PYTHON python', re.I))
# ['Python', 'PYTHON', 'python']

print(re.findall(r'^\w+', 'one\ntwo\nthree'))          # ['one']
print(re.findall(r'^\w+', 'one\ntwo\nthree', re.M))    # ['one', 'two', 'three']

print(re.fullmatch('a.b', 'a\nb', re.S) is not None)   # True

print(re.findall('abc', 'ABC abc', re.I | re.M))       # combining flags
```

### Verbose patterns (readable regex)

```python
pattern = re.compile(r"""
    (\d{2})   # day
    /
    (\d{2})   # month
    /
    (\d{4})   # year
""", re.VERBOSE)

print(pattern.fullmatch('31/12/1970').groups())    # ('31', '12', '1970')
```

Inline flag: `(?i)python` is the same as using `re.I`.

---

## 11. Advanced: Lookaround and Backreferences

### Lookaround: match based on the surroundings, without consuming them

| Syntax       | Name                 | Meaning                                |
|--------------|----------------------|----------------------------------------|
| `(?=...)`    | lookahead            | followed by `...`                      |
| `(?!...)`    | negative lookahead   | **not** followed by `...`              |
| `(?<=...)`   | lookbehind           | preceded by `...`                      |
| `(?<!...)`   | negative lookbehind  | **not** preceded by `...`              |

```python
print(re.findall(r'\d+(?= dollars)', '10 dollars 20 euros'))   # ['10']
print(re.findall(r'(?<=\$)\d+', 'cost $50 and $7'))            # ['50', '7']
print(re.findall(r'\b\d+\b(?! euros)', '10 dollars 20 euros')) # ['10']
```

Password rule with several lookaheads (each one checks a requirement at the start):

```python
# at least one digit AND at least one uppercase AND length 8+
print(bool(re.fullmatch(r'(?=.*\d)(?=.*[A-Z]).{8,}', 'Hello1234')))   # True
```

### Backreferences: reuse a captured group

`\1` means "the same text that group 1 matched".

```python
print(re.search(r'(\w)\1', 'hello').group())              # 'll'   (a doubled letter)
print(re.findall(r'\b(\w+) \1\b', 'this is is a test test'))   # ['is', 'test']  (repeated words)
```

### Performance warning (catastrophic backtracking)

Patterns with nested quantifiers such as `(a+)+$` can take an enormous time on certain inputs (for example `'aaaaaaaaaaaaaaaaaaaaaaaa!'`). This is called **ReDoS**. For user-supplied input:
- avoid nested repeats on overlapping sets,
- limit the input length,
- prefer specific sets (`[^,]+`) over `.*`.

---

## 12. Mini Programs

**1. Extract all email addresses**

```python
text = 'Contact a.b@x.com or support@site.org today'
print(re.findall(r'[\w.+-]+@[\w-]+(?:\.[\w-]+)+', text))
# ['a.b@x.com', 'support@site.org']
```

**2. Extract numbers (including decimals)**

```python
print(re.findall(r'\d+(?:\.\d+)?', 'Price 45.50 and 3 items, 7.25'))
# ['45.50', '3', '7.25']
```

**3. Remove special characters**

```python
print(re.sub(r'[^A-Za-z0-9 ]+', '', 'Hello, World! 123.'))   # Hello World 123
```

**4. Normalise spaces**

```python
print(re.sub(r'\s+', ' ', '  too    many \n spaces  ').strip())   # too many spaces
```

**5. Word frequency**

```python
from collections import Counter

text = 'the cat and the hat and the bat'
words = re.findall(r'\w+', text.lower())
print(Counter(words).most_common(2))     # [('the', 3), ('and', 2)]
```

**6. Mask a phone number**

```python
print(re.sub(r'\d(?=\d{4})', '*', '9876543210'))     # ******3210
```

**7. Parse a log line using named groups**

```python
line = '2024-05-17 ERROR Disk full'
m = re.match(r'(?P<date>\d{4}-\d{2}-\d{2}) (?P<level>\w+) (?P<msg>.+)', line)
print(m.groupdict())
# {'date': '2024-05-17', 'level': 'ERROR', 'msg': 'Disk full'}
```

**8. Validate several inputs**

```python
checks = {
    'PIN':    (r'\d{6}', '560001'),
    'Mobile': (r'[6-9]\d{9}', '9876543210'),
    'Time':   (r'([01]\d|2[0-3]):[0-5]\d', '23:59'),
}
for label, (pattern, value) in checks.items():
    print(label, bool(re.fullmatch(pattern, value)))
```

**9. camelCase to snake_case (advanced)**

```python
print(re.sub(r'(?<!^)(?=[A-Z])', '_', 'camelCaseString').lower())    # camel_case_string
```

---

## 13. Cheat Sheet

```text
FUNCTIONS    match (start)   fullmatch (whole)   search (anywhere)   findall (list)
             finditer        split               sub / subn          compile   escape

QUANTIFIERS  +  1 or more    *  0 or more    ?  0 or 1
             {m} exactly m   {m,n} m to n    {m,} m or more     add ? for lazy: +? *? ??

SPECIAL      [abc] set   [a-z] range   [^abc] not in set   . any char (not \n)
             ^ start     $ end         R|S  R or S         ( ) group   \ escape

SEQUENCES    \d digit   \D non-digit   \w word [a-zA-Z0-9_]   \W non-word
             \s space   \S non-space   \b word boundary        \A start   \Z end

GROUPS       (...) capture   (?:...) no capture   (?P<name>...) named
LOOKAROUND   (?=...) (?!...) (?<=...) (?<!...)
FLAGS        re.I  re.M  re.S  re.X  re.A

ALWAYS       use raw strings: r'\d+'     test with: if re.fullmatch(...):
```

---

## 14. Common Mistakes

| Mistake                                            | Fix                                                        |
|----------------------------------------------------|------------------------------------------------------------|
| Forgetting `import re`                             | `import re` at the top                                     |
| Pattern without `r''` (`'\bcat\b'`)                | `r'\bcat\b'`                                               |
| Missing closing quote: `re.fullmatch('(ab)?, 'ab')`| `re.fullmatch('(ab)?', 'ab')`                              |
| Using `match()` to find text in the middle         | use `search()` or `findall()`                              |
| Using `match()` for validation of the whole string | use `fullmatch()`                                          |
| Calling `.group()` on `None`                       | `m = re.search(...)` then `if m:`                          |
| Thinking `?` means "0 or more"                     | `?` is 0 or 1; `*` is 0 or more                            |
| Thinking `[eimt]{4}` means each letter once        | letters may repeat (`eeee` matches)                        |
| Using `[A-z]`                                      | use `[A-Za-z]`                                             |
| Typos in ranges: `[a-zA-z_]`                       | `[a-zA-Z_]`                                                |
| `\D` or `\W` described as the wrong set            | `\D` = `[^0-9]`, `\W` = `[^a-zA-Z0-9_]`                    |
| `|d{4}` instead of `\d{4}`                         | a backslash, not a pipe                                    |
| `{com\|org)` mixed brackets                        | `(com\|org)`                                               |
| Spaces inside patterns (`\w+ @\w+`)                | spaces are literal characters; remove them                 |
| Lowercase `\z`                                     | use `\Z`                                                   |
| Unescaped dot in `\w+.com`                         | `\w+\.com` (the dot would match any character)             |
| Greedy `.*` swallowing too much                    | use lazy `.*?` or a negated set `[^>]*`                    |
| Forgetting `findall()` returns tuples when there are groups | use `(?:...)` or read the tuples                  |
| Expecting regex to validate meaning (real dates, real emails) | regex checks the **format** only                |
| Passing `flags` as the 3rd positional argument of `split()` | use `flags=re.I` as a keyword                     |

---

## 15. Practice Questions

Predict the output first (`import re` is assumed).

```python
# 1
print(bool(re.fullmatch(r'\d{3}-\d{4}', '555-1234')))

# 2
print(re.findall(r'\d+', 'a1b22c333'))

# 3
print(re.sub(r'\s+', ' ', 'a   b    c'))

# 4
print(re.match(r'abc', 'xabc'))

# 5
print(re.search(r'abc', 'xabc').span())

# 6
print(re.fullmatch(r'[A-Z][a-z]+', 'Hello') is not None)

# 7
print(re.findall(r'<.+?>', '<a><b>'))

# 8
print(re.split(r'[,;]\s*', 'a, b;c'))

# 9
print(re.fullmatch(r'[^0-9]+', 'abc1'))

# 10
print(re.findall(r'\b\w{4}\b', 'this is some text'))

# 11
print(bool(re.fullmatch('(ab)?', 'abab')))

# 12
print(re.fullmatch(r'a.c', 'a\nc'))

# 13
print(re.sub(r'(\w+) (\w+)', r'\2 \1', 'hello world'))

# 14
print(re.findall(r'[A-Z]', 'Hello World'))

# 15
print(re.findall('can', 'can you can a can as a canner'))
```

<details>
<summary>Answers</summary>

1. `True`
2. `['1', '22', '333']`
3. `a b c`
4. `None` (`match` only checks the start)
5. `(1, 4)`
6. `True`
7. `['<a>', '<b>']`
8. `['a', 'b', 'c']`
9. `None` (the string contains the digit `1`)
10. `['this', 'some', 'text']`
11. `False` (`?` allows at most one `ab`)
12. `None` (`.` does not match a new line)
13. `world hello`
14. `['H', 'W']`
15. `['can', 'can', 'can', 'can']`

</details>

### Mini challenges

1. Validate a **6-digit PIN code** that does not start with `0`.
2. Validate an **Indian mobile number**, with an optional `+91` or `0` in front.
3. Check a **vehicle number** like `KA01AB1234` (2 letters, 2 digits, 1–2 letters, 4 digits).
4. Extract all **hashtags** (`#python`) from a tweet.
5. Extract all **dates** in the format `dd/mm/yyyy` from a paragraph.
6. Find all **repeated words** such as "is is" in a sentence.
7. Replace every run of digits in a string by its length (hint: `re.sub` with a function).
8. Check a strong password: at least 8 characters, one uppercase, one lowercase, one digit and one special character.
9. Convert `snake_case` to `camelCase` using `re.sub`.
10. Read a text file and count how many times each word occurs, ignoring case and punctuation.
