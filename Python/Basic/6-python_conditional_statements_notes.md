# Python Conditional Statements

**Topics:** Relational operators, `if` / `else` / `elif`, Compound conditions with logical operators, Truth tables, Bitwise operators, Membership operators

---

## 1. Why Do We Need Conditional Statements?

Normally a program runs **line by line from top to bottom**. A **conditional statement** lets the program **make decisions**: it runs a block of code **only if a condition is true**.

Real-life example: *If it rains, take an umbrella; otherwise, wear sunglasses.*

A condition is built using **relational operators**, which compare values and give a **Boolean result** (`True` or `False`).

---

## 2. Relational (Comparison) Operators

| Operator | Meaning | Example | Result |
|:--------:|---------|---------|:------:|
| `<` | less than | `4 < 5` | `True` |
| `<=` | less than or equal to | `4 <= 5` | `True` |
| `>` | greater than | `4 > 5` | `False` |
| `>=` | greater than or equal to | `5 >= 5` | `True` |
| `==` | equal to | `4 == 4` | `True` |
| `!=` | not equal to | `4 != 5` | `True` |

- The result is always a **Boolean value**: `True` or `False`.

```python
print(4 < 5)       # True
print(4 <= 5)      # True
print(4 <= 4)      # True
print(4 > 5)       # False
print(4 == 5)      # False
print(4 != 5)      # True
print(type(4 < 5)) # <class 'bool'>
```

> **Correction:** `4 <= -> True` in your notes was incomplete. A full example is `4 <= 5` or `4 <= 4`.

### Important points

- **`=` is assignment, `==` is comparison.**

```python
a = 5        # assigns 5 to a
a == 5       # checks whether a is equal to 5 -> True
```

- Do **not** put a space inside an operator. `a > = 0` is a **SyntaxError**; write `a >= 0`.
- Relational operators work on numbers, strings and other types.

```python
print('apple' < 'banana')    # True  (compared alphabetically)
print('a' == 'A')            # False (case-sensitive)
print(10 == 10.0)            # True
```

### Chained comparison (Python special feature)

```python
a = 5
print(0 < a < 10)      # True   (same as 0 < a and a < 10)
```

### Floating-point caution

```python
print(0.1 + 0.2 == 0.3)    # False (tiny precision error)
```

---

## 3. Conditional Statements: `if`, `else`, `elif`

### 3.1 Syntax

**`if` only**

```python
if <condition>:
    statement 1
    statement 2
```

**`if ... else`**

```python
if <condition>:
    statement 1
else:
    statement 2
```

**`if ... elif ... else`**

```python
if <condition1>:
    statement 1
elif <condition2>:
    statement 2
elif <condition3>:
    statement 3
else:
    statement 4
```

### 3.2 Rules

1. A **colon `:`** is required after `if <condition>`, `elif <condition>` and `else`.
2. The statements inside are **indented** (usually **4 spaces**). Indentation is how Python knows which statements belong to the block.
3. All statements in the **same block must have the same indentation**.
4. `else` has **no condition** and comes **last**. It runs when none of the earlier conditions are true.
5. `elif` (short for "else if") is used for **multiple conditions**; you can have as many as needed.
6. Only **one block** of an `if/elif/else` chain is executed.
7. `else` and `elif` are **optional**.

```python
a = 10
if a > 5:
print("big")      # IndentationError: expected an indented block
```

### 3.3 Flow

```
          condition
         /         \
      True         False
        |            |
   if-block      else-block
         \         /
       rest of program
```

---

### 3.4 Program: Check whether a Number is Positive or Negative

```python
a = 10
if a >= 0:
    print("positive")
else:
    print("negative")
```
```
Output:
positive
```

With input from the user:

```python
a = int(input("Enter a number: "))
if a >= 0:
    print("positive")
else:
    print("negative")
```
```
Enter a number: 10
positive
```

> **Corrections:** `a > =0` must be `a >= 0` (no space in the operator). Also, 0 is neither positive nor negative; see the three-way check below.

**Three-way check using `elif`:**

```python
a = int(input("Enter a number: "))

if a > 0:
    print("positive")
elif a < 0:
    print("negative")
else:
    print("zero")
```

---

### 3.5 More Programs

**Even or odd**

```python
n = int(input("Enter a number: "))
if n % 2 == 0:
    print("even")
else:
    print("odd")
```

**Eligible to vote**

```python
age = int(input("Enter age: "))
if age >= 18:
    print("Eligible to vote")
else:
    print("Not eligible")
```

**Grade using `elif`**

```python
marks = int(input("Enter marks: "))

if marks >= 90:
    print("Grade A")
elif marks >= 75:
    print("Grade B")
elif marks >= 50:
    print("Grade C")
else:
    print("Fail")
```
```
Enter marks: 85
Grade B
```

> Conditions are checked **from top to bottom**; the first true one runs and the rest are skipped.

### 3.6 Nested `if`

An `if` inside another `if`.

```python
age = 25
has_id = True

if age >= 18:
    if has_id:
        print("Entry allowed")
    else:
        print("ID required")
else:
    print("Under age")
```

### 3.7 One-line `if-else` (conditional expression)

```python
a = 10
result = "positive" if a >= 0 else "negative"
print(result)       # positive
```

---

## 4. Compound Conditional Statements and Logical Operators

A **compound condition** combines **two or more conditions** into one using **logical operators**.

| Operator | Meaning | Example |
|:--------:|---------|---------|
| `and` | True only if **both** conditions are true | `a > 0 and a < 10` |
| `or` | True if **at least one** condition is true | `a < 0 or a > 100` |
| `not` | **Reverses** the result | `not a == 8` |

> **Correction:** In Python the logical operators are written in **lowercase**: `and`, `or`, `not`. Writing `And`, `OR` or `NOT` gives an error.

### Syntax

```python
if <cond1> and <cond2>:
    ----
else:
    ----
```

> **Correction:** `if <cond1> <cond2>:` is not valid; the two conditions must be joined by a logical operator (`and` / `or`).

### 4.1 `and` Operator and Truth Table

Result is `True` **only when both** conditions are `True`.

| Cond 1 | Cond 2 | Cond1 `and` Cond2 |
|:------:|:------:|:-----------------:|
| T | T | **T** |
| T | F | F |
| F | T | F |
| F | F | F |

**Example: largest check**

```python
a = 5
b = 7
c = 3

print(a > b and a > c)      # False
```

```
a > b  ->  5 > 7  ->  False
a > c  ->  5 > 3  ->  True
False and True  ->  False
```

### 4.2 `or` Operator and Truth Table

Result is `True` when **at least one** condition is `True`.

| Cond 1 | Cond 2 | Cond1 `or` Cond2 |
|:------:|:------:|:----------------:|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | **F** |

```python
a = 5
print(a < 0 or a > 3)       # True   (second condition is true)
```

### 4.3 `not` Operator and Truth Table

Reverses the condition: `True` becomes `False` and `False` becomes `True`.

| Cond | `not` Cond |
|:----:|:----------:|
| T | F |
| F | T |

```python
if not <condition>:
    ----
else:
    ----
```

```python
a = 8
print(not a == 8)        # False
```

```
a == 8      ->  True
not True    ->  False
```

> **Correction:** `not a === 8` in your notes is invalid; Python has only `==` (two equals signs).

### 4.4 Examples

```python
a = 8
print(a > 0 and a < 10)    # True  (T and T)
print(a > 0 or a > 100)    # True  (T or F)
print(not a == 8)          # False
print(a != 8)              # False (same as not a == 8)
```

### 4.5 Programs using Logical Operators

**Largest of three numbers**

```python
a = 5
b = 7
c = 3

if a > b and a > c:
    print("a is largest")
elif b > c:
    print("b is largest")
else:
    print("c is largest")
```
```
Output:
b is largest
```

**Number in range**

```python
a = int(input("Enter a number: "))
if a > 0 and a < 10:        # or: if 0 < a < 10:
    print("Between 1 and 9")
else:
    print("Out of range")
```

**Leap year**

A year is a leap year if it is divisible by 4 but not by 100, or it is divisible by 400.

```python
year = int(input("Enter year: "))
if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
    print("Leap year")
else:
    print("Not a leap year")
```
```
2024 -> Leap year      1900 -> Not a leap year      2000 -> Leap year
```

**Login check**

```python
user = input("User: ")
pwd = input("Password: ")
if user == "admin" and pwd == "1234":
    print("Welcome")
else:
    print("Invalid login")
```

### 4.6 Precedence of Logical Operators

Order (highest to lowest): **`not`**, then **`and`**, then **`or`**. Relational operators are evaluated **before** all of them. Use parentheses for clarity.

```python
print(True or False and False)      # True   (and first: False and False = False; then True or False)
print((True or False) and False)    # False
```

### 4.7 Short-Circuit Evaluation

- In `A and B`, if **A is False**, Python does **not** check B (the answer is already False).
- In `A or B`, if **A is True**, Python does **not** check B.

```python
x = 0
print(x != 0 and 10 / x > 1)    # False  (no ZeroDivisionError, because 10/x is never evaluated)
```

`and` / `or` actually return one of the **operands**, not always `True`/`False`:

```python
print(0 or 5)        # 5
print(3 and 7)       # 7
print("" or "default")   # default
```

---

## 5. Bitwise Operators

Work on the **individual bits** (binary digits) of integers.

| Operator | Name | Rule |
|:--------:|------|------|
| `&` | Bitwise AND | 1 only if **both** bits are 1 |
| `\|` | Bitwise OR | 1 if **at least one** bit is 1 |
| `^` | Bitwise XOR | 1 if the bits are **different** |
| `~` | Bitwise NOT | flips every bit (`~x = -(x+1)`) |
| `<<` | Left shift | shifts bits left (multiplies by 2 each shift) |
| `>>` | Right shift | shifts bits right (divides by 2 each shift) |

**Truth table (bits)**

| A | B | `A & B` | `A \| B` | `A ^ B` |
|:-:|:-:|:-------:|:--------:|:-------:|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |

```python
a = 5      # 101
b = 3      # 011

print(a & b)     # 1    (001)
print(a | b)     # 7    (111)
print(a ^ b)     # 6    (110)
print(~a)        # -6
print(a << 1)    # 10   (1010)
print(a >> 1)    # 2    (10)
```

> `and`/`or`/`not` work on **whole conditions** (True/False). `&`/`|`/`~` work on the **bits** of numbers.

---

## 6. Membership Operators

Check whether a value is **present in** a sequence or collection (string, list, tuple, set, dictionary).

| Operator | Meaning |
|:--------:|---------|
| `in` | True if the value **is found** |
| `not in` | True if the value **is not found** |

```python
nums = [10, 20, 30]
print(20 in nums)          # True
print(25 in nums)          # False
print(25 not in nums)      # True

print('a' in 'apple')      # True
print('z' not in 'apple')  # True

d = {'name': 'john', 'roll': 125}
print('name' in d)         # True  (checks keys)
print('john' in d)         # False
```

**Use in a condition**

```python
fruits = ["apple", "banana", "mango"]
item = input("Enter fruit: ")

if item in fruits:
    print("Available")
else:
    print("Not available")
```

```python
ch = input("Enter a letter: ")
if ch.lower() in "aeiou":
    print("Vowel")
else:
    print("Not a vowel")
```

---

## 7. Operators Summary

| Category | Operators | Result |
|----------|-----------|--------|
| Relational | `<  <=  >  >=  ==  !=` | `True` / `False` |
| Logical | `and  or  not` | `True` / `False` (or an operand) |
| Bitwise | `&  \|  ^  ~  <<  >>` | integer |
| Membership | `in  not in` | `True` / `False` |

---

## 8. Quick Revision Points

- Conditional statements make the program **decide** which block to run.
- Relational operators: `<  <=  >  >=  ==  !=`; the result is `True` or `False`.
- `=` assigns; `==` compares. No spaces inside operators (`>=`, not `> =`).
- Syntax: `if cond:` / `elif cond:` / `else:`, with a **colon** and **indented** block.
- Only **one** block in an `if-elif-else` chain runs; `else` has no condition and is last.
- A **compound condition** joins conditions with `and`, `or`, `not` (lowercase).
- **`and`:** True only if both are True. **`or`:** False only if both are False. **`not`:** reverses.
- Precedence: relational, then `not`, then `and`, then `or`.
- `and` / `or` use **short-circuit** evaluation.
- Bitwise operators `& | ^ ~ << >>` work on bits; membership operators `in` / `not in` check presence in a collection.
- Chained comparison: `0 < a < 10`.
