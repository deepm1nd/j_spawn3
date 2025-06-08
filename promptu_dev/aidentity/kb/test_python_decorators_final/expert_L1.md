# Understanding Python Decorators - L1 Research Findings

Date: 2024-05-24

---
## Research Findings for: Understanding Python Decorators (Level 1 - Aggregated Subtopics)

Date: 2024-05-24

## Content for: Fundamentals of Python Functions for Decorators

Decorators in Python are built upon the understanding that functions are first-class objects. This means functions can be:
1.  **Assigned to variables:** `my_func = some_function`
2.  **Passed as arguments to other functions:** `higher_order_func(my_func)`
3.  **Returned from other functions:** `return my_func`

**Inner Functions (Closures):**
A crucial concept is the ability to define functions inside other functions, known as inner functions. These inner functions have access to variables in the enclosing (outer) function's scope, even after the outer function has completed execution. This is called a closure.
```python
def outer_function(msg):
    message = msg
    def inner_function():
        print(message) # Accesses 'message' from outer_function's scope
    return inner_function

my_printer = outer_function("Hello from closure!")
my_printer() # Output: Hello from closure!
```
Decorators heavily rely on closures: the wrapper function defined inside the decorator "closes over" the original function passed to the decorator and any arguments the decorator itself might have received.

## Content for: Basic Decorator Mechanics

The primary purpose of a decorator is to wrap additional functionality around an existing function or method. The `@decorator_name` syntax is a convenient shorthand.

**Syntax Equivalence:**
```python
@my_decorator
def decorated_function():
    pass

# Is equivalent to:
def decorated_function():
    pass
decorated_function = my_decorator(decorated_function)
```

**Wrapper Function:**
The `my_decorator` function takes `decorated_function` as an argument (`func`) and typically defines an inner "wrapper" function. This wrapper function adds the extra behavior and calls the original function. The decorator then returns this wrapper function.
```python
def my_decorator(func):
    def wrapper(*args, **kwargs): # Handles arbitrary arguments
        print("Action before calling the original function.")
        result = func(*args, **kwargs) # Call original
        print("Action after calling the original function.")
        return result # Return original's result
    return wrapper
```
The use of `*args` (for positional arguments) and `**kwargs` (for keyword arguments) in the wrapper function is essential to ensure that the decorator can be applied to any function, regardless of its parameters. The wrapper then passes these collected arguments when calling the original function.

## Content for: Advanced Decorator Patterns

### Decorators with Arguments
Sometimes, it's useful for a decorator itself to accept arguments. This requires an extra level of nesting. The decorator function that accepts arguments must return another function, which is the actual decorator that takes the function-to-be-decorated as an argument.

**Structure:**
```python
def decorator_factory(arg1, arg2): # Takes decorator arguments
    def actual_decorator(func):     # Takes the function to decorate
        @functools.wraps(func)      # Preserve metadata
        def wrapper(*args, **kwargs): # The actual wrapper
            print(f"Decorator args: {arg1}, {arg2}")
            # Logic using arg1, arg2 before func call
            result = func(*args, **kwargs)
            # Logic using arg1, arg2 after func call
            return result
        return wrapper
    return actual_decorator

@decorator_factory("param1_value", "param2_value")
def my_function(x, y):
    print(f"Executing my_function({x}, {y})")
    return x + y

my_function(10, 20)
```
Here, `decorator_factory("param1_value", "param2_value")` is called first. It returns `actual_decorator` (with `arg1` and `arg2` captured in its closure). Then, `actual_decorator` is applied to `my_function`.

### Class-based Decorators
Classes can also be used to create decorators. This is particularly useful when the decorator needs to maintain state or has more complex logic involving multiple methods. A class decorator must implement the `__init__` method (which receives the function to be decorated) and the `__call__` method (which makes instances of the class callable and acts as the wrapper).

**Structure:**
```python
import functools

class StatefulDecorator:
    def __init__(self, func):
        functools.update_wrapper(self, func) # For classes, use update_wrapper
        self.func = func
        self.num_calls = 0 # Example state

    def __call__(self, *args, **kwargs):
        self.num_calls += 1
        print(f"Call {self.num_calls} of {self.func.__name__}")
        return self.func(*args, **kwargs)

@StatefulDecorator
def greet(name):
    print(f"Hello, {name}")

greet("World") # Call 1 of greet
greet("Again") # Call 2 of greet
```
The `__init__` method stores the decorated function and initializes any state. The `__call__` method is executed when the decorated function (which is now an instance of `StatefulDecorator`) is called. `functools.update_wrapper` is used instead of `@functools.wraps` for class decorators to copy metadata.

## Content for: Common Use Cases & Examples

### Caching / Memoization with `@functools.lru_cache` and `@functools.cache`
Python's `functools` module provides powerful built-in decorators for caching the results of function calls. This is known as memoization and is very effective for optimizing expensive function calls that are frequently made with the same arguments.

*   `@functools.lru_cache(maxsize=N, typed=False)`: Creates a Least Recently Used (LRU) cache. It stores up to `maxsize` results. If `maxsize` is `None`, the LRU feature is disabled, and the cache can grow indefinitely. If `typed` is `True`, arguments of different types will be cached separately (e.g., `f(3)` and `f(3.0)` would be distinct).
*   `@functools.cache`: A simpler, unbounded cache (equivalent to `lru_cache(maxsize=None)`). Added in Python 3.9.

**Example (Fibonacci with caching):**
```python
import functools

@functools.cache # or @functools.lru_cache(maxsize=None)
def fibonacci(n):
    if n < 2:
        return n
    print(f"Calculating fibonacci({n})") # To see when it's not cached
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(10))
print(fibonacci(7)) # Will largely use cached values from fibonacci(10) call
print(fibonacci.cache_info())
```
This significantly improves performance for recursive functions like Fibonacci by avoiding re-computation of already known values. The `cache_info()` method provides statistics on cache hits and misses.
---
*(Additional L1 content for other subtopics like "Nesting Decorators", "Decorating Class Methods", "Specific Use Cases like Timing/Debugging in more detail" would follow here, each aiming for similar depth and drawing from multiple sources if necessary.)*
