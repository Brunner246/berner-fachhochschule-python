# Exercises — Reading Model Data

In these exercises you will work with a timber framed slab model provided as a cadwork 3d file. The goal is to practice querying elements and their attributes without modifying anything.

!!! info "Setup"
    [Download the timber framed slab model](model-download.md) and open it in cadwork 3d before starting. All exercises use the `element_controller` and `attribute_controller` modules.

---

## Exercise 1: Count All Elements

Write a script that prints the total number of elements in the model.

??? example "Hint"
    Use `ec.get_all_identifiable_element_ids()` and `len()`.

??? success "Solution"
    ```python
    import element_controller as ec

    all_ids = ec.get_all_identifiable_element_ids()
    print(f"Total elements: {len(all_ids)}")
    ```

---

## Exercise 2: List Unique Names

Write a script that collects all element names and prints a sorted list of unique names found in the model.

??? example "Hint"
    Use a `set` to collect unique values from `ac.get_name()`.

??? success "Solution"
    ```python
    import element_controller as ec
    import attribute_controller as ac

    all_ids = ec.get_all_identifiable_element_ids()
    names = {ac.get_name(eid) for eid in all_ids}

    for name in sorted(names):
        print(name)
    ```

---

## Exercise 3: Filter by Name

Write a script that finds all elements with the name `"Joist"` and prints how many were found.

??? example "Hint"
    Use a list comprehension with a condition on `ac.get_name()`.

??? success "Solution"
    ```python
    import element_controller as ec
    import attribute_controller as ac

    all_ids = ec.get_all_identifiable_element_ids()
    joists = [eid for eid in all_ids if ac.get_name(eid) == "Joist"]
    print(f"Joists found: {len(joists)}")
    ```

---

## Exercise 3.1: Filter by Element Type
Write a script that finds all elements of type `"Rectangular Beam"` and prints how many were found.

??? example "Hint"
    Use `ac.get_element_type()` to check the type of each element.

??? success "Solution"
    ```python
    import cadwork
    import element_controller as ec
    import attribute_controller as ac

    all_ids = ec.get_all_identifiable_element_ids()
    beams = [eid for eid in all_ids if ac.get_element_type(eid).is_rectangular_beam()]
    print(f"Rectangular Beams found: {len(beams)}")
    ```

## Exercise 4: Group Elements by Name

Write a script that groups all elements by their name and prints a summary table showing each name and the count of elements.

Expected output format:

```
Joist         : 12
Rim Joist     :  4
Sheathing     :  2
Blocking      :  6
```

??? example "Hint"
    Use a `dict` to accumulate counts, or use `collections.Counter`.

??? success "Solution"
    ```python
    import element_controller as ec
    import attribute_controller as ac
    from collections import Counter

    all_ids = ec.get_all_identifiable_element_ids()
    name_counts = Counter(ac.get_name(eid) for eid in all_ids)

    for name, count in sorted(name_counts.items()):
        print(f"{name:20s}: {count:3d}")

    # or with a dict:
    name_counts = {}
    for eid in all_ids:
        name = ac.get_name(eid)
        name_counts[name] = name_counts.get(name, 0) + 1
    for name, count in sorted(name_counts.items()):
        print(f"{name:20s}: {count:3d}")
    ```

---

## Exercise 5: Group by Name and Material

Extend the previous exercise: group elements by both name **and** material, then print a summary.

Expected output format:

```
Joist        | GL24h       : 12
Rim Joist    | C24         :  4
Sheathing    | OSB         :  2
```

??? example "Hint"
    Use a tuple `(name, material)` as the dictionary key.

??? success "Solution"
    ```python
    import element_controller as ec
    import attribute_controller as ac
    from collections import Counter

    all_ids = ec.get_all_identifiable_element_ids()
    groups = Counter(
        (ac.get_name(eid), ac.get_element_material_name(eid))
        for eid in all_ids
    )

    for (name, material), count in sorted(groups.items()):
        print(f"{name:15s}| {material:12s}: {count:3d}")
    ```

---

## Exercise 6: Filter by Group and Subgroup

Write a script that finds all elements where the group is `"Slab"` and the subgroup is `"Structure"`, then prints their names and IDs.

??? success "Solution"
    ```python
    import element_controller as ec
    import attribute_controller as ac

    all_ids = ec.get_all_identifiable_element_ids()

    for eid in all_ids:
        group = ac.get_group(eid)
        subgroup = ac.get_subgroup(eid)
        if group == "Slab" and subgroup == "Structure":
            print(f"ID: {eid}, Name: {ac.get_name(eid)}")
    ```

---

## Exercise 7: Export to CSV

Write a script that exports a CSV file containing the columns: `ID`, `Name`, `Group`, `Subgroup`, `Material` for every element in the model.

??? example "Hint"
    Use Python's built-in `csv` module and `utility_controller.get_3d_file_path()` for the output location.

??? success "Solution"
    ```python
    import csv
    import os
    import element_controller as ec
    import attribute_controller as ac
    import utility_controller as uc

    all_ids = ec.get_all_identifiable_element_ids()
    output_path = os.path.join(uc.get_3d_file_path(), "element_report.csv")

    with open(output_path, "w", newline="") as f:
        writer = csv.writer(f)
        writer.writerow(["ID", "Name", "Group", "Subgroup", "Material"])
        for eid in all_ids:
            writer.writerow([
                eid,
                ac.get_name(eid),
                ac.get_group(eid),
                ac.get_subgroup(eid),
                ac.get_element_material_name(eid),
            ])

    print(f"Exported {len(all_ids)} elements to {output_path}")
    ```
