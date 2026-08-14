# Python OOP — Complete Study Notes

---

## 1. BASICS

### Class vs Object
- What it is: A class is a blueprint/design; an object is the actual thing built from that blueprint.
- Syntax:
```python
class ClassName:
    pass

obj = ClassName()
```
- Example:
```python
class Dog:
    pass

d = Dog()
print(type(d))
# Output: <class '__main__.Dog'>
```

### Attributes vs Methods
- What it is: Attributes are variables that store data about an object; methods are functions that define what an object can do.
- Syntax:
```python
class ClassName:
    attribute = value        # attribute
    def method(self):        # method
        pass
```
- Example:
```python
class Dog:
    sound = "Bark"            # attribute
    def make_sound(self):     # method
        return self.sound

d = Dog()
print(d.sound)
# Output: Bark
print(d.make_sound())
# Output: Bark
```

### self keyword
- What it is: `self` refers to the current object — it's how a method accesses that object's own data.
- Syntax:
```python
def method(self, ...):
    self.attribute
```
- Example:
```python
class Dog:
    def bark(self):
        return "Woof, I am a dog object"

d = Dog()
print(d.bark())
# Output: Woof, I am a dog object
```

### __init__ constructor
- What it is: A special method that runs automatically when an object is created — used to set up initial values.
- Syntax:
```python
def __init__(self, param1, param2):
    self.param1 = param1
```
- Example:
```python
class Dog:
    def __init__(self, name):
        self.name = name

d = Dog("Tommy")
print(d.name)
# Output: Tommy
```

---

## 2. FOUR PILLARS OF OOP

### Encapsulation
- What it is: Bundling data and methods together, and restricting direct access to some data using `_` (protected) or `__` (private).
- Syntax:
```python
class ClassName:
    def __init__(self):
        self._protected_var = value
        self.__private_var = value
```
- Example:
```python
class Account:
    def __init__(self, balance):
        self._protected_bal = balance      # protected — convention only
        self.__private_bal = balance       # private — name-mangled

acc = Account(1000)
print(acc._protected_bal)
# Output: 1000
# print(acc.__private_bal)      # Error: AttributeError
print(acc._Account__private_bal)
# Output: 1000   (private vars are accessible via name mangling, but shouldn't be)
```

### Inheritance — Single
- What it is: A child class gets all the attributes and methods of one parent class.
- Syntax:
```python
class Parent:
    pass
class Child(Parent):
    pass
```
- Example:
```python
class Animal:
    def eat(self):
        return "eating"

class Dog(Animal):
    def bark(self):
        return "barking"

d = Dog()
print(d.eat(), d.bark())
# Output: eating barking
```

### Inheritance — Multiple
- What it is: A child class inherits from more than one parent class at the same time.
- Syntax:
```python
class Child(Parent1, Parent2):
    pass
```
- Example:
```python
class Father:
    def skill(self):
        return "Cooking"

class Mother:
    def talent(self):
        return "Painting"

class Child(Father, Mother):
    pass

c = Child()
print(c.skill(), c.talent())
# Output: Cooking Painting
```

### Inheritance — Multilevel
- What it is: A chain of inheritance — a class inherits from a child class, which inherits from another parent.
- Syntax:
```python
class A: pass
class B(A): pass
class C(B): pass
```
- Example:
```python
class Animal:
    def eat(self):
        return "eating"

class Dog(Animal):
    def bark(self):
        return "barking"

class Puppy(Dog):
    def weep(self):
        return "weeping"

p = Puppy()
print(p.eat(), p.bark(), p.weep())
# Output: eating barking weeping
```

### Polymorphism — Method Overriding
- What it is: A child class redefines a method that already exists in its parent class, with new behavior.
- Syntax:
```python
class Parent:
    def method(self): pass
class Child(Parent):
    def method(self): pass   # overridden
```
- Example:
```python
class Animal:
    def sound(self):
        return "Some sound"

class Cat(Animal):
    def sound(self):
        return "Meow"

a = Animal()
c = Cat()
print(a.sound(), c.sound())
# Output: Some sound Meow
```

### Polymorphism — Duck Typing
- What it is: Python doesn't check the object's type — if it has the method you're calling, it just works ("if it quacks like a duck...").
- Syntax:
```python
def func(obj):
    obj.method()   # works for any object that has this method
```
- Example:
```python
class Duck:
    def sound(self):
        return "Quack"

class Human:
    def sound(self):
        return "I can quack too!"

def make_it_sound(thing):
    return thing.sound()

print(make_it_sound(Duck()))
# Output: Quack
print(make_it_sound(Human()))
# Output: I can quack too!
```

### Abstraction
- What it is: Hiding internal details and showing only essential features — using the `abc` module to force child classes to implement certain methods.
- Syntax:
```python
from abc import ABC, abstractmethod
class ClassName(ABC):
    @abstractmethod
    def method(self): pass
```
- Example:
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, r):
        self.r = r
    def area(self):
        return 3.14 * self.r * self.r

c = Circle(5)
print(c.area())
# Output: 78.5
# Shape()   # Error: Can't instantiate abstract class Shape
```

---

## 3. CONSTRUCTORS & VARIABLES

### Instance Variables vs Class Variables
- What it is: Instance variables are unique to each object; class variables are shared by all objects of the class.
- Syntax:
```python
class ClassName:
    class_var = value          # shared by all objects
    def __init__(self):
        self.instance_var = value   # unique per object
```
- Example:
```python
class Dog:
    species = "Canine"          # class variable — shared

    def __init__(self, name):
        self.name = name        # instance variable — unique

d1 = Dog("Tommy")
d2 = Dog("Rocky")
print(d1.name, d2.name)
# Output: Tommy Rocky
print(d1.species, d2.species)
# Output: Canine Canine
```

### Constructor Overloading (via default args)
- What it is: Python has no true method overloading — instead, use default argument values to mimic multiple constructors.
- Syntax:
```python
def __init__(self, param1=None, param2=None):
    pass
```
- Example:
```python
class Dog:
    def __init__(self, name=None):
        self.name = name if name else "Unnamed"

d1 = Dog()
d2 = Dog("Tommy")
print(d1.name, d2.name)
# Output: Unnamed Tommy
```

### Destructor (__del__)
- What it is: A special method that runs automatically when an object is about to be destroyed/deleted.
- Syntax:
```python
def __del__(self):
    pass
```
- Example:
```python
class Dog:
    def __del__(self):
        print("Dog object destroyed")

d = Dog()
del d
# Output: Dog object destroyed
```

---

## 4. ACCESS MODIFIERS

### Public, Protected, Private
- What it is: Public vars are open to everyone, protected (`_var`) are meant for internal/child-class use only (by convention), private (`__var`) are name-mangled to discourage outside access.
- Syntax:
```python
self.public_var = value
self._protected_var = value
self.__private_var = value
```
- Example:
```python
class Demo:
    def __init__(self):
        self.public_var = "I am public"
        self._protected_var = "I am protected"
        self.__private_var = "I am private"

d = Demo()
print(d.public_var)
# Output: I am public
print(d._protected_var)
# Output: I am protected   (accessible, but convention says don't touch outside class)
# print(d.__private_var)   # Error: AttributeError
print(d._Demo__private_var)
# Output: I am private   (only accessible via name mangling)
```

---

## 5. METHOD TYPES

### Instance Method
- What it is: A normal method that works on a specific object's data, using `self` — use when you need to access/modify object data.
- Syntax:
```python
def method(self):
    pass
```
- Example:
```python
class Dog:
    def __init__(self, name):
        self.name = name
    def show(self):
        return f"Dog name: {self.name}"

d = Dog("Tommy")
print(d.show())
# Output: Dog name: Tommy
```

### Class Method (@classmethod)
- What it is: A method that works on the class itself (using `cls`), not on individual objects — use when you need to access/modify class-level data.
- Syntax:
```python
@classmethod
def method(cls):
    pass
```
- Example:
```python
class Dog:
    count = 0
    def __init__(self):
        Dog.count += 1

    @classmethod
    def total_dogs(cls):
        return cls.count

d1 = Dog()
d2 = Dog()
print(Dog.total_dogs())
# Output: 2
```

### Static Method (@staticmethod)
- What it is: A method that doesn't use `self` or `cls` — just a regular function placed inside a class for organization — use for utility/helper logic.
- Syntax:
```python
@staticmethod
def method():
    pass
```
- Example:
```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b

print(MathUtils.add(3, 5))
# Output: 8
```

---

## 6. SPECIAL / DUNDER METHODS

### __str__ vs __repr__
- What it is: `__str__` gives a readable string for end users (used by `print()`); `__repr__` gives an unambiguous string for developers/debugging.
- Syntax:
```python
def __str__(self):
    return "readable version"
def __repr__(self):
    return "developer version"
```
- Example:
```python
class Dog:
    def __str__(self):
        return "Dog(friendly pet)"
    def __repr__(self):
        return "Dog(debug_info=True)"

d = Dog()
print(str(d))
# Output: Dog(friendly pet)
print(repr(d))
# Output: Dog(debug_info=True)
```

### __eq__, __len__, __add__ (Operator Overloading)
- What it is: Dunder methods let you define how built-in operators (`==`, `len()`, `+`) behave on your own objects.
- Syntax:
```python
def __eq__(self, other): pass
def __len__(self): pass
def __add__(self, other): pass
```
- Example:
```python
class Box:
    def __init__(self, size):
        self.size = size
    def __eq__(self, other):
        return self.size == other.size
    def __len__(self):
        return self.size
    def __add__(self, other):
        return Box(self.size + other.size)

b1 = Box(5)
b2 = Box(5)
b3 = Box(3)
print(b1 == b2)
# Output: True
print(len(b1))
# Output: 5
print((b1 + b3).size)
# Output: 8
```

### super() usage
- What it is: `super()` lets a child class call methods from its parent class — commonly used inside `__init__`.
- Syntax:
```python
class Child(Parent):
    def __init__(self):
        super().__init__()
```
- Example:
```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)     # calls Animal's __init__
        self.breed = breed

d = Dog("Tommy", "Labrador")
print(d.name, d.breed)
# Output: Tommy Labrador
```

---

## 7. ADVANCED CONCEPTS

### Multiple Inheritance & MRO
- What it is: When a class inherits from multiple classes, Python decides which parent's method to use first based on Method Resolution Order (left to right).
- Syntax:
```python
class Child(Parent1, Parent2):
    pass
print(Child.__mro__)
```
- Example:
```python
class A:
    def show(self):
        return "A"

class B:
    def show(self):
        return "B"

class C(A, B):
    pass

c = C()
print(c.show())
# Output: A     (A comes first in inheritance list, so its method wins)
print(C.__mro__)
# Output: (<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)
```

### Composition vs Inheritance
- What it is: Inheritance is an "is-a" relationship (Dog IS AN Animal); composition is a "has-a" relationship (Car HAS AN Engine) — build objects using other objects instead of inheriting.
- Syntax:
```python
class Engine:
    pass
class Car:
    def __init__(self):
        self.engine = Engine()    # composition — "has-a"
```
- Example:
```python
class Engine:
    def start(self):
        return "Engine started"

class Car:
    def __init__(self):
        self.engine = Engine()     # Car HAS AN Engine
    def start(self):
        return self.engine.start()

c = Car()
print(c.start())
# Output: Engine started
```

### Interfaces in Python (via Abstract Classes)
- What it is: Python has no `interface` keyword — abstract classes with only abstract methods act as interfaces, forcing child classes to implement them.
- Syntax:
```python
from abc import ABC, abstractmethod
class InterfaceName(ABC):
    @abstractmethod
    def method(self): pass
```
- Example:
```python
from abc import ABC, abstractmethod

class Printable(ABC):
    @abstractmethod
    def print_me(self):
        pass

class Report(Printable):
    def print_me(self):
        return "Printing report..."

r = Report()
print(r.print_me())
# Output: Printing report...
```

---

## 8. Comparison Table

| Concept | Purpose | Example Keyword/Syntax |
|---|---|---|
| Class | Blueprint for creating objects | `class Name:` |
| Object | Real instance created from a class | `obj = Name()` |
| self | Refers to the current object | `def method(self):` |
| __init__ | Sets up initial data when object is created | `def __init__(self):` |
| Encapsulation | Restrict access to internal data | `_var`, `__var` |
| Inheritance | Child class reuses parent class code | `class Child(Parent):` |
| Polymorphism | Same method name, different behavior | Method overriding |
| Abstraction | Hide details, force child to implement | `abstractmethod` |
| Instance variable | Data unique to each object | `self.var = value` |
| Class variable | Data shared across all objects | `class_var = value` |
| Constructor overloading | Simulate multiple constructors | Default args `param=None` |
| Destructor | Cleanup when object is deleted | `def __del__(self):` |
| Instance method | Works on object data | `def method(self):` |
| Class method | Works on class-level data | `@classmethod` |
| Static method | Independent utility function in class | `@staticmethod` |
| __str__ | Readable string for users | `print(obj)` |
| __repr__ | Debug-friendly string for developers | `repr(obj)` |
| Operator overloading | Custom behavior for `+`, `==`, `len()` | `__add__`, `__eq__`, `__len__` |
| super() | Call parent class methods | `super().__init__()` |
| MRO | Order Python checks classes for methods | `Class.__mro__` |
| Composition | "has-a" relationship between objects | Object as attribute |
| Interface | Contract that forces implementation | `ABC` + `abstractmethod` |

---

## 9. Common Interview Questions

**Q1: What is the difference between a class and an object?**
A: A class is the blueprint/design; an object is the actual instance created from it.

**Q2: Why do we use `self` in every method?**
A: `self` lets a method know which specific object's data it should work with.

**Q3: What's the difference between `__str__` and `__repr__`?**
A: `__str__` is for a readable, user-facing message; `__repr__` is for an exact, debug-friendly message.

**Q4: Can Python have true method overloading like Java/C++?**
A: No — Python simulates it using default argument values instead.

**Q5: What is the difference between encapsulation and abstraction?**
A: Encapsulation hides *data* by restricting access; abstraction hides *implementation details* by showing only essential behavior.

**Q6: What is MRO in Python?**
A: MRO (Method Resolution Order) is the order Python follows to search for a method across multiple parent classes, left to right.

---
*End of notes — good for quick revision before interviews or exams.*
