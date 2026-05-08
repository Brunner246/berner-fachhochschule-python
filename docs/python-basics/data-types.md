# Data Types

Python provides several built-in data types. Understanding them is the foundation for writing any program.

![data types](https://realpython.com/cdn-cgi/image/width=960,format=auto/https://files.realpython.com/media/Basic-Data-Types-in-Python_Watermarked.e3dd34457952.jpg)

## Variables

In Python, variables are created by assigning a value:

```python
name = "cadwork"
version = 30
pi = 3.14159
```

## Numbers

Python supports integers and floating-point numbers:

```python
count = 42          # int
height = 3.5        # float
result = count + 10 # arithmetic
```

## Strings

Strings are sequences of characters:

```python
greeting = "Hello, World!"
print(greeting.upper())  # HELLO, WORLD!
print(len(greeting))     # 13
```

## Lists

Lists are ordered, mutable collections:

```python
elements = ["beam", "panel", "column"]
elements.append("plate")
print(elements[0])  # beam
```

## Dictionaries

Dictionaries store key-value pairs:

```python
beam = {
    "width": 120,
    "height": 240,
    "length": 5000,
    "material": "GL24h"
}
print(beam["material"])  # GL24h
```

!!! note
    Python is dynamically typed — you do not need to declare the type of a variable explicitly.
