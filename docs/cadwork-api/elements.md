# Elements

Elements are the core building blocks in cadwork 3d. The Python API lets you create, query, and manipulate them.

## Querying Elements

```python
import element_controller as ec

# Get all element IDs
all_ids = ec.get_all_identifiable_element_ids()

# Get active (selected) elements
active_ids = ec.get_active_identifiable_element_ids()

print(f"Total: {len(all_ids)}, Selected: {len(active_ids)}")
```

## Creating Elements

```python
import cadwork as cw
import element_controller as ec

# Define a beam by start point, end point, and cross-section
p1 = cw.point_3d(0, 0, 0)
p2 = cw.point_3d(5000, 0, 0)

local_x_dir = (p2 - p1).normalized()  # Direction from p1 to p2
local_z_dir = cw.point_3d(0, 0, 1)          # Up direction

width = 120.0
height = 240.0

element_id = ec.create_rectangular_beam_vectors(width, height, p1, local_x_dir, local_z_dir)
```

## Deleting Elements

```python
import element_controller as ec

# Delete specific elements
element_ids = [1001, 1002, 1003]
ec.delete_elements(element_ids)
```

## Filtering Elements

```python
import element_controller as ec
import attribute_controller as ac

all_ids = ec.get_all_identifiable_element_ids()

# Filter beams by name
beams = [
    eid for eid in all_ids
    if ac.get_name(eid) == "Beam"
]
```

!!! note
    Element IDs are integers that uniquely identify each element in the current cadwork session. They may change between sessions.
