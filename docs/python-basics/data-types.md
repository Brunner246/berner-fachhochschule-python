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

!!! question "Quick Check: Variable assignment"
    What type does Python infer for each of these?

    ```python
    a = 30
    b = 30.0
    c = "30"
    d = True
    ```

    ??? success "Show answer"
        - `a` → `int`
        - `b` → `float`
        - `c` → `str`
        - `d` → `bool`

        Python infers the type from the value on the right-hand side — no declaration needed.

## Numbers

Python supports integers and floating-point numbers:

```python
count = 42          # int
height = 3.5        # float
result = count + 10 # arithmetic
```

!!! question "Quick Check: Integer vs. float division"
    What are the values of `a` and `b`?

    ```python
    a = 7 / 2
    b = 7 // 2
    ```

    ??? success "Show answer"
        - `a` → `3.5` (true division, always returns a `float`)
        - `b` → `3` (floor division, drops the remainder)

        Tip: `7 % 2` gives the remainder (`1`).

## Strings

Strings are sequences of characters:

```python
greeting = "Hello, World!"
print(greeting.upper())  # HELLO, WORLD!
print(len(greeting))     # 13
```

!!! question "Quick Check: String operators"
    What does this print?

    ```python
    prefix = "GL"
    grade = 24
    name = prefix + str(grade) + "h"
    print(name * 2)
    ```

    ??? success "Show answer"
        ```
        GL24hGL24h
        ```

        `+` concatenates strings, `*` repeats them. Note: `str(grade)` is required — Python will raise `TypeError` if you try to concatenate `str + int` directly.

## References and Mutability

![references](https://python-course.eu/images/python-tutorial/python_variable_2_500w.webp)

When you write `name = value`, Python does **not** copy the value into a box called `name`. Instead, `name` becomes a **label** that points to the value. Think of variable names like sticky notes attached to things on a shelf — several notes can stick to the same thing.

```python
beam_a = [120, 240, 5000]
beam_b = beam_a               # same list, two names
```

Both `beam_a` and `beam_b` now point to the **same** list. Modifying the list through one name is visible through the other:

```python
beam_b.append("GL24h")
print(beam_a)                 # [120, 240, 5000, 'GL24h']
```

This surprises everyone the first time they see it.

### Mutable vs. immutable types

| Type                     | Mutable? | Example                                |
|--------------------------|----------|----------------------------------------|
| `int`, `float`, `bool`   | No       | `x = x + 1` creates a **new** number   |
| `str`                    | No       | `s.upper()` returns a **new** string   |
| `tuple`                  | No       | Cannot change items after creation     |
| `list`                   | Yes      | `lst.append(...)`, `lst[0] = ...`      |
| `dict`                   | Yes      | `d["key"] = value`, `d.pop(...)`       |
| `set`                    | Yes      | `s.add(...)`, `s.remove(...)`          |

- **Mutable** types can be changed in place — every name pointing to them sees the change.
- **Immutable** types cannot be changed in place — operations always produce a **new** object.

### Immutable behaves intuitively

```python
length_a = 5000
length_b = length_a           # both labels point to 5000

length_b = length_b + 1000    # length_b now points to a NEW int (6000)
print(length_a)               # 5000 — unaffected
```

Because `int` is immutable, `length_b + 1000` builds a fresh integer and re-binds the label `length_b` to it. The original `5000` is untouched, so `length_a` still sees it.

### When you want a real copy

```python
beam_a = [120, 240, 5000]
beam_b = beam_a.copy()        # NEW list, independent

beam_b.append("GL24h")
print(beam_a)                 # [120, 240, 5000]            — untouched
print(beam_b)                 # [120, 240, 5000, 'GL24h']
```

Other ways to copy a list: `list(beam_a)`, or the slice `beam_a[:]`. For dicts use `d.copy()` or `dict(d)`. For sets use `s.copy()` or `set(s)`.

!!! question "Quick Check: Predict the output"
    What does the second `print` show?

    ```python
    a = [1, 2, 3]
    b = a
    b[0] = 99
    print(a)
    ```

    ??? success "Show answer"
        `[99, 2, 3]`

        `b = a` does not copy the list — both names point to the **same** list. Changing element `0` through `b` is visible through `a`. To get an independent list, write `b = a.copy()`.

!!! question "Quick Check: Why is this different?"
    What does this print?

    ```python
    a = 10
    b = a
    b = 99
    print(a)
    ```

    ??? success "Show answer"
        `10`

        `int` is immutable. `b = 99` does not modify the original number — it just rebinds the label `b` to a different integer. `a` still labels the original `10`.

!!! tip "Rule of thumb"
    When you pass a `list`, `dict`, or `set` into a function, **the function can change it**. When you pass an `int`, `float`, `str`, or `tuple`, it cannot. If you want to be sure a function does not change your data, pass a copy.
