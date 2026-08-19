# PEP 8 – Writing Better Python Code

> *"Readability counts."* — PEP 8

---

## 1. What is PEP 8?

**PEP** stands for **Python Enhancement Proposal**. It's simply a document that proposes a new feature, guideline, or improvement for the Python language.

**PEP 8** is one specific PEP — the **official style guide for writing Python code**. It was written by Guido van Rossum (the creator of Python), along with Barry Warsaw and Nick Coghlan.

A **"style guide"** is just a set of agreed-upon conventions for how code should *look*. It doesn't change what your code *does* — it changes how easy your code is to *read*.

PEP 8 exists because Python is read by thousands of different developers, and without a shared standard, everyone's code would look completely different from everyone else's.

---

## 2. Why Should We Care About PEP 8?

Here's a simple scenario:

Imagine 5 developers working on the same project, and each one writes the same variable in a different way:

```python
# Developer 1
userName = "Abhishek"

# Developer 2
user_name = "Abhishek"

# Developer 3
USER_NAME = "Abhishek"

# Developer 4
u = "Abhishek"
```

All four lines run perfectly fine. But now imagine reading a 5,000-line file where every developer named things their own way. You'd constantly have to stop and think, *"Wait, is this the same variable as before?"*

Consistent style helps with:

- **Readability** — anyone can scan the code and instantly recognize patterns
- **Maintainability** — future developers (including future-you) can update code faster
- **Code reviews** — reviewers focus on logic, not arguing about formatting
- **Team collaboration** — everyone's code "feels" like it was written by one person
- **Debugging** — consistent structure makes bugs easier to spot
- **Long-term projects** — style debt doesn't pile up over years

---

## 3. PEP 8 Is a Guide, Not a Law

This is one of the most important things to understand about PEP 8.

Python will happily run this:

```python
x=10
```

There is no syntax error. It's **valid Python**.

But PEP 8 recommends:

```python
x = 10
```

This is **good Python style**.

**Valid Python** = code the interpreter accepts.
**Good Python style** = code that other humans can read comfortably.

PEP 8 itself says something important: if your project already has its own style guide, and it conflicts with PEP 8, your project's guide usually wins — **as long as everyone follows it consistently.** The real goal isn't "obey PEP 8 word for word." The real goal is **readability and consistency within a project.**

---

## 4. Code Layout

### 4.1 Indentation

Rules:
- Use **4 spaces** per indentation level
- **Prefer spaces over tabs**
- **Never mix tabs and spaces** in the same file

❌ Bad

```python
if age >= 18:
  print("Adult")
```

✅ Better

```python
if age >= 18:
    print("Adult")
```

**Why it matters:** Indentation in Python isn't just style — it defines code blocks. Inconsistent indentation makes it hard to tell what belongs inside a function, loop, or condition, and mixing tabs/spaces can even cause errors in some editors.

---

### 4.2 Maximum Line Length

The traditional PEP 8 recommendation is:

- **79 characters** for code
- **~72 characters** for comments and docstrings

❌ Bad

```python
result = calculate_total_price(item_price, item_quantity, discount_percentage, tax_percentage, shipping_fee)
```

✅ Better

```python
result = calculate_total_price(
    item_price, item_quantity, discount_percentage, tax_percentage, shipping_fee
)
```

Shorter lines help with:
- Reading top-to-bottom without horizontal scrolling
- Viewing two files **side-by-side**
- Reviewing diffs in pull requests
- Reading comfortably on smaller screens or split editors

**Note:** 79 is not a hard Python rule. Many modern teams (and tools like Black) use 88 or 99 characters instead. What matters most is that **your team agrees on one number and sticks to it.**

---

### 4.3 Breaking Long Lines

Use `()`, `[]`, or `{}` to break long expressions instead of backslashes.

❌ Bad

```python
result = some_function(first_value, second_value, third_value, fourth_value)
```

✅ Better

```python
result = some_function(
    first_value,
    second_value,
    third_value,
    fourth_value,
)
```

Parentheses are preferred over backslash (`\`) line continuations because they're more visible, less error-prone (a stray space after a backslash breaks the code), and every editor understands them.

---

### 4.4 Blank Lines

- **Two blank lines** before and after top-level functions and classes
- **One blank line** between methods inside a class
- Blank lines can be used sparingly to separate logical sections inside a function

```python
class Student:

    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello, {self.name}")


def main():
    student = Student("Rahul")
    student.greet()
```

---

## 5. Imports

### Separate imports

❌ Bad

```python
import os, sys
```

✅ Better

```python
import os
import sys
```

### Import order

Imports should be grouped in this order, with a blank line between each group:

1. Standard library imports
2. Third-party library imports
3. Local application imports

```python
import os

import requests

from myapp.models import User
```

### Avoid wildcard imports

❌ Bad

```python
from math import *
```

✅ Better

```python
from math import sqrt
```

**Why:** With `from math import *`, you have no idea where a name like `sqrt` or `pi` actually came from — it could even silently overwrite a name you already defined. Explicit imports keep the source of every name traceable.

---

## 6. Whitespace

❌ Bad

```python
x=10
y =20
result=x+y
```

✅ Better

```python
x = 10
y = 20
result = x + y
```

Use a single space around operators like:

`=`, `+`, `-`, `==`, `<`, `>`, `and`, `or`

But **don't** add extra spaces right after an opening bracket or before a closing one, and don't add a space before `(` or `[` when calling a function or indexing.

❌ Bad

```python
print( "Hello" )
numbers [0]
function (value)
```

✅ Better

```python
print("Hello")
numbers[0]
function(value)
```

---

## 7. String Quotes

Python allows both:

```python
name = "Abhishek"
name = 'Abhishek'
```

PEP 8 does **not** force single or double quotes — it only asks you to be **consistent**.

The one tip PEP 8 gives: pick whichever quote avoids extra backslash-escaping.

❌ Bad

```python
message = 'It\'s a beautiful day.'
```

✅ Better

```python
message = "It's a beautiful day."
```

---

## 8. Comments

The golden rule of comments:

> **Comments should explain WHY, not simply repeat WHAT the code does.**

❌ Bad

```python
x = x + 1  # Add 1 to x
```

✅ Better

```python
x = x + 1  # Compensate for the zero-based index
```

Guidelines:
- Keep comments accurate — an outdated comment is worse than no comment
- Add comments when they explain reasoning or non-obvious context
- Avoid comments that just restate the code
- Avoid over-commenting every single line

---

## 9. Docstrings

A **docstring** is a string placed right after a function, class, or module definition, describing what it does. Unlike a regular comment (`#`), it's accessible at runtime via `.__doc__` and shown in help systems.

```python
def calculate_average(numbers):
    """Return the average of a list of numbers."""
    return sum(numbers) / len(numbers)
```

**Comment vs. docstring:**
- A **comment** explains a specific line or decision, for developers reading the source.
- A **docstring** documents the *purpose* of a function/class/module, and tools can read it programmatically.

PEP 8 covers docstring *placement and basic conventions*, but for detailed formatting rules, it points to **PEP 257 – Docstring Conventions**.

---

## 10. Naming Conventions

This is one of the most impactful parts of PEP 8, because names are what you read the most.

### Variables — `snake_case`

✅ Preferred

```python
student_name = "Rahul"
```

❌ Avoid

```python
studentName = "Rahul"
```

### Functions — `snake_case`

✅ Preferred

```python
def calculate_average():
    ...

def get_student_data():
    ...
```

❌ Avoid

```python
def calculateAverage():
    ...

def getStudentData():
    ...
```

### Classes — `CapWords` (PascalCase)

✅ Preferred

```python
class Student:
    ...

class BankAccount:
    ...

class StudentManager:
    ...
```

### Constants — `UPPER_CASE`

✅ Preferred

```python
MAX_RETRIES = 3
DEFAULT_TIMEOUT = 30
```

### Private / internal names

A **leading underscore** signals "this is internal — not part of the public API."

```python
_internal_function()
_internal_variable
```

It's a convention, not enforced by Python — but other developers (and tools) will treat it as "please don't rely on this from outside."

### Avoid meaningless names

❌ Bad

```python
x = 10
a = 20
d = 30
```

✅ Better

```python
age = 10
student_count = 20
total_marks = 30
```

Descriptive names reduce the need for extra comments — the code explains itself.

### Avoid these single-character names

Avoid `l`, `O`, and `I` as variable names — depending on the font, they can look identical to the digits `1` and `0`, causing confusing bugs.

---

## 11. Writing Readable Functions

PEP 8 isn't only about spacing — it also encourages readable structure.

❌ Bad

```python
def f(a,b,c):
    x=a+b
    y=x*c
    return y
```

✅ Better

```python
def calculate_total(price, quantity, tax_rate):
    subtotal = price * quantity
    total = subtotal * (1 + tax_rate)
    return total
```

What changed:
- Meaningful function and parameter names
- Proper spacing
- Clear, self-explanatory logic

**Important distinction:** PEP 8 does **not** explicitly require "small functions." Keeping functions short and focused is a general good-programming practice (often associated with Clean Code), and it works well *alongside* PEP 8 — but it's not a PEP 8 rule itself.

---

## 12. Writing Conditions Clearly

❌ Less clear

```python
if user.is_authenticated == True:
    ...
```

✅ Clearer

```python
if user.is_authenticated:
    ...
```

Comparing a boolean to `True`/`False` is redundant — the value is already a boolean.

```python
if not user.is_admin:
    ...
```

This reads almost like plain English: "if the user is not an admin."

**Note:** Some of this falls under general Python best practice rather than a strict PEP 8 rule — it's worth knowing the difference so you don't misquote PEP 8 in a review.

---

## 13. One Statement Per Line

❌ Bad

```python
if age >= 18: print("Adult")
```

✅ Better

```python
if age >= 18:
    print("Adult")
```

❌ Bad

```python
x = 1; y = 2; z = 3
```

✅ Better

```python
x = 1
y = 2
z = 3
```

Compound statements save a couple of keystrokes but make code much harder to scan line-by-line — especially during debugging.

---

## 14. Trailing Commas

```python
students = [
    "Alice",
    "Bob",
    "Charlie",
]
```

Adding a trailing comma after the last item means that when you add a new item later, only **one line changes** in your Git diff — not two. It keeps version-control history clean and reviews easier.

---

## 15. Python Naming Cheat Sheet

| What | Style | Example |
|------|-------|---------|
| Variable | snake_case | `student_name` |
| Function | snake_case | `calculate_total()` |
| Class | CapWords | `StudentManager` |
| Constant | UPPER_CASE | `MAX_SIZE` |
| Module | lowercase | `student.py` |
| Package | lowercase | `utils` |
| Internal | _leading_underscore | `_helper()` |

---

## 16. Real Example: Bad Python Code → PEP 8 Code

❌ Before

```python
import os,sys
def calculateStudentMarks(name,marks):
    total=0
    for i in marks:
        total+=i
    return total
```

✅ After

```python
import os
import sys


def calculate_student_marks(name, marks):
    total = 0

    for mark in marks:
        total += mark

    return total
```

**What changed, and why:**

1. **Import formatting** — `os` and `sys` split onto separate lines for clarity
2. **Blank lines** — two blank lines before the top-level function
3. **Function naming** — `calculateStudentMarks` → `calculate_student_marks` (snake_case)
4. **Variable naming** — kept meaningful (`total`)
5. **Spaces** — `=` and `+=` now have spaces around them
6. **Loop variable naming** — `i` → `mark`, which describes what it actually holds
7. **Overall readability** — the intent of the function is now obvious at a glance

---

## 17. Let's Review This Code 🔍

Here's some real (bad) code. Before scrolling further — **ask your audience: "What's wrong with this?"**

```python
import os,sys
userName="Abhishek"
def getUserData(id):
    if id==1: print("Found")
    else: print("Not Found")
    return userName
```

**Reveal the answers one by one:**

1. `import os,sys` → should be two separate import lines
2. `userName` → should be `user_name` (snake_case)
3. No spaces around `=` in `userName="Abhishek"`
4. `getUserData` → should be `get_user_data`
5. `id` shadows Python's built-in `id()` function — better to rename it
6. `if id==1: print(...)` → compound statement on one line, and missing spaces around `==`
7. No blank lines around the function definition
8. No docstring explaining what the function does

---

## 18. Common PEP 8 Mistakes Beginners Make

| Mistake | ❌ Bad | ✅ Better | Why |
|---|---|---|---|
| No spaces around operators | `x=1+2` | `x = 1 + 2` | Easier to scan |
| Wrong indentation | 2 spaces | 4 spaces | Matches convention, avoids confusion |
| Mixing tabs and spaces | tab + spaces | spaces only | Can break code in some editors |
| camelCase variables | `userName` | `user_name` | Python convention is snake_case |
| Meaningless names | `x`, `d` | `age`, `data` | Improves readability |
| Extremely long lines | 150+ chars | wrapped lines | Easier to read/review |
| Too many blank lines | 4+ blank lines | 1–2 blank lines | Reduces unnecessary scrolling |
| Missing blank lines | functions crammed together | 2 blank lines between them | Visually separates logic |
| Multiple statements per line | `x=1; y=2` | separate lines | Easier to debug |
| Wildcard imports | `from math import *` | `from math import sqrt` | Keeps origin of names clear |
| Imports in random places | import mid-file | imports at top | Predictable structure |
| Too many inline comments | comment on every line | comment only when needed | Reduces noise |
| Missing docstrings | no docstring on public function | add a docstring | Documents intent |
| Inconsistent naming | mixing styles in one file | pick one style | Keeps codebase uniform |

---

## 19. PEP 8 vs Clean Code

It's easy to think "PEP 8 = good code," but that's not quite accurate.

**PEP 8 covers:**
- Formatting
- Naming
- Whitespace
- Imports
- Comments
- Layout

**Clean Code** is a broader concept that also includes:
- SOLID principles
- Separation of concerns
- Avoiding duplication
- Good architecture
- Testability
- Maintainability
- Appropriate abstractions

**Key idea:** *Following PEP 8 does not automatically mean your code is well-designed.*

Example — this code is perfectly PEP 8-compliant, but poorly designed:

```python
def process(data):
    result = []
    for item in data:
        if item is not None:
            if item > 0:
                if item % 2 == 0:
                    result.append(item * 2)
    return result
```

Perfect spacing and naming — but deeply nested logic that's hard to follow. PEP 8 handles the *surface*; Clean Code principles handle the *structure underneath*.

---

## 20. How Can We Automatically Check PEP 8?

You don't have to enforce all this by hand — tools can do it for you.

### pycodestyle

Checks your code against PEP 8 and reports violations.

```bash
pycodestyle my_file.py
```

### Ruff

A modern, extremely fast Python linter *and* formatter, written in Rust.

```bash
ruff check .
ruff format .
```

### Black

An opinionated Python code **formatter** — it automatically reformats your code to a consistent style.

```bash
black .
```

**Linter vs. Formatter:**
- A **linter** (like pycodestyle) *finds* problems and reports them.
- A **formatter** (like Black) *automatically rewrites* your code to fix formatting.

**Note:** Black is opinionated and doesn't implement every single PEP 8 rule exactly — it makes its own consistent choices (e.g., preferring double quotes, using 88-character lines by default). It aligns with PEP 8's spirit, not a line-by-line implementation of it.

---

## 21. PEP 8 in VS Code / PyCharm

Modern editors make following PEP 8 almost automatic:

- **Formatting** — auto-format on save (via Black, Ruff, or autopep8)
- **Linting** — real-time warnings/underlines for style violations
- **Import organization** — auto-sorts and groups imports
- **Extensions** — VS Code's Python extension and PyCharm both have built-in PEP 8 checks

Set these up once, and most of this guide gets applied automatically as you type.

---

## 22. Top 10 PEP 8 Rules to Remember

| # | Rule | Example |
|---|------|---------|
| 1 | Use 4 spaces for indentation | `if x:\n    do_thing()` |
| 2 | Prefer spaces over tabs | (no tabs, ever) |
| 3 | Keep lines reasonably short | wrap at ~79–99 chars |
| 4 | Use blank lines to organize code | 2 lines between functions |
| 5 | Use spaces around operators | `x = 1 + 2` |
| 6 | Use snake_case for variables/functions | `student_name` |
| 7 | Use CapWords for classes | `class Student:` |
| 8 | Keep imports organized | stdlib → third-party → local |
| 9 | Avoid wildcard imports | `from math import sqrt` |
| 10 | Write readable, consistent code | above all else |

---

## 23. PEP 8 Quiz 🎯

Use these live with your audience — reveal the answer after they guess.

**Q1.** Which is better?
A. `studentName="Rahul"`
B. `student_name = "Rahul"`
**Answer: B** — snake_case with spaces around `=`.

**Q2.** Find the mistake:
```python
import os,sys
```
**Answer:** Two imports on one line — should be split into two separate `import` statements.

**Q3.** Which is more readable?
A. `if x==True:`
B. `if x:`
**Answer: B** — comparing to `True` is redundant.

**Q4.** What's wrong with this class name?
```python
class student_manager:
```
**Answer:** Classes should use CapWords → `StudentManager`.

**Q5.** Bad vs good — pick the good one:
A. `result=a+b`
B. `result = a + b`
**Answer: B**

**Q6.** True or False: PEP 8 requires exactly 79-character lines in every modern project.
**Answer: False** — 79 is the traditional recommendation; teams may choose longer limits (e.g. 88, 99) as long as they're consistent.

**Q7.** What's wrong with this constant?
```python
maxRetries = 3
```
**Answer:** Constants should be UPPER_CASE → `MAX_RETRIES = 3`.

**Q8.** Find the mistake:
```python
if age >= 18: print("Adult")
```
**Answer:** Compound statement on one line — should be split across two lines.

**Q9.** Which import style is discouraged?
A. `from math import sqrt`
B. `from math import *`
**Answer: B** — wildcard imports hide where names come from.

**Q10.** What's the difference between a comment and a docstring?
**Answer:** A comment explains a specific line for readers of the source code; a docstring documents a function/class/module's purpose and is accessible at runtime via `.__doc__`.

---

## 24. PEP 8 Cheat Sheet

| Topic | Rule |
|---|---|
| **Indentation** | 4 spaces, no tabs |
| **Line length** | ~79 chars (code), ~72 (comments); teams may extend to 88–99 |
| **Blank lines** | 2 before/after top-level defs, 1 between methods |
| **Imports** | One per line; stdlib → third-party → local, in that order |
| **Whitespace** | Spaces around operators; none inside brackets or before `(`/`[` |
| **Naming (variables/functions)** | `snake_case` |
| **Naming (classes)** | `CapWords` |
| **Naming (constants)** | `UPPER_CASE` |
| **Naming (internal)** | `_leading_underscore` |
| **Comments** | Explain *why*, not *what*; keep accurate |
| **Docstrings** | Triple-quoted, describe purpose, see PEP 257 |
| **Strings** | Single or double quotes — just be consistent |
| **Statements** | One statement per line |

---

## 25. Final Takeaway

> **PEP 8 is not about making Python code look pretty.**
> **It is about making code easier for humans to read, understand, review and maintain.**

Five things to remember:

1. 🧑‍💻 **Write for humans** — code is read far more often than it's written
2. 🔁 **Be consistent** — within your file, your project, and your team
3. 🏷️ **Use meaningful names** — good names replace half your comments
4. 🧹 **Keep formatting clean** — spacing, indentation, blank lines matter
5. 📖 **Prefer readability** — when in doubt, choose the option a stranger could understand fastest