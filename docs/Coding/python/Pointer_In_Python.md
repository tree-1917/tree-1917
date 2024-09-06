---
title: 📘  Understanding How Python Handles Non-Primitive Data Types (Objects and Arrays)
---

### 📘 Tutorial: Understanding How Python Handles Non-Primitive Data Types (Objects and Arrays)

Python’s handling of objects and arrays (lists) often confuses developers coming from other languages like C or Java. In Python, variables do not store the actual value or object directly; instead, they store references to the objects. This means that Python **always passes objects by reference** when they are assigned or passed to a function.

In this tutorial, we’ll dive into how Python handles non-primitive data types like objects and lists, and explore how we can simulate pointer-like behavior using Python’s capabilities.

### 📝 Table of Contents

1. [Primitive vs. Non-Primitive Data Types](#1-primitive-vs-non-primitive-data-types)
2. [Understanding References in Python](#2-understanding-references-in-python)
3. [Examples of Pass-by-Reference Behavior](#3-examples-of-pass-by-reference-behavior)
4. [How to Simulate Pointers in Python](#4-how-to-simulate-pointers-in-python)
5. [Conclusion](#5-conclusion)

---

### 1. Primitive vs. Non-Primitive Data Types

- **Primitive Data Types**: These include integers, floats, strings, and booleans. They are **immutable**, meaning that their values cannot be changed once created.
- **Non-Primitive Data Types**: These include lists, dictionaries, sets, and custom objects. They are **mutable**, meaning that their values can be changed after they are created.

When you assign a primitive data type, you are copying the value. When you assign a non-primitive data type, you are copying the reference to the memory location where the object is stored.

### 2. Understanding References in Python

In Python, **variables are references** to objects stored in memory. When you assign a variable to another variable, you are copying the reference, not the actual object.

#### Example:

```python
# Primitive Data Type Example
a = 10
b = a  # Copying the value
b += 5  # Modifying 'b' does not affect 'a'

print(a)  # Output: 10
print(b)  # Output: 15

# Non-Primitive Data Type Example (List)
x = [1, 2, 3]
y = x  # Copying the reference
y.append(4)  # Modifying 'y' also affects 'x'

print(x)  # Output: [1, 2, 3, 4]
print(y)  # Output: [1, 2, 3, 4]
```

In the above example:

- `a` and `b` are **independent** after assignment because `a` is a primitive type.
- `x` and `y` are **interdependent** after assignment because they refer to the **same list object** in memory.

### 3. Examples of Pass-by-Reference Behavior

Python’s handling of non-primitive types can be seen when passing lists or objects to functions. If a list or an object is modified inside the function, the changes are reflected outside the function because the reference to the object is passed, not a copy.

#### Example:

```python
def modify_list(lst):
    lst.append(100)  # Modifies the original list

my_list = [1, 2, 3]
modify_list(my_list)

print(my_list)  # Output: [1, 2, 3, 100]
```

Here, `my_list` is passed by reference to the `modify_list` function. Modifications to `lst` inside the function directly affect `my_list` because they refer to the same object in memory.

### 4. How to Simulate Pointers in Python

Python does not support pointers as in languages like C or C++. However, you can simulate pointer-like behavior using references. You can also use **mutable types** (like lists or dictionaries) to create a form of pointer behavior.

#### Example: Simulating Pointers with Lists

```python
# Simulating Pointer Behavior with Lists
class Pointer:
    def __init__(self, value):
        self.ref = value  # Reference to another object

def modify(pointer_obj):
    pointer_obj.ref[0] = 100  # Modifying the first element

data = [10, 20, 30]
ptr = Pointer(data)  # Pointer-like reference to the 'data' list

print("Before modification:", ptr.ref)  # Output: [10, 20, 30]
modify(ptr)  # Modify the list through the 'pointer'
print("After modification:", ptr.ref)  # Output: [100, 20, 30]
```

In this example:

- `Pointer` class simulates a pointer by holding a reference to a list.
- When `modify()` changes the list through `Pointer`, it modifies the original `data` list.

### 5. Conclusion

- Python handles **non-primitive data types** (like lists and objects) by reference. This means when you assign them or pass them to a function, you're working with a reference to the original object.
- **Simulating Pointers**: You can simulate pointers in Python by using mutable types and classes to hold references, allowing you to create pointer-like behavior.

By understanding how Python manages memory and references, you can write more efficient code and avoid common pitfalls related to mutable objects.
