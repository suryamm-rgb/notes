# Python Operators and Expressions

**Topics:** Types of operators, Arithmetic operators, Expressions, Operator precedence, Associativity, Parentheses, Input from keyboard, Programs using formulas (area, unit conversion, displacement, cuboid, quadratic roots)

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

| Kind    | Operands | Example            |
| ------- | :------: | ------------------ |
| Unary   |    1     | `-a`               |
| Binary  |    2     | `a + b`            |
| Ternary |    3     | `x if cond else y` |

---

## 2. Types of Operators in Python

| #   | Type                        | Operators                                           | Purpose                                               |
| --- | --------------------------- | --------------------------------------------------- | ----------------------------------------------------- |
| 1   | **Arithmetic**              | `+  -  *  /  //  %  **`                             | Mathematical calculations                             |
| 2   | **Assignment**              | `=  +=  -=  *=  /=  //=  %=  **=`                   | Assign / update values                                |
| 3   | **Unary minus**             | `-`                                                 | Change the sign of a number                           |
| 4   | **Relational** (comparison) | `==  !=  >  <  >=  <=`                              | Compare values, result is `True`/`False`              |
| 5   | **Logical**                 | `and  or  not`                                      | Combine conditions                                    |
| 6   | **Bitwise**                 | `&  \|  ^  ~  <<  >>`                               | Work on individual bits                               |
| 7   | **Membership**              | `in  not in`                                        | Check whether a value is present in a collection      |
| 8   | **Identity**                | `is  is not`                                        | Check whether two names refer to the same object      |
| 9   | **Special**                 | (identity and membership are commonly grouped here) | Special-purpose checks                                |
| 10  | **Mathematical**            | `math` module functions                             | `sqrt()`, `pow()`, `floor()`, `ceil()`, `factorial()` |

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

| Operator | Name                   | Example   | Result |
| :------: | ---------------------- | --------- | :----: |
|   `+`    | Addition               | `10 + 20` |  `30`  |
|   `-`    | Subtraction            | `20 - 10` |  `10`  |
|   `*`    | Multiplication         | `10 * 20` | `200`  |
|   `/`    | Division               | `14 / 4`  | `3.5`  |
|   `//`   | Floor division         | `14 // 4` |  `3`   |
|   `%`    | Modulus (remainder)    | `14 % 4`  |  `2`   |
|   `**`   | Exponentiation (power) | `2 ** 5`  |  `32`  |

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

|  Priority   |      Operator       | Name                                              |   Associativity   | Rank (from notes) |
| :---------: | :-----------------: | ------------------------------------------------- | :---------------: | :---------------: |
| 1 (highest) |        `( )`        | Parentheses                                       |   Left to Right   |        18         |
|      2      |        `**`         | Exponentiation                                    | **Right to Left** |        14         |
|      3      | `*`, `/`, `//`, `%` | Multiplication, division, floor division, modulus |   Left to Right   |        11         |
| 4 (lowest)  |      `+`, `-`       | Addition, subtraction                             |   Left to Right   |        10         |

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

| Associativity     | Operators                             |
| ----------------- | ------------------------------------- |
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

## 8. Input from the Keyboard

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

### Reading two values in one line

```python
a, b = map(int, input("Enter two numbers: ").split())
print(a + b)
```

```
Enter two numbers: 10 20
30
```

---

## 9. Programs Using Formulas

**Steps to solve any formula-based program:**

1. Read the input values (or assign fixed values).
2. Write the formula as a Python expression (use `*` for multiplication and **brackets** to keep the order right).
3. Print the result.

---

### 9.1 Area of a Rectangle

Formula: `area = length x breadth`

**With fixed values**

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

**With input from the user**

```python
length = int(input("Enter length: "))
breadth = int(input("Enter breadth: "))
area = length * breadth
print("Area:", area)
```

```
Enter length: 15
Enter breadth: 5
Area: 75
```

> **Corrections:** `legth` is a misspelling of `length` (Python would raise `NameError`, because variable names must match exactly), and no semicolon `;` is needed at the end of a line.

---

### 9.2 Area of a Triangle

Formula: `area = 1/2 x base x height`, or `(base x height) / 2`

```python
b = 10
h = 5
area = b * h / 2
print("Area", area)
```

```
Output:
Area 25.0
```

With input:

```python
b = float(input("Enter base: "))
h = float(input("Enter height: "))
area = (b * h) / 2
print("Area:", area)
```

> The result is `25.0` (a float) because `/` always returns a float. Use `//` only if you want a whole number.

---

### 9.3 Area of a Trapezium

A **trapezium** is a four-sided figure with **one pair of parallel sides** (`a` and `b`). `h` is the **perpendicular distance** between them.

Formula: `area = 1/2 x (a + b) x h`

```python
a = 10          # first parallel side
b = 6           # second parallel side
h = 4           # height

area = 1/2 * (a + b) * h
print("Area", area)
```

```
Output:
Area 32.0
```

```
Step by step:  (a + b) = 16
               1/2 * 16 = 8.0
               8.0 * 4  = 32.0
```

> The brackets around `(a + b)` are essential. Without them, `1/2 * a + b * h` would give a completely different (wrong) result, because `*` is done before `+`.

---

### 9.4 Area of a Circle

Formula: `area = pi x r^2`, where `pi = 3.14159` (approx.)

**Using a fixed value of pi**

```python
pi = 3.14159
r = 5
area = pi * r ** 2
print("Area", area)        # Area 78.53975
```

**Using the `math` module (more accurate)**

```python
import math

r = 5
area = math.pi * r ** 2
print("Area", area)        # Area 78.53981633974483
```

> **Correction:** The formula in your notes, `math.pi ** r * r`, is wrong. That calculates (pi to the power r) x r. The correct versions are:
>
> - `math.pi * r ** 2`
> - `math.pi * r * r`
>
> `**` has higher precedence than `*`, so `r ** 2` is done first and then multiplied by pi.

**Extra:** circumference = `2 * math.pi * r`

```python
import math
r = float(input("Enter radius: "))
print("Area:", math.pi * r ** 2)
print("Circumference:", 2 * math.pi * r)
```

---

### 9.5 Kilometres to Miles

Conversion factors:

```
1 mile = 1.60934 km
1 km   = 0.621371 miles
```

Formula: `miles = 0.621371 x km`

```python
km = 15
miles = 0.621371 * km
print("Miles:", miles)           # about 9.320565
```

With input and tidy output:

```python
km = float(input("Enter distance in km: "))
miles = 0.621371 * km
print("Miles:", round(miles, 2))
```

```
Enter distance in km: 15
Miles: 9.32
```

**Reverse (miles to km):**

```python
miles = 10
km = 1.60934 * miles
print("Km:", km)                 # 16.0934
```

---

### 9.6 Displacement (Distance Moved from Start to End)

**Displacement** is how far an object moves from its **start position to its end position**.

Using the equation of motion `v^2 = u^2 + 2as`, we get:

```
s = (v x v - u x u) / (2 x a)
```

| Symbol | Meaning                  |
| :----: | ------------------------ |
|  `u`   | initial velocity (start) |
|  `v`   | final velocity (end)     |
|  `a`   | acceleration (constant)  |
|  `s`   | displacement             |

```python
u = 4
v = 6
a = 3

d = (v * v - u * u) / (2 * a)
print("Displacement:", d)
```

```
Output:
Displacement: 3.3333333333333335
```

```
Step by step:  v*v = 36,  u*u = 16
               36 - 16 = 20
               2 * a = 6
               20 / 6 = 3.3333...
```

> **Corrections:**
>
> - Write the denominator as `(2 * a)` **with brackets**. `... / 2 * a` would divide by 2 first and then **multiply** by `a`, which is wrong because `/` and `*` have equal precedence and go left to right.
> - Python is case-sensitive: `A` and `a` are different variables. Use the same case everywhere.
> - This formula is valid only for **constant acceleration**.

---

### 9.7 Total Surface Area of a Cuboid

**What is a cuboid?**
A **cuboid** is a **three-dimensional (3D)** solid figure with:

- **6 rectangular faces**
- 12 edges and 8 vertices
- Three measurements: **length (l), breadth (b) and height (h)**
- **Opposite faces are equal** in area (like a box or a brick)

**Total surface area** = the sum of the areas of all six faces.

The six faces form **three pairs** of equal faces:

| Pair of faces                     | Area of each | Total for the pair |
| --------------------------------- | :----------: | :----------------: |
| Front and back (length x height)  |   `l * h`    |    `2 * l * h`     |
| Top and bottom (length x breadth) |   `l * b`    |    `2 * l * b`     |
| Left and right (breadth x height) |   `b * h`    |    `2 * b * h`     |

**Formula:**

```
Total surface area = 2*l*h + 2*l*b + 2*b*h
                   = 2 * (l*h + l*b + b*h)
```

```python
length = 10
breadth = 5
height = 7

area = 2 * (length * height + length * breadth + breadth * height)
print("Total surface area:", area)
```

```
Output:
Total surface area: 310
```

```
Step by step:  l*h = 70,  l*b = 50,  b*h = 35
               70 + 50 + 35 = 155
               2 * 155 = 310
```

**Related formulas:**

```python
volume = length * breadth * height                      # 350
lateral_area = 2 * height * (length + breadth)          # 210 (four side faces only)
```

---

### 9.8 Roots of a Quadratic Equation

A **quadratic equation** has the form:

```
a x^2 + b x + c = 0        (a is not 0)
```

The two roots are given by the **quadratic formula**:

```
x = ( -b  ±  sqrt(b^2 - 4ac) ) / (2a)
```

which gives two roots:

```
r1 = ( -b + sqrt(b^2 - 4ac) ) / (2a)
r2 = ( -b - sqrt(b^2 - 4ac) ) / (2a)
```

`b^2 - 4ac` is called the **discriminant (D)**.

In Python we use `math.sqrt()` for the square root:

```python
import math

a = 1
b = -3
c = 2

r1 = (-b + math.sqrt(b ** 2 - 4 * a * c)) / (2 * a)
r2 = (-b - math.sqrt(b ** 2 - 4 * a * c)) / (2 * a)

print("Root 1:", r1)
print("Root 2:", r2)
```

```
Output:
Root 1: 2.0
Root 2: 1.0
```

```
Step by step (a=1, b=-3, c=2):
D = (-3)^2 - 4*1*2 = 9 - 8 = 1
sqrt(D) = 1
r1 = (3 + 1) / 2 = 2.0
r2 = (3 - 1) / 2 = 1.0
```

**With input from the user:**

```python
import math

a = float(input("Enter a: "))
b = float(input("Enter b: "))
c = float(input("Enter c: "))

d = b ** 2 - 4 * a * c
r1 = (-b + math.sqrt(d)) / (2 * a)
r2 = (-b - math.sqrt(d)) / (2 * a)

print("Roots:", r1, r2)
```

> **Corrections to the formula in your notes:**
>
> - Both roots use **`±`**: one with `+` and one with `-`. The line `x = -b + ...` only gives one root.
> - The multiplication sign is required: `4 * a * c`, not `4 * a c`.
> - **Brackets are essential:** the whole top part `(-b + math.sqrt(...))` and the bottom `(2 * a)` must each be in brackets. Without them, `-b + math.sqrt(...) / 2 * a` gives a wrong answer because of precedence.

**Things to remember (the discriminant):**

| Discriminant `D = b^2 - 4ac` | Roots                                                                |
| ---------------------------- | -------------------------------------------------------------------- |
| `D > 0`                      | Two different real roots                                             |
| `D = 0`                      | Two equal real roots                                                 |
| `D < 0`                      | Complex roots (`math.sqrt()` gives `ValueError`; use `cmath.sqrt()`) |

```python
import cmath
print(cmath.sqrt(-4))      # 2j
```

---

### 9.9 More Practice Programs

**Simple interest**: `SI = (P x R x T) / 100`

```python
p = float(input("Principal: "))
r = float(input("Rate: "))
t = float(input("Time (years): "))
si = (p * r * t) / 100
print("Simple Interest:", si)       # P=1000, R=5, T=2 -> 100.0
```

**Celsius to Fahrenheit**: `F = (C x 9/5) + 32`

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

## 10. Formula Summary

| Program                   | Formula                     | Python expression                        |
| ------------------------- | --------------------------- | ---------------------------------------- |
| Rectangle area            | l x b                       | `length * breadth`                       |
| Triangle area             | (b x h) / 2                 | `b * h / 2`                              |
| Trapezium area            | 1/2 (a + b) h               | `1/2 * (a + b) * h`                      |
| Circle area               | pi r^2                      | `math.pi * r ** 2`                       |
| Circle circumference      | 2 pi r                      | `2 * math.pi * r`                        |
| Km to miles               | 0.621371 x km               | `0.621371 * km`                          |
| Miles to km               | 1.60934 x miles             | `1.60934 * miles`                        |
| Displacement              | (v^2 - u^2) / 2a            | `(v*v - u*u) / (2*a)`                    |
| Cuboid total surface area | 2(lh + lb + bh)             | `2 * (l*h + l*b + b*h)`                  |
| Cuboid volume             | l x b x h                   | `l * b * h`                              |
| Quadratic roots           | (-b ± sqrt(b^2 - 4ac)) / 2a | `(-b + math.sqrt(b**2 - 4*a*c)) / (2*a)` |
| Simple interest           | PRT / 100                   | `p * r * t / 100`                        |

---

## 11. Quick Revision Points

- **Operator** performs an operation; **operands** are the values it works on.
- Types: arithmetic, assignment, unary minus, relational, logical, bitwise, membership, identity, special and mathematical.
- Arithmetic operators: `+  -  *  /  //  %  **`.
- `/` always gives a **float**: `14 / 4 = 3.5`.
- `//` gives the **quotient**: `14 // 4 = 3`.
- `%` gives the **remainder**: `14 % 4 = 2`.
- `**` is power: `2 ** 5 = 32`.
- An **expression** is operands plus operators that evaluate to a value.
- **Precedence** (high to low): `( )`, then `**`, then `* / // %`, then `+ -`.
- **Associativity:** left to right for most operators; **right to left for `**`\*\* and assignment.
- `2 ** 3 ** 2 = 512` (not 64).
- `5 + 2 * 3 - 8 / 2 = 7.0` (not 6).
- Use **parentheses** to force the order, especially around the numerator and denominator in formulas.
- `input()` always returns a **string**; use `int()` or `float()` to convert it.
- Circle area is `math.pi * r ** 2` (not `math.pi ** r * r`).
- Displacement is `(v*v - u*u) / (2*a)`; the denominator needs brackets.
- Cuboid total surface area is `2 * (l*h + l*b + b*h)`.
- Quadratic roots use `±`: one root with `+`, one with `-`, and brackets around both the numerator and `(2*a)`.
