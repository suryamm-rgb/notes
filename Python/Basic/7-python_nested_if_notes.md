# Nested `if`, `if-elif` Ladder and Challenges

**Topics:** Nested if, if-elif ladder, Temperature check, Discounted bill, Day / month / digit to name, Leap year (with history), Practice challenges

---

## 1. Recap: Types of Conditional Structures

| Structure | Use |
|-----------|-----|
| `if` | Run a block only when a condition is true |
| `if ... else` | Choose between **two** paths |
| `if ... elif ... else` (ladder) | Choose between **many** paths |
| **Nested `if`** | A decision **inside** another decision |

---

## 2. Nested `if`

A **nested if** is an `if` statement placed **inside** another `if` (or `else`) block. The inner condition is checked **only if** the outer condition allows it.

### Syntax 1: `if` inside `if`

```python
if <condition1>:
    if <condition2>:
        statement A
    else:
        statement B
else:
    statement C
```

### Syntax 2: `if` inside `else`

```python
if <condition1>:
    statement A
else:
    if <condition2>:
        statement B
    else:
        statement C
```

### Example

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
```
Output:
Entry allowed
```

> **Note:** Indentation decides which `if`/`else` belongs together. Each level of nesting adds **4 more spaces**.

---

## 3. `if-elif` Ladder

When there are **many mutually exclusive choices**, use a ladder instead of deep nesting.

```python
if <condition1>:
    ...
elif <condition2>:
    ...
elif <condition3>:
    ...
else:
    ...
```

- Conditions are checked **from top to bottom**.
- The **first true condition** runs, and the **rest are skipped**.
- `else` runs only if **none** are true (it is optional).

### Nested if vs elif ladder

| Nested if | elif ladder |
|-----------|-------------|
| Inner decision depends on the outer one | Choices are **independent alternatives** |
| Code moves deeper (more indentation) | Code stays flat and readable |
| Example: "adult? then has ID?" | Example: "grade A, B, C or fail" |

> `else if` is **not** Python. Write **`elif`** (one word).

---

## 4. Temperature Check

**Problem:** If the temperature is 25, print `normal`; below 25, print `cold`; above 25, print `hot`.

```
temp == 25  ->  normal
temp <  25  ->  cold
temp >  25  ->  hot
```

### Program (if-elif ladder)

```python
temp = float(input("Enter temperature: "))

if temp == 25:
    print("Normal")
elif temp < 25:
    print("Cold")
else:
    print("Hot")
```
```
Enter temperature: 18
Cold
```

### Same program using nested if

```python
temp = float(input("Enter temperature: "))

if temp == 25:
    print("Normal")
else:
    if temp < 25:
        print("Cold")
    else:
        print("Hot")
```

> **Corrections to your code:**
> - `input("enter temperature')` has **mismatched quotes**; use the same quote at both ends: `input("Enter temperature: ")`.
> - `el if` must be written **`elif`** (no space).
> - The `print()` statements must be **indented** inside each block.
> - `print('hot")` also has mismatched quotes.
> - `else` needs no condition, because "hot" is the only case left.

---

## 5. Generating a Discounted Bill

**Discount** = a percentage reduction in the amount.

```
discount   = amount x discount_percent / 100
final bill = amount - discount
```

### Fixed 15% discount

```python
amount = 1000
amount = amount - amount * 0.15
print("Bill amount:", amount)
```
```
Output:
Bill amount: 850.0
```

```
amount * 0.15 = 1000 * 0.15 = 150.0
1000 - 150.0 = 850.0
```

Shorter: `amount *= 0.85` (pay 85% of the amount).

> **Correction:** variable names in your notes were mixed (`almutn`, `almount`). Python needs the **same name** everywhere, so use `amount`.

### Discount depending on the amount (if-elif ladder)

> The slabs below are an example; change them as the question requires.

```python
amount = float(input("Enter bill amount: "))

if amount >= 5000:
    discount = 0.20
elif amount >= 3000:
    discount = 0.15
elif amount >= 1000:
    discount = 0.10
else:
    discount = 0

final = amount - amount * discount
print("Discount:", amount * discount)
print("Final bill:", final)
```
```
Enter bill amount: 3500
Discount: 525.0
Final bill: 2975.0
```

> **Order matters:** the **largest** slab is checked first. If `amount >= 1000` were first, every large amount would match it and stop there.

---

## 6. Converting a Number to a Name

All three programs use an **if-elif ladder**.

### 6.1 Day number to day name

```python
day = int(input("Enter day number (1-7): "))

if day == 1:
    print("Monday")
elif day == 2:
    print("Tuesday")
elif day == 3:
    print("Wednesday")
elif day == 4:
    print("Thursday")
elif day == 5:
    print("Friday")
elif day == 6:
    print("Saturday")
elif day == 7:
    print("Sunday")
else:
    print("Invalid day number")
```
```
Enter day number (1-7): 3
Wednesday
```

### 6.2 Month number to month name

```python
month = int(input("Enter month number (1-12): "))

if month == 1:
    print("January")
elif month == 2:
    print("February")
elif month == 3:
    print("March")
elif month == 4:
    print("April")
elif month == 5:
    print("May")
elif month == 6:
    print("June")
elif month == 7:
    print("July")
elif month == 8:
    print("August")
elif month == 9:
    print("September")
elif month == 10:
    print("October")
elif month == 11:
    print("November")
elif month == 12:
    print("December")
else:
    print("Invalid month number")
```

### 6.3 Digit to words

```python
d = int(input("Enter a digit (0-9): "))

if d == 0:
    print("Zero")
elif d == 1:
    print("One")
elif d == 2:
    print("Two")
elif d == 3:
    print("Three")
elif d == 4:
    print("Four")
elif d == 5:
    print("Five")
elif d == 6:
    print("Six")
elif d == 7:
    print("Seven")
elif d == 8:
    print("Eight")
elif d == 9:
    print("Nine")
else:
    print("Not a single digit")
```

### Shorter alternatives (for later)

```python
# using a list
months = ["January", "February", "March", "April", "May", "June",
          "July", "August", "September", "October", "November", "December"]
m = int(input("Month number: "))
if 1 <= m <= 12:
    print(months[m - 1])
else:
    print("Invalid month number")

# using match-case (Python 3.10+)
match d:
    case 0: print("Zero")
    case 1: print("One")
    case _: print("Other")
```

---

## 7. Leap Year

### 7.1 What is a leap year?

- A normal year has **365 days** and February has **28 days**.
- A **leap year** has **366 days**, and February has **29 days**.
- The extra day (29 February) is called the **leap day**.

### 7.2 Why do we need leap years?

The Earth takes about **365.2422 days** (roughly **365.242**) to go once around the Sun. A calendar year of exactly 365 days is therefore **short by about 0.2422 day (almost 6 hours) every year**. Without correction, the calendar would slowly drift away from the seasons.

### 7.3 Julian calendar

- Introduced by **Julius Caesar** (45 BC).
- Rule: **every 4th year is a leap year**.
- Average year length: `365 + 1/4 = 365.25` days (one extra day every 4 years: 365 x 4 + 1 = 1461 days, so 1461 / 4 = 365.25).

```python
if year % 4 == 0:      # Julian rule
    print("leap year")
```

**Problem:** 365.25 is slightly **more than** the real 365.2422, so the Julian calendar gains about **11 minutes per year** (about **1 day in 128 years**). By the 1500s the calendar was about **10 days** ahead of the seasons.

### 7.4 Gregorian calendar

- Introduced by **Pope Gregory XIII** in **1582**, and used almost everywhere today.
- It keeps the 4-year rule but adds **two exceptions** to remove the extra days:

| Rule | Effect | Adjustment |
|------|--------|:----------:|
| Divisible by **4** | leap year | `+ 1/4` |
| Divisible by **100** | **not** a leap year (century years) | `- 1/100` |
| Divisible by **400** | **is** a leap year again | `+ 1/400` |

**Average year length:**

```
365 + 1/4 - 1/100 + 1/400
= 365 + 0.25 - 0.01 + 0.0025
= 365.2425 days
```

This is very close to the real value, **365.2422**.

> **Corrections to your notes:** the starting number must be **365** (not 364): `365 + 1/4 - 1/100 + 1/400`. Also `1/400 = 0.0025` (not 0.02). The correct total is **365.2425**.

**Check with 400 years:** there are `100 - 4 + 1 = 97` leap years in 400 years, so the days are `400 x 365 + 97 = 146097`, and `146097 / 400 = 365.2425`.

### 7.5 Leap year rule (Gregorian)

A year is a **leap year** if:
- it is **divisible by 4**, **and not divisible by 100**, **or**
- it is **divisible by 400**.

| Year | Divisible by 4? | by 100? | by 400? | Leap year? |
|:----:|:---------------:|:-------:|:-------:|:----------:|
| 2024 | Yes | No | n/a | **Yes** |
| 2023 | No | n/a | n/a | No |
| 1900 | Yes | Yes | No | **No** |
| 2000 | Yes | Yes | Yes | **Yes** |
| 2100 | Yes | Yes | No | **No** |
| 1600 | Yes | Yes | Yes | **Yes** |

### 7.6 Program 1: if-elif ladder (check 100 first)

```python
year = int(input("Enter year: "))

if year % 100 == 0:
    if year % 400 == 0:
        print("Leap year")
    else:
        print("Not a leap year")
elif year % 4 == 0:
    print("Leap year")
else:
    print("Not a leap year")
```

**How it works:**
1. If the year is a **century year** (divisible by 100), it is a leap year only if divisible by 400.
2. Otherwise, if divisible by 4, it is a leap year.
3. Otherwise, it is not.

> **Corrections to your code:**
> - `==` is comparison; `=` is assignment (`year % 400 = = 0` is invalid).
> - `===` does not exist in Python; use `==`.
> - `el if` must be `elif`.
> - `print()` statements must be indented, and quotes must match: `print("Leap year")`.
> - `"leap here"` and `"not a lipe year"` were typos.

### 7.7 Program 2: nested if (check 4 first)

```python
year = int(input("Enter year: "))

if year % 4 == 0:
    if year % 100 == 0:
        if year % 400 == 0:
            print("Leap year")
        else:
            print("Not a leap year")
    else:
        print("Leap year")
else:
    print("Not a leap year")
```

### 7.8 Program 3: one compound condition

```python
year = int(input("Enter year: "))

if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
    print("Leap year")
else:
    print("Not a leap year")
```

```
2024 -> Leap year       1900 -> Not a leap year
2000 -> Leap year       2023 -> Not a leap year
```

All three programs give the same result for every year.

---

## 8. Challenges (Practice Problems)

### Solved challenge 1: Simple calculator (if-elif with nested if)

```python
a = float(input("Enter first number: "))
b = float(input("Enter second number: "))
op = input("Enter operator (+ - * /): ")

if op == '+':
    print(a + b)
elif op == '-':
    print(a - b)
elif op == '*':
    print(a * b)
elif op == '/':
    if b != 0:                       # nested if
        print(a / b)
    else:
        print("Cannot divide by zero")
else:
    print("Invalid operator")
```

### Solved challenge 2: Nature of roots of a quadratic equation

```python
a = float(input("a: "))
b = float(input("b: "))
c = float(input("c: "))

d = b ** 2 - 4 * a * c          # discriminant

if d > 0:
    print("Two different real roots")
elif d == 0:
    print("Two equal real roots")
else:
    print("Complex roots")
```

### Solved challenge 3: Type of triangle from its sides

```python
a = int(input("Side 1: "))
b = int(input("Side 2: "))
c = int(input("Side 3: "))

if a == b and b == c:
    print("Equilateral")
elif a == b or b == c or a == c:
    print("Isosceles")
else:
    print("Scalene")
```

### Practice problems (try these yourself)

1. Check whether a number is **positive, negative or zero**.
2. Find the **largest of three numbers** (use nested `if`, then `and`).
3. Find the **smallest of three numbers**.
4. Check whether a triangle is **valid** (sum of any two sides must be greater than the third).
5. Grade calculator from marks (A, B, C, D, Fail).
6. **BMI category:** BMI = weight(kg) / height(m)^2, then print underweight, normal, overweight or obese.
7. Check whether a character is a **vowel, consonant, digit or special character**.
8. **Electricity bill** with slab rates (first 100 units at one rate, next 100 at another, and so on).
9. Input a date (day, month, year) and check if it is **valid** (use the leap year check for February).
10. Number of **days in a given month** (use the leap year check for February).
11. Convert a **marks percentage** to a class: Distinction, First, Second, Pass, Fail.
12. Check whether a number is **divisible by both 3 and 5**.

### Common mistakes to avoid

| Mistake | Correct |
|---------|---------|
| `el if`, `else if` | `elif` |
| `if x = 5:` | `if x == 5:` |
| `x === 5` | `x == 5` |
| Missing colon `:` | `if x > 5:` |
| Mismatched quotes `'hot"` | `"hot"` or `'hot'` |
| Statements not indented | Indent 4 spaces inside every block |
| `else` with a condition | `else:` has no condition |
| Wrong order of conditions in a ladder | Put the most specific / largest condition first |

---

## 9. Quick Revision Points

- **Nested if:** an `if` inside another `if` or `else`; the inner condition is checked only when the outer one allows it.
- **if-elif ladder:** many mutually exclusive choices; the **first true** condition runs and the rest are skipped.
- Use **`elif`**, not `else if` or `el if`; `else` has no condition and comes last.
- Indentation (4 spaces per level) decides which block each statement belongs to.
- Temperature check: `==` 25 normal, `<` 25 cold, otherwise hot.
- Discounted bill: `amount = amount - amount * 0.15`; the largest slab is checked first.
- Day, month and digit to name programs use a ladder of `==` checks, with `else` for invalid input.
- **Leap year:** Earth takes about 365.2422 days to orbit the Sun.
- Julian calendar: every 4th year is a leap year, giving an average of 365.25 days.
- Gregorian calendar (1582): `365 + 1/4 - 1/100 + 1/400 = 365.2425` days.
- Leap year rule: **divisible by 4 and not by 100, or divisible by 400**.
- 2024 and 2000 are leap years; 1900, 2100 and 2023 are not.
