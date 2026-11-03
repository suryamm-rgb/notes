# Python Roadmap: Beginner to Advanced

A step-by-step learning path with topics, practice projects, and suggested timelines. Timelines assume 1–2 hours of practice per day; adjust to your pace.

---

## Overview

| Stage | Focus | Approx. Time |
|-------|-------|--------------|
| 1. Beginner | Syntax, basic data types, control flow | 3–4 weeks |
| 2. Intermediate | OOP, modules, files, error handling | 4–6 weeks |
| 3. Advanced Core | Decorators, generators, concurrency, typing | 6–8 weeks |
| 4. Specialization | Web, Data, Automation, ML, etc. | 2–6 months |
| 5. Professional Skills | Testing, packaging, CI/CD, design | Ongoing |

---

## Stage 1: Beginner

### 1.1 Setup
- Install Python 3.12+ from python.org
- Choose an editor: VS Code, PyCharm, or Jupyter Notebook
- Learn to run scripts from the terminal (`python script.py`)
- Use the REPL for quick experiments

### 1.2 Core Basics
- Variables and naming conventions (PEP 8)
- Data types: `int`, `float`, `str`, `bool`, `None`
- Type conversion (`int()`, `str()`, `float()`)
- Input and output (`input()`, `print()`, f-strings)
- Operators: arithmetic, comparison, logical, assignment
- Comments and docstrings

### 1.3 Control Flow
- `if` / `elif` / `else`
- `for` and `while` loops
- `break`, `continue`, `pass`
- `range()`, `enumerate()`, `zip()`

### 1.4 Data Structures
- **Lists**: indexing, slicing, methods, list comprehensions
- **Tuples**: immutability, unpacking
- **Dictionaries**: key-value access, iteration, methods
- **Sets**: uniqueness, set operations (union, intersection)
- **Strings**: slicing, methods (`split`, `join`, `strip`, `replace`)

### 1.5 Functions
- Defining functions, parameters, return values
- Default, keyword, `*args`, `**kwargs`
- Scope (local, global) and the `LEGB` rule
- Lambda functions
- Built-ins: `map`, `filter`, `sorted`, `sum`, `min`, `max`, `any`, `all`

### Beginner Projects
1. Number guessing game
2. Simple calculator
3. To-do list in the terminal
4. Password generator
5. Rock-Paper-Scissors
6. Temperature/unit converter

---

## Stage 2: Intermediate

### 2.1 Object-Oriented Programming (OOP)
- Classes and objects, `__init__`, `self`
- Instance vs class attributes
- Methods: instance, `@classmethod`, `@staticmethod`
- Inheritance, `super()`, method overriding
- Encapsulation and `@property`
- Dunder (magic) methods: `__str__`, `__repr__`, `__eq__`, `__len__`, `__add__`
- Dataclasses (`@dataclass`)
- Abstract base classes (`abc` module)

### 2.2 Modules and Packages
- `import`, `from ... import`, aliases
- Creating your own modules and packages
- The standard library tour: `os`, `sys`, `pathlib`, `datetime`, `random`, `math`, `collections`, `itertools`, `functools`, `json`, `re`
- Virtual environments (`venv`) and `pip`
- Dependency files (`requirements.txt`, `pyproject.toml`)

### 2.3 File Handling
- Reading and writing text files (`with open(...)`)
- Working with CSV and JSON
- `pathlib` for file paths
- Context managers (`with` statement)

### 2.4 Error Handling
- `try` / `except` / `else` / `finally`
- Raising exceptions (`raise`)
- Custom exceptions
- Logging with the `logging` module

### 2.5 Intermediate Concepts
- Regular expressions (`re`)
- Working with dates and times
- Comprehensions: list, dict, set, generator expressions
- Iterators and the iterator protocol
- Basic use of `collections`: `Counter`, `defaultdict`, `deque`, `namedtuple`

### 2.6 Tools
- Git and GitHub basics
- Debugging (`pdb`, IDE debugger)
- Linting and formatting: `ruff`, `black`, `flake8`

### Intermediate Projects
1. Contact book (OOP + JSON storage)
2. Expense tracker with CSV export
3. Text-based adventure game
4. Web scraper (with `requests` + `BeautifulSoup`)
5. Command-line tool using `argparse`
6. Library/bank management system

---

## Stage 3: Advanced Core Python

### 3.1 Advanced Functions
- Closures
- Decorators (with and without arguments)
- `functools`: `lru_cache`, `partial`, `wraps`, `reduce`
- First-class functions and higher-order functions

### 3.2 Generators and Iterators
- `yield` and generator functions
- Generator pipelines
- `itertools` in depth
- Lazy evaluation and memory efficiency

### 3.3 Advanced OOP
- Multiple inheritance and MRO
- Mixins
- Descriptors
- Metaclasses (conceptual understanding)
- `__slots__`
- Context managers via `__enter__` / `__exit__` and `contextlib`

### 3.4 Type Hints and Static Typing
- Type annotations for variables, functions, classes
- `typing` module: `Optional`, `Union`, `List`, `Dict`, `Callable`, `TypeVar`, `Protocol`, `Generic`
- Modern syntax (`list[int]`, `X | None`)
- Static checking with `mypy` or `pyright`
- Data validation with `pydantic`

### 3.5 Concurrency and Parallelism
- The GIL (Global Interpreter Lock): what it is and why it matters
- `threading` for I/O-bound tasks
- `multiprocessing` for CPU-bound tasks
- `concurrent.futures` (ThreadPoolExecutor, ProcessPoolExecutor)
- `asyncio`: `async` / `await`, tasks, event loop
- Async libraries: `aiohttp`, `httpx`

### 3.6 Performance and Memory
- Profiling: `cProfile`, `timeit`, `line_profiler`
- Memory management and garbage collection
- Choosing the right data structure
- Big-O complexity basics
- Speed-ups: `numpy`, `numba`, Cython, or rewriting hot spots

### 3.7 Python Internals
- How Python executes code (bytecode, CPython)
- Mutable vs immutable, identity vs equality
- Shallow vs deep copy
- Name binding, references, and the `is` operator
- The `dis` module

### Advanced Projects
1. Custom decorator library (timing, caching, retry, logging)
2. Async web crawler
3. Plugin system using ABCs and dynamic imports
4. In-memory database or key-value store
5. Task scheduler with threads/async
6. Mini web framework or ORM

---

## Stage 4: Choose a Specialization

### Web Development
- **Frameworks**: Flask, Django, FastAPI
- REST API design, authentication (JWT, OAuth)
- Databases: SQL (PostgreSQL), SQLAlchemy / Django ORM, Redis
- Frontend basics: HTML, CSS, JavaScript
- Deployment: Docker, Nginx, cloud platforms

### Data Science and Analysis
- **Libraries**: NumPy, Pandas, Matplotlib, Seaborn, Plotly
- Data cleaning, EDA, visualization
- SQL for analysts
- Statistics and probability fundamentals
- Jupyter notebooks

### Machine Learning and AI
- **Libraries**: scikit-learn, PyTorch, TensorFlow/Keras
- ML fundamentals: regression, classification, clustering, evaluation metrics
- Deep learning: neural networks, CNNs, RNNs, Transformers
- NLP: Hugging Face, spaCy
- LLM applications: prompt engineering, RAG, APIs, vector databases
- MLOps basics: experiment tracking, model serving

### Automation and Scripting
- File and system automation (`os`, `shutil`, `subprocess`)
- Web automation: Selenium, Playwright
- Excel/PDF/Email automation (`openpyxl`, `PyPDF`, `smtplib`)
- Scheduling tasks (cron, `schedule`, Airflow)

### DevOps and Cloud
- Scripting infrastructure tasks
- Docker, Kubernetes basics
- Cloud SDKs: `boto3` (AWS), Google Cloud, Azure
- Infrastructure as Code (Terraform, Pulumi)
- CI/CD pipelines (GitHub Actions, GitLab CI)

### Cybersecurity
- Networking basics, `socket`, `scapy`
- Web security fundamentals (OWASP Top 10)
- Writing security tools for defensive and authorized testing

### Game Development
- Pygame, Arcade, Godot (GDScript is Python-like)

### Desktop and GUI Apps
- Tkinter, PyQt/PySide, Kivy

### Data Engineering
- ETL pipelines, Apache Airflow, Spark (PySpark)
- Data warehouses, Kafka, dbt

---

## Stage 5: Professional Skills

### Testing
- `unittest` and `pytest`
- Fixtures, parametrization, mocking (`unittest.mock`)
- Test coverage (`pytest-cov`)
- Test-driven development (TDD)

### Code Quality
- PEP 8, PEP 20 (Zen of Python), PEP 257
- Code reviews and refactoring
- Pre-commit hooks
- Clean code and SOLID principles

### Design Patterns and Architecture
- Singleton, Factory, Observer, Strategy, Decorator, Adapter
- Clean architecture and dependency injection
- Microservices vs monoliths
- API design principles

### Packaging and Distribution
- `pyproject.toml`, `setuptools`, `poetry`, `uv`
- Publishing to PyPI
- Semantic versioning
- Documentation: Sphinx, MkDocs

### Databases
- SQL fundamentals and query optimization
- ORMs (SQLAlchemy)
- NoSQL: MongoDB, Redis
- Migrations (Alembic)

### DevOps Essentials
- Git workflows (branching, pull requests)
- Docker and containers
- CI/CD with GitHub Actions
- Logging, monitoring, and observability

### Soft Skills
- Reading other people's code and documentation
- Contributing to open source
- Writing technical docs and READMEs
- Building a portfolio on GitHub

---

## Suggested Month-by-Month Plan

| Month | Goals |
|-------|-------|
| 1 | Basics, control flow, data structures, functions; 3 small projects |
| 2 | OOP, modules, file I/O, error handling; 2 intermediate projects |
| 3 | Standard library, regex, Git, virtual environments, testing basics |
| 4 | Decorators, generators, type hints, async basics; 1 advanced project |
| 5–6 | Pick a specialization; learn its main libraries; build a portfolio project |
| 7+ | Deepen skills, contribute to open source, learn deployment and system design |

---

## Recommended Resources

### Free
- [Official Python Tutorial](https://docs.python.org/3/tutorial/)
- [Real Python](https://realpython.com/)
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
- [freeCodeCamp Python courses](https://www.freecodecamp.org/)
- [CS50P (Harvard)](https://cs50.harvard.edu/python/)
- [Python Module of the Week](https://pymotw.com/)

### Books
- *Python Crash Course* — Eric Matthes (beginner)
- *Automate the Boring Stuff with Python* — Al Sweigart (beginner)
- *Fluent Python* — Luciano Ramalho (intermediate to advanced)
- *Effective Python* — Brett Slatkin (intermediate to advanced)
- *Python Cookbook* — David Beazley & Brian K. Jones (advanced)
- *Architecture Patterns with Python* — Harry Percival & Bob Gregory (advanced)

### Practice Platforms
- [LeetCode](https://leetcode.com/), [HackerRank](https://www.hackerrank.com/), [Codewars](https://www.codewars.com/)
- [Exercism Python Track](https://exercism.org/tracks/python)
- [Advent of Code](https://adventofcode.com/)
- [Kaggle](https://www.kaggle.com/) (data science)

---

## Tips for Success

1. **Code every day.** Consistency beats long, irregular sessions.
2. **Build projects.** Tutorials teach syntax; projects teach problem-solving.
3. **Read error messages.** They usually tell you exactly what's wrong.
4. **Read other people's code.** Explore open-source repositories.
5. **Don't skip fundamentals.** Strong basics make advanced topics easier.
6. **Use version control from the start.** Commit your work to GitHub.
7. **Learn to debug.** Use print statements, a debugger, and tests.
8. **Teach what you learn.** Write blog posts or explain concepts to others.
9. **Don't aim for perfection.** Make it work, then make it clean, then make it fast.
10. **Stay curious.** Python evolves; follow release notes and PEPs.

---

## Progress Checklist

### Beginner
- [ ] Variables, data types, operators
- [ ] Conditionals and loops
- [ ] Lists, tuples, dicts, sets
- [ ] Functions and scope
- [ ] 3+ small projects completed

### Intermediate
- [ ] Classes and OOP
- [ ] Modules, packages, virtual environments
- [ ] File I/O, JSON, CSV
- [ ] Exceptions and logging
- [ ] Git and GitHub
- [ ] 2+ medium projects completed

### Advanced
- [ ] Decorators, generators, context managers
- [ ] Type hints and static analysis
- [ ] Concurrency (threads, processes, asyncio)
- [ ] Testing with pytest
- [ ] Profiling and optimization
- [ ] 1+ large project completed

### Specialization
- [ ] Chosen a domain
- [ ] Learned its core libraries
- [ ] Built and deployed a portfolio project
- [ ] Contributed to an open-source project

---

*Happy coding!*
