# Warm-Up

Before diving into the exercises, this page gets you back into the rhythm of Python and the cadwork 3d API. **Part A** is pen-and-paper: predict what each snippet prints, then reveal the answer. **Part B** is hands-on: short scripts you run against the model, each deliberately smaller than the first real exercise.

!!! info "Setup"
    For Part B, [download the timber framed slab model](model-download.md) and open it in cadwork 3d. The warm-up tasks use the `element_controller`, `attribute_controller`, and `cadwork` modules — the same ones you will use throughout the exercises.

---

## Part A — Python recall (no cadwork needed)

Read each snippet and predict the output **before** revealing the answer.

!!! question "Quick Check: a loop over a range"
    What does this print?

    ```python
    total = 0
    for i in range(1, 5):
        total += i
    print(total)
    ```

    ??? success "Show answer"
        `10` — it sums `1 + 2 + 3 + 4`. `range(1, 5)` stops **before** `5`.

!!! question "Quick Check: a list comprehension with a filter"
    What is the value of `result`?

    ```python
    numbers = [120, 240, 60, 300, 90]
    result = [n for n in numbers if n >= 100]
    print(result)
    ```

    ??? success "Show answer"
        `[120, 240, 300]` — the comprehension keeps only the values where `n >= 100`.

!!! question "Quick Check: counting with a dictionary"
    What does this print?

    ```python
    names = ["Joist", "Joist", "Rim Joist", "Joist"]
    counts = {}
    for name in names:
        counts[name] = counts.get(name, 0) + 1
    print(counts)
    ```

    ??? success "Show answer"
        `{'Joist': 3, 'Rim Joist': 1}` — `counts.get(name, 0)` returns the current count or `0` if the name is new, so each occurrence adds one.

---

## Part B — Back into cadwork

Now run a few small scripts against the open model. Each one reactivates an API call you already saw on the [Elements](elements.md) and [Geometry](geometry.md) pages.

## Warm-up 1: Count the elements

Import the controllers and print how many elements the model contains.

??? example "Hint"
    Use `ec.get_all_identifiable_element_ids()` to get a list of IDs, then `len()`.

??? success "Solution"
    ```python
    import element_controller as ec

    all_ids = ec.get_all_identifiable_element_ids()
    print(f"Number of elements: {len(all_ids)}")
    ```

---

## Warm-up 2: Print every element name

Loop over the element IDs and print each element's name.

??? example "Hint"
    `ac.get_name(eid)` returns the name of a single element. Call it inside a `for` loop.

??? success "Solution"
    ```python
    import element_controller as ec
    import attribute_controller as ac

    all_ids = ec.get_all_identifiable_element_ids()

    for eid in all_ids:
        print(f"{eid}: {ac.get_name(eid)}")
    ```

---

## Warm-up 3: A point, a direction, a distance

Build two points, then compute the direction and distance between them. This is the exact input shape that element-creation calls expect.

??? example "Hint"
    `point − point` gives a vector. Use `.normalized()` for a pure direction and `.distance()` for the length. See [Geometry](geometry.md) if you need a refresher.

??? success "Solution"
    ```python
    import cadwork as cw

    p1 = cw.point_3d(0, 0, 0)
    p2 = cw.point_3d(5000, 0, 0)

    direction = (p2 - p1).normalized()   # unit vector from p1 to p2
    length = p2.distance(p1)             # 5000.0

    print(f"direction: ({direction.x}, {direction.y}, {direction.z})")
    print(f"length: {length}")
    ```

---

Keep these open if you get stuck mid-warm-up:

- [Geometry](geometry.md) and [Elements](elements.md) — the topic pages this warm-up draws from.
- [Cheat Sheet](cheatsheet.md) — quick lookup for module names and common calls.

