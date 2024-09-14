---
title: Python OOP Tutorial 🚀
---

# Python OOP Tutorial: Exploring Different Types of Attributes 🚀

---

### Table of Contents 📚

1. [Introduction to Object-Oriented Programming (OOP) in Python 🐍](#1-introduction-to-object-oriented-programming-oop-in-python)
2. [Instance Attributes 🌐](#2-instance-attributes)
   - [Example: Instance Attribute](#example-instance-attribute)
3. [Class Attributes 📚](#3-class-attributes)
   - [Example: Class Attribute](#example-class-attribute)
4. [Public Attributes 🌍](#4-public-attributes)
   - [Example: Public Attribute](#example-public-attribute)
5. [Private Attributes 🔒](#5-private-attributes)
   - [Example: Private Attribute](#example-private-attribute)
   - [Name Mangling in Python](#name-mangling-in-python)
6. [Protected Attributes 🛡️](#6-protected-attributes)
   - [Example: Protected Attribute](#example-protected-attribute)
7. [Special (Magic) Attributes ✨](#7-special-magic-attributes)
   - [Example: Special (Magic) Attribute](#example-special-magic-attribute)
8. [Summary 📝](#8-summary)

---

#### 1. Introduction to Object-Oriented Programming (OOP) in Python 🐍

Object-Oriented Programming (OOP) is a powerful paradigm in Python that allows developers to structure code using objects and classes. Attributes are a crucial part of OOP, and understanding the different types of attributes is key to writing clean and efficient Python code.

In this tutorial, we will cover the following types of attributes in Python:

- **Instance Attributes**
- **Class Attributes**
- **Public Attributes**
- **Private Attributes**
- **Protected Attributes**
- **Special (Magic) Attributes**

---

#### 2. Instance Attributes 🌐

**Instance attributes** are specific to each instance of a class. This means that different objects created from the same class can have different values for these attributes.

##### Example: Instance Attribute

```python
class MyClass:
    def __init__(self, value):
        self.instance_attr = value  # Instance attribute

# Creating instances of MyClass
obj1 = MyClass(42)
obj2 = MyClass(100)

print(obj1.instance_attr)  # Output: 42
print(obj2.instance_attr)  # Output: 100
```

---

#### 3. Class Attributes 📚

**Class attributes** are shared across all instances of a class. They are defined within the class but outside any instance methods, meaning all instances share the same value.

##### Example: Class Attribute

```python
class MyClass:
    class_attr = "I am a class attribute"  # Class attribute

# Creating instances of MyClass
obj1 = MyClass()
obj2 = MyClass()

print(obj1.class_attr)  # Output: I am a class attribute
print(obj2.class_attr)  # Output: I am a class attribute
```

---

#### 4. Public Attributes 🌍

**Public attributes** are accessible from anywhere, both inside and outside the class. By default, all attributes in Python are public unless specified otherwise.

##### Example: Public Attribute

```python
class MyClass:
    def __init__(self, value):
        self.public_attr = value  # Public attribute

# Creating an instance of MyClass
obj = MyClass(42)

# Accessing and modifying the public attribute
print(obj.public_attr)  # Output: 42
obj.public_attr = 100
print(obj.public_attr)  # Output: 100
```

---

#### 5. Private Attributes 🔒

**Private attributes** are intended to be hidden from outside access. In Python, you can define a private attribute by prefixing its name with two underscores (`__`). This makes it inaccessible directly from outside the class, though it remains accessible within the class.

##### Example: Private Attribute

```python
class MyClass:
    def __init__(self, value):
        self.__private_attr = value  # Private attribute

    def get_private_attr(self):
        return self.__private_attr  # Accessing private attribute within the class

# Creating an instance of MyClass
obj = MyClass(42)

# Attempting to access the private attribute directly will raise an error
# print(obj.__private_attr)  # Uncommenting this will raise an AttributeError

# Accessing the private attribute through a method
print(obj.get_private_attr())  # Output: 42
```

##### Name Mangling in Python

Private attributes use **name mangling** to change their names, making them accessible through a special syntax (though it's generally not recommended to use this).

```python
# Accessing the private attribute using name mangling
print(obj._MyClass__private_attr)  # Output: 42
```

---

#### 6. Protected Attributes 🛡️

**Protected attributes** are intended to be accessed only within the class and its subclasses. They are defined by prefixing the attribute name with a single underscore (`_`). This is a convention indicating that it should not be accessed directly.

##### Example: Protected Attribute

```python
class MyClass:
    def __init__(self, value):
        self._protected_attr = value  # Protected attribute

    def get_protected_attr(self):
        return self._protected_attr  # Accessing protected attribute within the class

class SubClass(MyClass):
    def access_protected_attr(self):
        return self._protected_attr  # Accessing protected attribute within a subclass

# Creating an instance of SubClass
sub_obj = SubClass(42)
print(sub_obj.access_protected_attr())  # Output: 42
```

---

#### 7. Special (Magic) Attributes ✨

**Special (Magic) attributes** are built-in attributes in Python that have special meanings and are typically surrounded by double underscores (`__`). Examples include `__init__`, `__str__`, and `__repr__`.

##### Example: Special (Magic) Attribute

```python
class MyClass:
    def __init__(self, value):
        self.value = value

    def __str__(self):
        return f"MyClass with value: {self.value}"  # Special (magic) method

# Creating an instance of MyClass
obj = MyClass(42)

# The __str__ method is called when we print the object
print(obj)  # Output: MyClass with value: 42
```

---

#### 8. Summary 📝

In this tutorial, we've covered the various types of attributes in Python OOP:

- **Instance Attributes**: Unique to each object.
- **Class Attributes**: Shared across all instances of a class.
- **Public Attributes**: Accessible from anywhere.
- **Private Attributes**: Intended to be hidden and prefixed with `__`.
- **Protected Attributes**: Intended for use within the class and subclasses, prefixed with `_`.
- **Special (Magic) Attributes**: Built-in attributes with special behavior, typically surrounded by double underscores.

Understanding these types of attributes will help you better structure and manage your Python code, ensuring that your data is properly encapsulated and accessed as intended.

---
