# Python Tutorial: Understanding Decorators 🚀

---

### Table of Contents 📚

1. [Introduction to Decorators 🐍](#1-introduction-to-decorators)
2. [Basic Function Decorators 💡](#2-basic-function-decorators)
   - [Example: Basic Function Decorator](#example-basic-function-decorator)
3. [Decorating Functions with Arguments 🎯](#3-decorating-functions-with-arguments)
   - [Example: Decorator with Arguments](#example-decorator-with-arguments)
4. [Returning Values from Decorated Functions 🔄](#4-returning-values-from-decorated-functions)
   - [Example: Decorator Returning Values](#example-decorator-returning-values)
5. [Chaining Multiple Decorators 🔗](#5-chaining-multiple-decorators)
   - [Example: Chaining Decorators](#example-chaining-decorators)
6. [Class-based Decorators 🏛️](#6-class-based-decorators)
   - [Example: Class-based Decorator](#example-class-based-decorator)
7. [Built-in Python Decorators 🎉](#7-built-in-python-decorators)
   - [Example: `@staticmethod`, `@classmethod`, and `@property`](#example-staticmethod-classmethod-and-property)
8. [Practical Use Cases for Decorators 🛠️](#8-practical-use-cases-for-decorators)
   - [Example: Logging, Timing, and Access Control](#example-logging-timing-and-access-control)
9. [Summary 📝](#9-summary)

---

#### 1. Introduction to Decorators 🐍

Decorators in Python are a powerful tool that allows you to modify the behavior of a function or class. They are essentially functions that take another function as an argument, extend or modify its behavior, and return a new function with the extended behavior.

In this tutorial, we'll explore how decorators work, how to create and apply them, and some practical use cases.

---

#### 2. Basic Function Decorators 💡

A decorator is a function that wraps another function. The basic syntax for a decorator involves defining a function and then applying it to another function using the `@` symbol.

##### Example: Basic Function Decorator

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

# When you call the function, it is automatically decorated
say_hello()

# Output:
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```

---

#### 3. Decorating Functions with Arguments 🎯

Decorators can also handle functions that take arguments. To do this, the inner wrapper function must accept `*args` and `**kwargs` and pass them to the original function.

##### Example: Decorator with Arguments

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Function is called with:", args, kwargs)
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")
greet("Bob", greeting="Hi")

# Output:
# Function is called with: ('Alice',) {}
# Hello, Alice!
# Function is called with: ('Bob',) {'greeting': 'Hi'}
# Hi, Bob!
```

---

#### 4. Returning Values from Decorated Functions 🔄

A decorator can also modify the return value of a function. To return the original function's result, simply return the result of the decorated function inside the wrapper.

##### Example: Decorator Returning Values

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@my_decorator
def add(a, b):
    return a + b

result = add(5, 3)
print("Result:", result)

# Output:
# Before function call
# After function call
# Result: 8
```

---

#### 5. Chaining Multiple Decorators 🔗

You can apply multiple decorators to a single function by stacking them on top of each other. The decorators are applied from the bottom up.

##### Example: Chaining Decorators

```python
def decorator1(func):
    def wrapper(*args, **kwargs):
        print("Decorator 1")
        return func(*args, **kwargs)
    return wrapper

def decorator2(func):
    def wrapper(*args, **kwargs):
        print("Decorator 2")
        return func(*args, **kwargs)
    return wrapper

@decorator1
@decorator2
def say_hello():
    print("Hello!")

say_hello()

# Output:
# Decorator 1
# Decorator 2
# Hello!
```

---

#### 6. Class-based Decorators 🏛️

Decorators can also be implemented using classes. A class-based decorator is a class with a `__call__` method, which allows instances of the class to be used as decorators.

##### Example: Class-based Decorator

```python
class MyDecorator:
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        print("Class-based decorator")
        return self.func(*args, **kwargs)

@MyDecorator
def say_hello():
    print("Hello from a class-based decorator!")

say_hello()

# Output:
# Class-based decorator
# Hello from a class-based decorator!
```

---

#### 7. Built-in Python Decorators 🎉

Python provides several built-in decorators that are commonly used:

- `@staticmethod`: Converts a method into a static method.
- `@classmethod`: Converts a method into a class method.
- `@property`: Allows you to define methods that behave like attributes.

##### Example: `@staticmethod`, `@classmethod`, and `@property`

```python
class MyClass:
    @staticmethod
    def static_method():
        print("This is a static method.")

    @classmethod
    def class_method(cls):
        print(f"This is a class method of {cls}.")

    def __init__(self, value):
        self._value = value

    @property
    def value(self):
        return self._value

    @value.setter
    def value(self, new_value):
        self._value = new_value

# Using the built-in decorators
MyClass.static_method()
MyClass.class_method()

obj = MyClass(10)
print(obj.value)  # Accessing the property
obj.value = 20   # Setting the property
print(obj.value)
```

---

#### 8. Practical Use Cases for Decorators 🛠️

Decorators are widely used in Python for tasks such as logging, timing functions, and access control.

##### Example: Logging, Timing, and Access Control

```python
import time

def timer_decorator(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function {func.__name__} took {end_time - start_time} seconds")
        return result
    return wrapper

@timer_decorator
def slow_function():
    time.sleep(2)
    print("Finished slow function")

slow_function()

# Output:
# Finished slow function
# Function slow_function took 2.0xxxx seconds
```

---

#### 9. Summary 📝

Decorators in Python provide a powerful way to extend the functionality of functions or methods without modifying their code. They can be used for a wide range of tasks, from logging and timing to access control and more. Whether you're using function-based or class-based decorators, understanding how they work will help you write more flexible and maintainable code.

---

