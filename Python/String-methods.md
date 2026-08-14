# Python Strings — Complete Study Notes

---

## 1. String Creation

```python
s1 = 'single quotes'
s2 = "double quotes"
s3 = '''triple quotes
can span multiple lines'''
s4 = """also triple, multi-line"""

name = "Abhishek"
f_string = f"Hello, {name}!"      # f-string: insert variables directly
print(f_string)
# Output: Hello, Abhishek!
```
- Single and double quotes work the same — pick one, be consistent.
- Triple quotes are used for multi-line strings or docstrings.
- f-strings (f"...") let you plug variables/expressions directly inside `{}`.

---

## 2. Indexing & Slicing

```python
s = "Python"
#    P  y  t  h  o  n
#    0  1  2  3  4  5
#   -6 -5 -4 -3 -2 -1

print(s[0])        # Output: P
print(s[-1])       # Output: n
print(s[1:4])       # Output: yth   (index 1 to 3)
print(s[:3])        # Output: Pyt   (start to index 2)
print(s[3:])        # Output: hon   (index 3 to end)
print(s[::-1])       # Output: nohtyP  (reversed string)
print(s[::2])        # Output: Pto   (every 2nd character)
```
- Indexing starts at 0 from the left, -1 from the right.
- Slicing syntax: `string[start:stop:step]` — stop index is not included.

---

## 3. String Immutability

```python
s = "hello"
# s[0] = "H"   # This will cause an ERROR — strings can't be changed in place

s = "H" + s[1:]   # Instead, create a NEW string
print(s)
# Output: Hello
```
- Once a string is created, it cannot be changed — any "modification" makes a new string.

---

## 4. Common String Operators

```python
a = "Py"
b = "thon"

print(a + b)          # Output: Python        (concatenation)
print(a * 3)          # Output: PyPyPy        (repetition)
print("y" in a)       # Output: True          (membership check)
print("z" not in a)   # Output: True          (negative membership check)
```
- `+` joins strings, `*` repeats a string, `in`/`not in` check if a substring exists.

---

## 5. Case Conversion

### upper()
- What it does: converts all letters to UPPERCASE
- Syntax: `string.upper()`
```python
s = "hello"
print(s.upper())
# Output: HELLO
```

### lower()
- What it does: converts all letters to lowercase
- Syntax: `string.lower()`
```python
s = "HELLO"
print(s.lower())
# Output: hello
```

### title()
- What it does: makes the first letter of each word capital
- Syntax: `string.title()`
```python
s = "hello world"
print(s.title())
# Output: Hello World
```

### capitalize()
- What it does: makes only the first letter of the whole string capital
- Syntax: `string.capitalize()`
```python
s = "hello world"
print(s.capitalize())
# Output: Hello world
```

### swapcase()
- What it does: flips uppercase to lowercase and lowercase to uppercase
- Syntax: `string.swapcase()`
```python
s = "Hello World"
print(s.swapcase())
# Output: hELLO wORLD
```

---

## 6. Search / Find

### find()
- What it does: gives the position of a substring, or -1 if not found
- Syntax: `string.find(sub)`
```python
s = "hello world"
print(s.find("world"))
# Output: 6
```

### rfind()
- What it does: same as find(), but searches from the right side
- Syntax: `string.rfind(sub)`
```python
s = "hello world world"
print(s.rfind("world"))
# Output: 12
```

### index()
- What it does: same as find(), but gives an error if substring not found
- Syntax: `string.index(sub)`
```python
s = "hello world"
print(s.index("world"))
# Output: 6
```

### rindex()
- What it does: same as rfind(), but gives an error if substring not found
- Syntax: `string.rindex(sub)`
```python
s = "hello world world"
print(s.rindex("world"))
# Output: 12
```

### count()
- What it does: counts how many times a substring appears
- Syntax: `string.count(sub)`
```python
s = "banana"
print(s.count("a"))
# Output: 3
```

### startswith()
- What it does: checks if string starts with given text
- Syntax: `string.startswith(sub)`
```python
s = "hello world"
print(s.startswith("hello"))
# Output: True
```

### endswith()
- What it does: checks if string ends with given text
- Syntax: `string.endswith(sub)`
```python
s = "hello world"
print(s.endswith("world"))
# Output: True
```

---

## 7. Replace

### replace()
- What it does: replaces old text with new text everywhere in the string
- Syntax: `string.replace(old, new)`
```python
s = "I like cats"
print(s.replace("cats", "dogs"))
# Output: I like dogs
```

---

## 8. Split / Join

### split()
- What it does: breaks a string into a list of pieces using a separator (default is space)
- Syntax: `string.split(sep)`
```python
s = "one two three"
print(s.split())
# Output: ['one', 'two', 'three']
```

### rsplit()
- What it does: same as split(), but starts splitting from the right side
- Syntax: `string.rsplit(sep, maxsplit)`
```python
s = "one two three"
print(s.rsplit(" ", 1))
# Output: ['one two', 'three']
```

### splitlines()
- What it does: breaks a string into a list, splitting at line breaks
- Syntax: `string.splitlines()`
```python
s = "line1\nline2\nline3"
print(s.splitlines())
# Output: ['line1', 'line2', 'line3']
```

### join()
- What it does: joins a list of strings into one string, using a separator
- Syntax: `separator.join(list)`
```python
words = ["one", "two", "three"]
print("-".join(words))
# Output: one-two-three
```

---

## 9. Strip / Trim

### strip()
- What it does: removes extra spaces (or given characters) from both ends
- Syntax: `string.strip(chars)`
```python
s = "   hello   "
print(s.strip())
# Output: hello
```

### lstrip()
- What it does: removes extra spaces (or given characters) from the left side only
- Syntax: `string.lstrip(chars)`
```python
s = "   hello   "
print(s.lstrip())
# Output: hello   
```

### rstrip()
- What it does: removes extra spaces (or given characters) from the right side only
- Syntax: `string.rstrip(chars)`
```python
s = "   hello   "
print(s.rstrip())
# Output:    hello
```

---

## 10. Check / Validate (is* methods)

### isalpha()
- What it does: checks if all characters are letters only
- Syntax: `string.isalpha()`
```python
s = "hello"
print(s.isalpha())
# Output: True
```

### isdigit()
- What it does: checks if all characters are digits (0-9) only
- Syntax: `string.isdigit()`
```python
s = "12345"
print(s.isdigit())
# Output: True
```

### isalnum()
- What it does: checks if all characters are letters or digits (no symbols/spaces)
- Syntax: `string.isalnum()`
```python
s = "hello123"
print(s.isalnum())
# Output: True
```

### isspace()
- What it does: checks if the string is made up of only spaces
- Syntax: `string.isspace()`
```python
s = "   "
print(s.isspace())
# Output: True
```

### isupper()
- What it does: checks if all letters in the string are uppercase
- Syntax: `string.isupper()`
```python
s = "HELLO"
print(s.isupper())
# Output: True
```

### islower()
- What it does: checks if all letters in the string are lowercase
- Syntax: `string.islower()`
```python
s = "hello"
print(s.islower())
# Output: True
```

### istitle()
- What it does: checks if every word starts with a capital letter
- Syntax: `string.istitle()`
```python
s = "Hello World"
print(s.istitle())
# Output: True
```

### isnumeric()
- What it does: checks if all characters are numbers (includes fractions, Roman numerals etc.)
- Syntax: `string.isnumeric()`
```python
s = "12345"
print(s.isnumeric())
# Output: True
```

### isdecimal()
- What it does: checks if all characters are decimal digits (0-9) only, stricter than isnumeric
- Syntax: `string.isdecimal()`
```python
s = "12345"
print(s.isdecimal())
# Output: True
```

---

## 11. Format

### format()
- What it does: inserts values into placeholders `{}` inside a string
- Syntax: `string.format(values)`
```python
s = "My name is {} and I am {}"
print(s.format("Abhishek", 25))
# Output: My name is Abhishek and I am 25
```

### format_map()
- What it does: inserts values into placeholders using a dictionary
- Syntax: `string.format_map(dict)`
```python
s = "My name is {name}"
data = {"name": "Abhishek"}
print(s.format_map(data))
# Output: My name is Abhishek
```

---

## 12. Alignment / Padding

### zfill()
- What it does: adds zeros in front of the string until it reaches given length
- Syntax: `string.zfill(width)`
```python
s = "42"
print(s.zfill(5))
# Output: 00042
```

### center()
- What it does: centers the string, padding both sides with a character (default space)
- Syntax: `string.center(width, char)`
```python
s = "hi"
print(s.center(10, "*"))
# Output: ****hi****
```

### ljust()
- What it does: pushes the string to the left, padding the right side
- Syntax: `string.ljust(width, char)`
```python
s = "hi"
print(s.ljust(10, "*"))
# Output: hi********
```

### rjust()
- What it does: pushes the string to the right, padding the left side
- Syntax: `string.rjust(width, char)`
```python
s = "hi"
print(s.rjust(10, "*"))
# Output: ********hi
```

---

## 13. Encoding

### encode()
- What it does: converts a string into bytes, using a given encoding (default UTF-8)
- Syntax: `string.encode(encoding)`
```python
s = "hello"
print(s.encode())
# Output: b'hello'
```

---

## 14. Misc

### ord()
- What it does: gives the ASCII (number) value of a single character
- Syntax: `ord(char)`
```python
print(ord('A'))
# Output: 65
```

### chr()
- What it does: gives the character for a given ASCII (number) value — opposite of ord()
- Syntax: `chr(number)`
```python
print(chr(65))
# Output: A
```

### ASCII Reference (good to memorize for problems)
| Range | ASCII values |
|---|---|
| A-Z | 65 to 90 |
| a-z | 97 to 122 |
| 0-9 | 48 to 57 |
| space | 32 |

- Trick: lowercase = uppercase + 32 (e.g. 'A'=65, 'a'=97, diff is 32).
```python
print(ord('a') - ord('A'))
# Output: 32
```

### Practice Problem: Convert case using ord() and chr() (without upper()/lower())
- Goal: flip uppercase to lowercase and lowercase to uppercase manually, using ASCII math.
```python
s = "Hello World"
result = ""

for ch in s:
    if 'A' <= ch <= 'Z':                 # if uppercase letter
        result += chr(ord(ch) + 32)       # shift to lowercase
    elif 'a' <= ch <= 'z':                # if lowercase letter
        result += chr(ord(ch) - 32)       # shift to uppercase
    else:
        result += ch                      # keep spaces/symbols as is

print(result)
# Output: hELLO wORLD
```
- This is the same result as `s.swapcase()` — but here you build it yourself using `ord()` and `chr()`, which is what most interview/DSA string problems expect.

### len()
- What it does: gives the number of characters in a string (built-in function, not a method)
- Syntax: `len(string)`
```python
s = "hello"
print(len(s))
# Output: 5
```

### Slicing (recap)
- What it does: picks out a part of a string using start:stop:step
- Syntax: `string[start:stop:step]`
```python
s = "hello world"
print(s[0:5])
# Output: hello
```

---

## 15. Comparison Table

| Method | Purpose | Returns |
|---|---|---|
| upper() | Convert to uppercase | New string |
| lower() | Convert to lowercase | New string |
| title() | Capitalize first letter of each word | New string |
| capitalize() | Capitalize only first letter of string | New string |
| swapcase() | Swap upper/lowercase letters | New string |
| find() | Locate substring position (left to right) | Integer (-1 if not found) |
| rfind() | Locate substring position (right to left) | Integer (-1 if not found) |
| index() | Locate substring position (left to right) | Integer (error if not found) |
| rindex() | Locate substring position (right to left) | Integer (error if not found) |
| count() | Count occurrences of substring | Integer |
| startswith() | Check start of string | Boolean |
| endswith() | Check end of string | Boolean |
| replace() | Replace substring with another | New string |
| split() | Break string into list by separator | List |
| rsplit() | Break string into list from right | List |
| splitlines() | Break string into list by line breaks | List |
| join() | Join list into one string | String |
| strip() | Remove chars from both ends | New string |
| lstrip() | Remove chars from left end | New string |
| rstrip() | Remove chars from right end | New string |
| isalpha() | Check if all letters | Boolean |
| isdigit() | Check if all digits | Boolean |
| isalnum() | Check if letters/digits only | Boolean |
| isspace() | Check if all spaces | Boolean |
| isupper() | Check if all uppercase | Boolean |
| islower() | Check if all lowercase | Boolean |
| istitle() | Check if title-cased | Boolean |
| isnumeric() | Check if all numeric characters | Boolean |
| isdecimal() | Check if all decimal digits | Boolean |
| format() | Insert values into placeholders | New string |
| format_map() | Insert dict values into placeholders | New string |
| zfill() | Pad with leading zeros | New string |
| center() | Center align with padding | New string |
| ljust() | Left align with padding | New string |
| rjust() | Right align with padding | New string |
| encode() | Convert string to bytes | Bytes object |
| len() | Count characters | Integer |
| Slicing `[:]` | Extract part of string | New string |

---
*End of notes — good for quick revision before interviews or exams.*
