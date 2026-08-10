# Python Lists — Complete Study Notes

---

## 1. List Creation

```python
empty = []                          # empty list
nums = [1, 2, 3, 4]                 # list with values
mixed = [1, "two", 3.0, True]       # list with mixed data types
from_range = list(range(5))         # list from range()
nested = [[1, 2], [3, 4], [5, 6]]   # nested list (list inside list)

print(empty)
# Output: []
print(nums)
# Output: [1, 2, 3, 4]
print(from_range)
# Output: [0, 1, 2, 3, 4]
print(nested)
# Output: [[1, 2], [3, 4], [5, 6]]
```
- `[]` or `list()` makes an empty list.
- A list can hold different data types together.
- `list(range(n))` quickly builds a list of numbers.
- A nested list is just a list where each item is itself a list.

---

## 2. List Indexing & Slicing

```python
lst = [10, 20, 30, 40, 50]
#       0   1   2   3   4     (positive index)
#      -5  -4  -3  -2  -1     (negative index)

print(lst[0])         # Output: 10
print(lst[-1])        # Output: 50
print(lst[1:4])       # Output: [20, 30, 40]
print(lst[:3])        # Output: [10, 20, 30]
print(lst[2:])        # Output: [30, 40, 50]
print(lst[::-1])      # Output: [50, 40, 30, 20, 10]   (reversed)
print(lst[::2])       # Output: [10, 30, 50]           (every 2nd item)
```
- Positive index starts at 0 from the left, negative index starts at -1 from the right.
- Slicing syntax: `list[start:stop:step]` — stop index is not included.

---

## 3. List Mutability

```python
lst = [1, 2, 3]
lst[0] = 100          # allowed — lists CAN be changed in place
print(lst)
# Output: [100, 2, 3]

s = "abc"
# s[0] = "z"          # NOT allowed — strings are immutable

t = (1, 2, 3)
# t[0] = 100          # NOT allowed — tuples are immutable
```
- Lists are mutable — you can change, add, or remove items after creation.
- Strings and tuples are immutable — you can't change them in place, you must create a new one.

---

## 4. Common List Operators & Built-in Functions

```python
a = [1, 2, 3]
b = [4, 5]

print(a + b)              # Output: [1, 2, 3, 4, 5]     (concatenation)
print(a * 2)               # Output: [1, 2, 3, 1, 2, 3]  (repetition)
print(3 in a)               # Output: True                (membership check)
print(10 not in a)          # Output: True                (negative membership)

print(len(a))               # Output: 3     (number of items)
print(max(a))               # Output: 3     (largest item)
print(min(a))               # Output: 1     (smallest item)
print(sum(a))               # Output: 6     (total of all items)
print(sorted(a, reverse=True))   # Output: [3, 2, 1]   (new sorted list, original unchanged)
```
- `+` joins two lists, `*` repeats a list, `in`/`not in` check membership.
- `len, max, min, sum` are built-in functions, not methods — used as `func(list)`.
- `sorted()` returns a **new** sorted list — original list stays the same (unlike `.sort()`).

---

## 5. List Comprehension Basics

```python
squares = [x*x for x in range(5)]
print(squares)
# Output: [0, 1, 4, 9, 16]

evens = [x for x in range(10) if x % 2 == 0]
print(evens)
# Output: [0, 2, 4, 6, 8]
```
- Basic form: `[expression for item in iterable]`.
- Add a condition at the end: `[expression for item in iterable if condition]`.
- It's a short-cut way to build a list instead of writing a full for-loop.

---

## 6. Shallow Copy vs Deep Copy

```python
import copy

original = [[1, 2], [3, 4]]

shallow = copy.copy(original)          # or original.copy() / original[:]
shallow[0][0] = 999                    # changes inner list
print(original)
# Output: [[999, 2], [3, 4]]           # original also changed! (shared inner lists)

deep = copy.deepcopy(original)
deep[0][0] = 111
print(original)
# Output: [[999, 2], [3, 4]]           # original NOT changed (fully independent copy)
```
- Shallow copy makes a new outer list, but inner lists are still shared (linked).
- Deep copy makes a fully independent copy — nothing is shared, safe for nested lists.
- Use `copy.deepcopy()` from the `copy` module when your list has lists inside it.

---

## 7. Add Elements

### append()
- What it does: adds one item to the end of the list
- Syntax: `list.append(item)`
- Returns: **None** (modifies list in-place)
```python
lst = [1, 2, 3]
lst.append(4)
print(lst)
# Output: [1, 2, 3, 4]
```

### extend()
- What it does: adds all items from another iterable to the end of the list
- Syntax: `list.extend(iterable)`
- Returns: **None** (modifies list in-place)
```python
lst = [1, 2, 3]
lst.extend([4, 5])
print(lst)
# Output: [1, 2, 3, 4, 5]
```

### insert()
- What it does: adds an item at a specific position/index
- Syntax: `list.insert(index, item)`
- Returns: **None** (modifies list in-place)
```python
lst = [1, 2, 3]
lst.insert(1, 100)
print(lst)
# Output: [1, 100, 2, 3]
```

---

## 8. Remove Elements

### remove()
- What it does: removes the first matching item by value (error if not found)
- Syntax: `list.remove(item)`
- Returns: **None** (modifies list in-place)
```python
lst = [1, 2, 3, 2]
lst.remove(2)
print(lst)
# Output: [1, 3, 2]
```

### pop()
- What it does: removes and returns the item at given index (default: last item)
- Syntax: `list.pop(index)`
- Returns: **the removed item** (modifies list in-place)
```python
lst = [1, 2, 3]
item = lst.pop()
print(item)
# Output: 3
print(lst)
# Output: [1, 2]
```

### clear()
- What it does: removes all items, making the list empty
- Syntax: `list.clear()`
- Returns: **None** (modifies list in-place)
```python
lst = [1, 2, 3]
lst.clear()
print(lst)
# Output: []
```

---

## 9. Search / Find

### index()
- What it does: gives the position of the first matching item (error if not found)
- Syntax: `list.index(item)`
- Returns: **an integer (index position)**
```python
lst = [10, 20, 30]
print(lst.index(20))
# Output: 1
```

---

## 10. Sort / Reverse

### sort()
- What it does: sorts the list in-place in ascending order (or descending with reverse=True)
- Syntax: `list.sort(reverse=False, key=None)`
- Returns: **None** (modifies list in-place)
```python
lst = [3, 1, 2]
lst.sort()
print(lst)
# Output: [1, 2, 3]
```

### reverse()
- What it does: reverses the order of items in the list, in-place
- Syntax: `list.reverse()`
- Returns: **None** (modifies list in-place)
```python
lst = [1, 2, 3]
lst.reverse()
print(lst)
# Output: [3, 2, 1]
```

---

## 11. Copy

### copy()
- What it does: makes a shallow copy of the list (new outer list)
- Syntax: `list.copy()`
- Returns: **a new list**
```python
lst = [1, 2, 3]
new_lst = lst.copy()
new_lst.append(4)
print(lst)
# Output: [1, 2, 3]
print(new_lst)
# Output: [1, 2, 3, 4]
```

---

## 12. Count

### count()
- What it does: counts how many times a value appears in the list
- Syntax: `list.count(item)`
- Returns: **an integer**
```python
lst = [1, 2, 2, 3, 2]
print(lst.count(2))
# Output: 3
```

---

## 13. Misc

### len() (built-in function)
- What it does: gives the number of items in the list
- Syntax: `len(list)`
- Returns: **an integer**
```python
lst = [1, 2, 3, 4]
print(len(lst))
# Output: 4
```

---

## 14. Comparison Table

| Method | Purpose | Returns | Modifies Original? |
|---|---|---|---|
| append() | Add one item to the end | None | Yes |
| extend() | Add multiple items to the end | None | Yes |
| insert() | Add item at a given position | None | Yes |
| remove() | Remove first matching item by value | None | Yes |
| pop() | Remove & return item at given index | Removed item | Yes |
| clear() | Remove all items | None | Yes |
| index() | Find position of first matching item | Integer | No |
| count() | Count how many times value appears | Integer | No |
| sort() | Sort the list in ascending/descending order | None | Yes |
| reverse() | Reverse the order of items | None | Yes |
| copy() | Make a shallow copy of the list | New list | No |
| len() | Count total items (built-in function) | Integer | No |
| sorted() | Get a new sorted list (built-in function) | New list | No |
| max() | Get the largest item (built-in function) | Item value | No |
| min() | Get the smallest item (built-in function) | Item value | No |
| sum() | Get the total of all items (built-in function) | Number | No |

---
*End of notes — good for quick revision before interviews or exams.*
