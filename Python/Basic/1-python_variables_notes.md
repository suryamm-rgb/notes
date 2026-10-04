# Python Variables and Data Types

**Topics:** What is a program, Variables, Dynamic typing, Basic data types, Naming rules, Literals, Type conversion

---

## 1. What is a Program?

- A program is a **step-by-step procedure** for performing a particular task.
- It is a **set of instructions** that the computer can understand.
- A program is made of **data + instructions**.

```
Program = Data + Instructions
```

To work with data, we need a way to store it and refer to it. That is where **variables** come in.

---

## 2. What is a Variable?

- A variable is a **name given to data** so that we can use that name to access the data in our program.
- A variable **occupies memory**. It does not hold the data itself; it is a **reference** to the data stored in memory.

```python
length = 15
```

| Part | Meaning |
|------|---------|
| `length` | Variable (the name), created by declaration |
| `15` | Data (the value), given by initialization |
| `=` | Assignment operator |

> In Python, declaration and initialization happen together in one step: the variable is created when a value is first assigned to it.

### Variable holds a reference

```
length  ------>  [ 15 ]   (object in memory)
```

### Types of data

```python
length = 15         # int    (integer)
price  = 12.75      # float  (decimal)
name   = "John"     # str    (string)
```

---

## 3. Multiple Assignments

### (a) Different values to different variables

```python
name, price, qty = 'soap', 12.75, 5
print(name, price, qty)      # soap 12.75 5
```

### (b) Same value using tuple-unpacking style

```python
a, b, c = 1, 1, 1
print(id(a), id(b), id(c))   # all three ids are the same
```

`id()` returns the memory address (identity) of the object. All three variables point to the **same object** `1`, because Python reuses small integers.

### (c) Chained assignment

```python
a = b = c = 1
print(a, b, c)               # 1 1 1
```

### Storing a list

```python
prices = [10, 7, 5, 6, 4]
print(prices)
```

### Swapping two variables (bonus)

```python
x, y = 10, 20
x, y = y, x
print(x, y)     # 20 10
```

---

## 4. Statically Typed vs Dynamically Typed

### Statically typed languages
- The **data type must be declared** before using the variable.
- The type of a variable is fixed and cannot change.
- Examples: **C, C++, Java**

```c
int a = 15;
float b = 12.74;
```

### Dynamically typed languages
- You **do not need to declare the type** beforehand.
- The type is decided at **runtime**, based on the value assigned.
- The same variable can hold different types at different times.
- Examples: **Python, JavaScript**

```python
a = 144       # a is int
a = 12.75     # now a is float
a = "hello"   # now a is str
```

| Statically Typed | Dynamically Typed |
|------------------|-------------------|
| Type declared explicitly | Type inferred from value |
| Type cannot change | Type can change |
| Errors caught at compile time | Errors appear at runtime |
| C, C++, Java | Python, JavaScript |

> **Remember:** In Python, **everything is an object**. Even numbers, strings and functions are objects.

---

## 5. Checking the Type: `type()`

```python
a = 15
print(type(a))       # <class 'int'>

b = 15.45
print(type(b))       # <class 'float'>

c = 'John'
print(type(c))       # <class 'str'>

d = [10, 12, 13]
print(type(d))       # <class 'list'>
```

---

## 6. Basic (Numeric) Data Types

| Type | Description | Example |
|------|-------------|---------|
| `int` | Whole numbers (any size) | `10`, `-5`, `0` |
| `float` | Decimal numbers | `12.75`, `-0.5` |
| `complex` | Real + imaginary part | `3 + 4j` |
| `bool` | Truth values | `True`, `False` |
| `str` | Text | `"hello"` |

```python
x = 10          # int
y = 3.14        # float
z = 2 + 5j      # complex
flag = True     # bool

print(type(z))       # <class 'complex'>
print(z.real)        # 2.0
print(z.imag)        # 5.0
```

> `bool` is a subtype of `int`: `True == 1` and `False == 0`.

---

## 7. Rules for Naming Variables (Identifiers)

1. Should be **meaningful**.
2. Can contain only **letters, digits and underscore** (alphanumeric + `_`).
3. Must **start with a letter or underscore** (not a digit).
4. Must **not be a keyword** (reserved word).
5. Are **case-sensitive**.
6. Cannot contain **spaces or special characters** such as `-`, `@`, `$`.

### Meaningful names

```python
prod_id = 154
price = 23.45
cust_city = 'new york'
```

### Alphanumeric and underscore only

```python
a1 = 10                    # valid
cust_name = 'james'        # valid
cust-name = 'james'        # invalid (hyphen is treated as minus)
cust name = 'james'        # invalid (space not allowed)
```

### Must start with a letter or underscore

```python
address1 = 'delhi'         # valid
_address = 'delhi'         # valid
1address = 'delhi'         # invalid (starts with a digit)
```

### Must not be a keyword

**Keywords** (reserved words) have a special meaning in Python, so we cannot use them as variable names.

```python
price = 19       # valid
if = 20          # invalid (if is a keyword)
None = 'delhi'   # invalid (None is a keyword)
False = 'yes'    # invalid (False is a keyword)
false = 'yes'    # valid (lowercase 'false' is NOT a keyword)
```

To see all keywords:

```python
import keyword
print(keyword.kwlist)
```

Some common keywords: `if`, `else`, `elif`, `for`, `while`, `def`, `class`, `return`, `import`, `True`, `False`, `None`, `and`, `or`, `not`, `in`, `is`, `try`, `except`.

### Python is case-sensitive

```python
price = 18
Price = 50
PRICE = 199.99
# three different variables
print(price, Price, PRICE)    # 18 50 199.99
```

> **Note:** Use `=` for assignment. `==` is the comparison operator.

### Naming conventions (PEP 8)

- Variables and functions: `snake_case` (e.g., `cust_name`)
- Constants: `UPPER_CASE` (e.g., `PI = 3.14`)
- Classes: `PascalCase` (e.g., `CustomerAccount`)

---

## 8. Literals

A **literal** is a fixed value written directly in the code.

| Literal Type | Example |
|--------------|---------|
| Integer | `10`, `-3`, `0b101` (binary), `0o17` (octal), `0xFF` (hex) |
| Float | `12.75`, `1.5e3` |
| Complex | `3 + 4j` |
| String | `'hello'`, `"world"`, `'''multi-line'''` |
| Boolean | `True`, `False` |
| Special | `None` |
| Collection | `[1, 2]`, `(1, 2)`, `{1, 2}`, `{'a': 1}` |

```python
age = 25            # 25 is an integer literal
name = "Asha"       # "Asha" is a string literal
```

---

## 9. Type Conversion (Casting)

Converting a value from one data type to another.

### Implicit conversion (done automatically by Python)

```python
a = 10        # int
b = 2.5       # float
c = a + b     # int is converted to float automatically
print(c, type(c))   # 12.5 <class 'float'>
```

### Explicit conversion (done by the programmer)

| Function | Converts to |
|----------|-------------|
| `int()` | integer |
| `float()` | float |
| `str()` | string |
| `bool()` | boolean |
| `complex()` | complex |

```python
print(int(12.9))        # 12  (decimal part is dropped, not rounded)
print(float(5))         # 5.0
print(str(100) + "%")   # 100%
print(int("25") + 5)    # 30
print(bool(0))          # False
print(bool("hi"))       # True
```

> `int("abc")` raises a `ValueError` because it is not a valid number.

---

## 10. Other Useful Things

### Deleting a variable

```python
x = 10
del x       # x no longer exists
```

### Taking input (always returns a string)

```python
name = input("Enter name: ")
age = int(input("Enter age: "))
print(name, age)
```

---

## 11. Quick Revision Points

- A program is **data + instructions**.
- A variable is a **name that refers to data** stored in memory.
- `variable = value`: `=` is the assignment operator.
- Python is **dynamically typed**, so no type declaration is needed and the type can change.
- Everything in Python is an **object**; use `type()` to check the type and `id()` to see identity.
- Variable names: letters, digits and underscore only; cannot start with a digit; cannot be a keyword; case-sensitive.
- Multiple assignment: `a, b = 1, 2` and `a = b = 1`.
- Core numeric types: `int`, `float`, `complex`, plus `bool` and `str`.
- Use `int()`, `float()`, `str()` for explicit type conversion.
