# Python Operators and Expressions

**Topics:** Types of operators, Arithmetic operators, Expressions, Operator precedence, Associativity, Parentheses, Input from keyboard

---

## 1. What is an Operator?

```python
a = 10
b = 20
c = a + b
```

- `a` and `b` are called **operands** (the values the operator works on).
- `+` is called the **operator** (the symbol that performs the operation).
- `c = a + b` is a statement; `a + b` is an **expression**.

### Classification by number of operands

| Kind | Operands | Example |
|------|:--------:|---------|
| Unary | 1 | `-a` |
| Binary | 2 | `a + b` |
| Ternary | 3 | `x if cond else y` |

---

## 2. Types of Operators in Python

| # | Type | Operators | Purpose |
|---|------|-----------|---------|
| 1 | **Arithmetic** | `+  -  *  /  //  %  **` | Mathematical calculations |
| 2 | **Assignment** | `=  +=  -=  *=  /=  //=  %=  **=` | Assign / update values |
| 3 | **Unary minus** | `-` | Change the sign of a number |
| 4 | **Relational** (comparison) | `==  !=  >  <  >=  <=` | Compare values, result is `True`/`False` |
| 5 | **Logical** | `and  or  not` | Combine conditions |
| 6 | **Bitwise** | `&  \|  ^  ~  <<  >>` | Work on individual bits |
| 7 | **Membership** | `in  not in` | Check whether a value is present in a collection |
| 8 | **Identity** | `is  is not` | Check whether two names refer to the same object |
| 9 | **Special** | (identity and membership are commonly grouped here) | Special-purpose checks |
| 10 | **Mathematical** | `math` module functions | `sqrt()`, `pow()`, `floor()`, `ceil()`, `factorial()` |

### Quick examples of each type

```python
# Assignment
x = 10
x += 5            # x = x + 5  -> 15

# Unary minus
a = 7
print(-a)         # -7

# Relational
print(10 > 5)     # True
print(10 == 5)    # False

# Logical
print(10 > 5 and 3 < 1)    # False
print(10 > 5 or 3 < 1)     # True
print(not True)            # False

# Bitwise
print(5 & 3)      # 1   (101 & 011 = 001)
print(5 | 3)      # 7   (101 | 011 = 111)
print(5 << 1)     # 10  (shift left)

# Membership
print(3 in [1, 2, 3])        # True
print('a' not in 'hello')    # True

# Identity
p = [1, 2]
q = p
print(p is q)     # True (same object)

# Mathematical (math module)
import math
print(math.sqrt(16))      # 4.0
print(math.factorial(5))  # 120
```

> Relational, logical, bitwise, membership and identity operators are covered in detail separately. This note focuses on **arithmetic operators** and **expressions**.

---

## 3. Arithmetic Operators

| Operator | Name | Example | Result |
|:--------:|------|---------|:------:|
| `+` | Addition | `10 + 20` | `30` |
| `-` | Subtraction | `20 - 10` | `10` |
| `*` | Multiplication | `10 * 20` | `200` |
| `/` | Division | `14 / 4` | `3.5` |
| `//` | Floor division | `14 // 4` | `3` |
| `%` | Modulus (remainder) | `14 % 4` | `2` |
| `**` | Exponentiation (power) | `2 ** 5` | `32` |

### 3.1 Addition, Subtraction, Multiplication

```python
a = 10
b = 20

print(a + b)    # 30
print(a - b)    # -10
print(a * b)    # 200
```

All three follow the same pattern: `a` and `b` are the operands and the symbol is the operator.

### 3.2 Division `/`

- Always returns a **float**, even if the result is a whole number.

```python
print(14 / 4)     # 3.5
print(10 / 2)     # 5.0   (float, not 5)
```

### 3.3 Floor Division `//` and Modulus `%`

```python
a = 14
b = 4

print(a / b)     # 3.5   true division (with decimal part)
print(a // b)    # 3     floor division (quotient only, decimal part dropped)
print(a % b)     # 2     modulus (remainder)
```

**How it works:** 14 = (4 x 3) + 2, so the **quotient is 3** and the **remainder is 2**.

```
a = (b x a//b) + (a % b)
14 = (4 x 3) + 2
```

- `//` gives the quotient rounded **down** (towards negative infinity).
- `%` gives the remainder.

```python
print(7.5 // 2)     # 3.0   (works with floats, result is float)
print(-14 // 4)     # -4    (rounds down, not towards zero)
print(-14 % 4)      # 2
print(divmod(14, 4))   # (3, 2)  -> quotient and remainder together
```

**Common uses of `%`:**

```python
print(10 % 2 == 0)     # True  -> even number check
print(17 % 5)          # 2
print(1234 % 10)       # 4     -> last digit of a number
```

### 3.4 Exponentiation (Power) `**`

```python
print(2 ** 5)      # 32   -> 2 x 2 x 2 x 2 x 2
print(5 ** 2)      # 25
print(9 ** 0.5)    # 3.0  -> square root
print(2 ** -1)     # 0.5
```

### 3.5 Division by zero

```python
print(10 / 0)      # ZeroDivisionError
print(10 // 0)     # ZeroDivisionError
print(10 % 0)      # ZeroDivisionError
```

### 3.6 Operators with strings (bonus)

```python
print("Hello " + "World")    # Hello World  (concatenation)
print("Hi" * 3)              # HiHiHi       (repetition)
```

---

## 4. Expressions

### What is an expression?

An **expression** is a combination of **operands and operators** that Python **evaluates to produce a value**.

```python
10 + 20          # expression -> 30
a * b - 5        # expression
x = a + b * c    # assignment statement containing the expression a + b * c
```

When an expression has **more than one operator**, Python needs rules to decide the order:

1. **Precedence** of operators: which operator is done first
2. **Associativity** of operators: the direction when operators have the same precedence
3. **Parentheses** `()`: used to force the order we want

---

## 5. Precedence of Operators

Operators with **higher precedence** are evaluated **first**.

| Priority | Operator | Name | Associativity | Rank (from notes) |
|:--------:|:--------:|------|:-------------:|:-----------------:|
| 1 (highest) | `( )` | Parentheses | Left to Right | 18 |
| 2 | `**` | Exponentiation | **Right to Left** | 14 |
| 3 | `*`, `/`, `//`, `%` | Multiplication, division, floor division, modulus | Left to Right | 11 |
| 4 (lowest) | `+`, `-` | Addition, subtraction | Left to Right | 10 |

> The "rank" numbers (18, 14, 11, 10) come from your course notes; they only show relative order. A bigger number means higher precedence.
>
> Note: the unary minus (`-x`) sits **below** `**` but **above** `*` and `/`. So `-2 ** 2` is `-(2 ** 2)` = `-4`.

### Example 1

```python
x = 2 + 3 * 5
```
```
= 2 + (3 * 5)        multiplication first
= 2 + 15
= 17
```

### Example 2

```python
x = 5 + 2 * 3 - 8 / 2
```
```
Step 1: 2 * 3 = 6        (multiplication)
Step 2: 8 / 2 = 4.0      (division)
Step 3: 5 + 6 = 11       (left to right)
Step 4: 11 - 4.0 = 7.0   (left to right)
```
```python
print(x)    # 7.0
```

> **Correction:** In your original notes, `10 - 4 = 6` was written, but `5 + 6` is **11**, not 10. So the correct answer is **7.0**. It is a float (`7.0`, not `7`) because `/` always returns a float.

---

## 6. Associativity of Operators

When two operators have the **same precedence**, **associativity** decides the direction of evaluation.

| Associativity | Operators |
|---------------|-----------|
| **Left to Right** | `*  /  //  %  +  -` (and most others) |
| **Right to Left** | `**`, assignment `=`, unary operators |

### Left to right example

```python
x = 20 - 5 - 3
# (20 - 5) - 3 = 12     (NOT 20 - (5 - 3) = 18)

y = 100 / 10 * 2
# (100 / 10) * 2 = 20.0
```

### Right to left example: exponentiation

```python
x = 2 ** 3 ** 2
```
```
= 2 ** (3 ** 2)      right to left: do 3 ** 2 first
= 2 ** 9
= 512
```
```python
print(2 ** 3 ** 2)        # 512
print((2 ** 3) ** 2)      # 64  (parentheses change the result)
```

### Right to left example: assignment

```python
a = b = c = 5     # c = 5 first, then b = c, then a = b
```

---

## 7. Importance of Parentheses `( )`

- Parentheses have the **highest precedence**.
- They **override** the default order and make the expression **easier to read**.

```python
x = (5 + 2) * (6 - 4) / 2
```
```
Step 1: (5 + 2) = 7
Step 2: (6 - 4) = 2
Step 3: 7 * 2   = 14
Step 4: 14 / 2  = 7.0
```
```python
print(x)    # 7.0
```

**Without vs with parentheses:**

```python
print(2 + 3 * 4)       # 14
print((2 + 3) * 4)     # 20
```

> **Tip:** Even when not required, use parentheses to make the intention clear. Nested parentheses are evaluated from the **innermost** outwards.

---

## 8. Programs

### 8.1 Area of a rectangle (fixed values)

Formula: `area = length x breadth`

```python
length = 15
breadth = 5
area = length * breadth
print("Area", area)
```
```
Output:
Area 75
```

> **Corrections:** the variable was misspelled `legth` in one place (Python would give a `NameError`), and no semicolon `;` is needed at the end of a line in Python.

---

## 9. Input from the Keyboard

### The `input()` function

```python
x = input("Enter Data: ")
print(x)
```

- `input()` shows the message (prompt), waits for the user to type, and returns the typed value.
- **`input()` always returns a string**, even if the user types a number.

```python
x = input("Enter a number: ")     # user types 25
print(type(x))                    # <class 'str'>
print(x * 2)                      # 2525   (string repetition, not 50!)
```

### Converting the input to a number

Wrap `input()` with `int()` or `float()`:

```python
x = int(input("Enter a number: "))      # user types 25
print(x * 2)                            # 50
```

### 8.2 Area of a rectangle (input from the user)

```python
length = int(input("Enter length: "))
breadth = int(input("Enter breadth: "))
area = length * breadth
print("Area:", area)
```
```
Sample run:
Enter length: 15
Enter breadth: 5
Area: 75
```

Use `float()` if decimal values are needed:

```python
length = float(input("Enter length: "))
breadth = float(input("Enter breadth: "))
print("Area:", length * breadth)
```

### Reading two values in one line

```python
a, b = map(int, input("Enter two numbers: ").split())
print(a + b)
```
```
Enter two numbers: 10 20
30
```

### More practice programs

**Simple interest** `SI = (P x R x T) / 100`

```python
p = float(input("Principal: "))
r = float(input("Rate: "))
t = float(input("Time (years): "))
si = (p * r * t) / 100
print("Simple Interest:", si)       # P=1000, R=5, T=2 -> 100.0
```

**Area of a circle** `A = pi x r^2`

```python
r = float(input("Radius: "))
area = 3.14159 * r ** 2
print("Area:", area)                # r=5 -> 78.53975
```

**Celsius to Fahrenheit** `F = (C x 9/5) + 32`

```python
c = float(input("Celsius: "))
f = (c * 9 / 5) + 32
print("Fahrenheit:", f)             # c=37 -> 98.6
```

**Swap two numbers**

```python
a = int(input("a: "))
b = int(input("b: "))
a, b = b, a
print(a, b)
```

---

## 10. Quick Revision Points

- **Operator** performs an operation; **operands** are the values it works on.
- Types: arithmetic, assignment, unary minus, relational, logical, bitwise, membership, identity, special and mathematical.
- Arithmetic operators: `+  -  *  /  //  %  **`.
- `/` always gives a **float**: `14 / 4 = 3.5`.
- `//` gives the **quotient**: `14 // 4 = 3`.
- `%` gives the **remainder**: `14 % 4 = 2`.
- `**` is power: `2 ** 5 = 32`.
- An **expression** is operands plus operators that evaluate to a value.
- **Precedence** (high to low): `( )`, then `**`, then `* / // %`, then `+ -`.
- **Associativity:** left to right for most operators; **right to left for `**`** and assignment.
- `2 ** 3 ** 2 = 512` (not 64).
- `5 + 2 * 3 - 8 / 2 = 7.0` (not 6).
- Use **parentheses** to force the order and make code clearer.
- `input()` always returns a **string**; use `int()` or `float()` to convert it before calculating.
