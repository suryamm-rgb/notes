# Python Notes

## 1. Introduction to Python

- Python is a **programming language**, like C++, Java and JavaScript.
- It is **simple and easy to learn**, with clean, readable syntax.
- It is a **scripting language** with **dynamic data types** (no need to declare the type of a variable; the type is decided at runtime).
- It is a **hybrid language**: both compiled and interpreted.
- It is **platform independent** and **portable**: the same code runs on Windows, macOS and Linux.
- It is a **general-purpose language**, not limited to one special purpose.
- It is **data-centric**, which makes it very popular in data-related fields.
- It is an **open-source**, **high-level** language with a very large standard library and community.

### Where is Python used?

- Desktop applications
- Web applications
- Games
- Data Science
- Machine Learning and AI
- Automation and scripting

### Example

```python
name = "Python"      # str
version = 3          # int (dynamic typing: no type declaration needed)
print(name, version)
```

---

## 2. Programming Languages

Programming languages are divided into two main levels:

### Low-Level Languages
- **Machine language**: binary (0s and 1s), directly understood by the CPU.
- **Assembly language**: uses mnemonics (e.g., `MOV`, `ADD`); needs an assembler to convert it to machine code.

### High-Level Languages
Human-readable languages that need translation into machine code.

| Type | Translator | Examples |
|------|-----------|----------|
| Compiled | Compiler | C, C++ |
| Interpreted | Interpreter | JavaScript, PHP |
| Hybrid | Compiler + Interpreter | Java, Python |

---

## 3. Compiler vs Interpreter

**Analogy:** You have a recipe written in English, but the cook only understands Chinese.

- **Compiler**: translates the *whole* recipe into Chinese **once**, then the cook prepares the dish using the translated copy.
- **Interpreter**: translates the recipe **line by line**, and the cook prepares the dish *while* the translation happens.
- If there is an error in the recipe:
  - **Compiler**: nothing is prepared (no translation is produced).
  - **Interpreter**: preparation continues *partially* until the line with the error.

### Comparison Table

| Compiler | Interpreter |
|----------|-------------|
| One-time translation | Translation happens every time (n times) |
| Produces a file in machine code | File remains in source code |
| Execution happens **after** translation | Execution happens **while** translating |
| Independent execution (no extra software needed) | Execution needs a runtime environment |
| If there is an error, compilation fails and nothing runs | Partial execution until the error is reached |
| Faster execution | Slower execution |
| Examples: C, C++ | Examples: JavaScript, PHP |

---

## 4. Python is a Hybrid Language

- Python uses **both a compiler and an interpreter**.
- The compiler converts source code into **bytecode**.
- The interpreter (PVM) then executes the bytecode.

### Execution Flow

```
first.py  -->  Compiler  -->  first.pyc (bytecode / intermediate language)  -->  Interpreter (PVM)  -->  Machine code  -->  Output
```

- `first.py`: source code
- `first.pyc`: Python bytecode (intermediate language), stored in the `__pycache__` folder
- **PVM (Python Virtual Machine)**: converts the bytecode into machine code and runs it

---

## 5. Python is Platform Independent

- `first.py` is compiled into `first.pyc` (bytecode).
- The bytecode is not tied to any specific OS or hardware.
- The same `.pyc` can run on **Windows, macOS and Linux**, as long as a **PVM** is installed on that system.
- Only the PVM is platform specific; the bytecode is universal.

> Write once, run anywhere.

---

## 6. Programming Paradigms

A **programming paradigm** is a style or approach to writing programs.

1. Procedural programming
2. Object-oriented programming
3. Modular programming
4. Functional programming

### 6.1 Monolithic Program
- A program with tens or hundreds of instructions written as a **single piece or single body**.
- Hard to read, debug, maintain and reuse.

### 6.2 Procedural Programming
- A set of instructions that performs a **smaller task or a complete task** is written as a **function** (procedure).
- A bigger application is broken down into many functions.
- Functions are stored inside **modules** and grouped together, which leads to modular programming.
- Example languages: C, Python.

### 6.3 Modular Programming
- The program is developed in the form of **pieces (modules)**.
- To **update or upgrade** a feature, only that particular module needs to change.
- To **replace** a feature, just replace the module.
- Advantages: reusability, easy maintenance, easy testing, teamwork.

### 6.4 Object-Oriented Programming (OOP)
- Focuses on **data and the operations (functions) together**.
- We define a **class**, then create **objects** of that class and use them.
- A class is a **blueprint** containing the data (attributes) and functions (methods).
- Main concepts: **Class, Object, Encapsulation, Inheritance, Polymorphism, Abstraction**.

```python
class Car:
    def __init__(self, brand):
        self.brand = brand      # data

    def start(self):            # operation
        print(self.brand, "started")

c1 = Car("Toyota")              # object
c1.start()
```

### 6.5 Functional Programming
- Based on **mathematical functions**.
- Functions take input and return output, avoiding changing shared data (no side effects).
- Python supports it through `lambda`, `map()`, `filter()` and `reduce()`.

```python
square = lambda x: x * x
print(square(5))    # 25
```

> **Note:** Python is a **multi-paradigm** language and supports all of the above.

---

## 7. Python Libraries / Modules

A **library** (or module) is a collection of pre-written code that we can reuse.

| Area | Libraries |
|------|-----------|
| Desktop applications | Tkinter, PySide |
| Web applications | Flask, Django |
| Games | Pygame, Panda3D |
| Database programming | PyMySQL, SQLite (`sqlite3`) |
| Data Science | NumPy, Pandas |
| Machine Learning | TensorFlow, Scikit-learn |

```python
import math
print(math.sqrt(16))   # 4.0
```

---

## 8. Quick Revision Points

- Python is high-level, general-purpose, dynamically typed and easy to learn.
- Python is both compiled (to bytecode) and interpreted (by the PVM), so it is hybrid.
- Source (`.py`) becomes bytecode (`.pyc`), which the PVM runs.
- Platform independence comes from bytecode plus the PVM.
- Paradigms: procedural, modular, object-oriented, functional.
- Libraries make Python useful for desktop, web, games, databases, data science and ML.
