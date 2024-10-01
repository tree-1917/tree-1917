---
title: 🚀 Caching in Python`
---

# 🚀 Caching in Python: How to Store and Reuse Data Efficiently

Caching is a powerful technique that helps improve the performance of applications by storing the results of expensive or time-consuming operations and reusing them for subsequent requests. This tutorial will guide you through how to implement caching in Python using the `functools` library and other methods.

## 🗂️ Table of Contents

1. [🔍 Introduction to Caching](#-introduction-to-caching)
2. [🛠️ Using `functools.lru_cache`](#️-using-functoolslru_cache)
   - [⚙️ Basic Usage of `@lru_cache`](#-basic-usage-of-lru_cache)
   - [🔧 Customizing Cache Size](#-customizing-cache-size)
   - [🧹 Clearing the Cache](#-clearing-the-cache)
3. [🧑‍💻 Examples of Caching in Python](#-examples-of-caching-in-python)
   - [🔹 Example 1: Caching a Fibonacci Function](#-example-1-caching-a-fibonacci-function)
   - [🔹 Example 2: Caching with Custom Cache Size](#-example-2-caching-with-custom-cache-size)
   - [🔹 Example 3: Manual Cache Implementation Using a Dictionary](#-example-3-manual-cache-implementation-using-a-dictionary)
4. [💡 Best Practices for Using Cache](#-best-practices-for-using-cache)
5. [📝 Conclusion](#-conclusion)

## 🔍 Introduction to Caching

Caching is a method of storing the results of expensive function calls and reusing them when the same inputs occur again. This can greatly reduce the time and resources needed to recompute results.

### Benefits of Caching:

- 🚀 **Improves Performance**: Reduces the time taken to fetch or compute data.
- 🧠 **Minimizes Resource Usage**: Lowers the number of computations or data retrieval operations.
- 😃 **Enhances User Experience**: Faster response times lead to a better user experience.

## 🛠️ Using `functools.lru_cache`

Python’s `functools` module provides an easy-to-use caching mechanism via the `@lru_cache` decorator. This decorator uses an LRU (Least Recently Used) caching strategy, which discards the least recently used items first when the cache is full.

### ⚙️ Basic Usage of `@lru_cache`

The `@lru_cache` decorator can be added above your function definition to enable caching:

```python
from functools import lru_cache

@lru_cache(maxsize=None)  # No limit on cache size
def expensive_computation(x):
    print(f"Computing {x}...")
    return x * x

# First call with 4; computation happens
print(expensive_computation(4))  # Output: 16

# Second call with 4; result is fetched from cache
print(expensive_computation(4))  # Output: 16 (Cached result)
```

**Output:**

```
Computing 4...
16
16
```

- `maxsize=None`: Cache an unlimited number of values.
- When called with the same argument again, the function does not re-execute; it returns the cached result.

### 🔧 Customizing Cache Size

You can limit the size of the cache by setting the `maxsize` parameter. This is helpful when you want to prevent excessive memory usage.

```python
from functools import lru_cache

@lru_cache(maxsize=3)  # Caches up to 3 unique calls
def expensive_computation(x):
    print(f"Computing {x}...")
    return x * x

# Using the generator
print(expensive_computation(2))  # Computes and caches
print(expensive_computation(3))  # Computes and caches
print(expensive_computation(4))  # Computes and caches
print(expensive_computation(5))  # Computes and caches, oldest cache (2) is removed
print(expensive_computation(2))  # Recomputes, since it was evicted
```

**Output:**

```
Computing 2...
4
Computing 3...
9
Computing 4...
16
Computing 5...
25
Computing 2...
4
```

### 🧹 Clearing the Cache

If you need to clear the cache at any point, you can use the `.cache_clear()` method:

```python
expensive_computation.cache_clear()
```

This method clears all cached results, forcing the function to recompute results on the next call.

## 🧑‍💻 Examples of Caching in Python

### 🔹 Example 1: Caching a Fibonacci Function

The Fibonacci sequence is a classic example where caching significantly improves performance.

```python
from functools import lru_cache

@lru_cache(maxsize=None)  # Cache all results
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# Using the cached Fibonacci function
print(fibonacci(10))  # Output: 55
print(fibonacci(20))  # Output: 6765
```

Without caching, the time complexity would be exponential (O(2^n)). With `@lru_cache`, it becomes linear (O(n)).

### 🔹 Example 2: Caching with Custom Cache Size

Here's how to cache results with a limited cache size:

```python
from functools import lru_cache

@lru_cache(maxsize=3)  # Cache up to 3 results
def power_of_two(n):
    print(f"Calculating 2^{n}...")
    return 2 ** n

# Calculations and caching
print(power_of_two(2))  # Output: 4
print(power_of_two(3))  # Output: 8
print(power_of_two(4))  # Output: 16

# Cache is full; oldest cache (2) is removed
print(power_of_two(5))  # Output: 32

# Recomputes 2^2 since it was evicted from cache
print(power_of_two(2))  # Output: 4 (Recomputed)
```

### 🔹 Example 3: Manual Cache Implementation Using a Dictionary

You can also implement caching manually using a dictionary.

```python
# Manual caching using a dictionary
cache = {}

def factorial(n):
    if n in cache:
        return cache[n]
    if n == 0:
        result = 1
    else:
        result = n * factorial(n - 1)
    cache[n] = result
    return result

# Using the manual cache
print(factorial(5))  # Output: 120 (Calculates and caches result)
print(factorial(5))  # Output: 120 (Fetched from cache)
```

**Output:**

```
120
120
```

## 💡 Best Practices for Using Cache

1. **Limit Cache Size**: Avoid unlimited caches when dealing with large datasets to prevent memory overflow.
2. **Use Cache Wisely**: Only cache functions that are called frequently with the same parameters.
3. **Clear Cache When Needed**: If your data changes, make sure to clear the cache or implement a cache invalidation strategy.
4. **Test for Memory Usage**: Monitor memory usage when caching large amounts of data or when caching data with large memory footprints.

## 📝 Conclusion

Caching is a powerful tool that can significantly improve performance and reduce computational overhead. Python’s `@lru_cache` is a great way to get started with caching, offering both simplicity and flexibility. By following the best practices, you can leverage caching to make your applications more efficient and responsive. 🚀

---
