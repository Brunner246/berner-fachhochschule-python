# Cheat Sheet

One-page reference of the most-used cadwork 3d Python API calls. Print it, keep it next to your keyboard.

## Imports

```python
import cadwork as cw
import element_controller as ec
import attribute_controller as ac
import geometry_controller as gc
import visualization_controller as vc
import utility_controller as uc
```

## `element_controller` — query, create, delete

| Call | Purpose |
|------|---------|
| `ec.get_all_identifiable_element_ids()` | All element IDs in the model |
| `ec.get_active_identifiable_element_ids()` | Currently selected IDs |
| `ec.get_visible_identifiable_element_ids()` | Currently visible IDs |
| `ec.create_rectangular_beam_vectors(w, h, l, p1, xl_dir)` | New beam from origin + direction |
| `ec.copy_elements(ids)` | Duplicate, returns new IDs |
| `ec.move_element(ids, vector)` | Translate by a `point_3d` offset |
| `ec.delete_elements(ids)` | Remove from model |

## `attribute_controller` — read & write metadata

| Call | Purpose |
|------|---------|
| `ac.get_name(eid)` / `ac.set_name([eid], "…")` | Element name |
| `ac.get_group(eid)` / `ac.set_group([eid], "…")` | Group |
| `ac.get_subgroup(eid)` / `ac.set_subgroup([eid], "…")` | Subgroup |
| `ac.get_element_material_name(eid)` | Material as a string |
| `ac.set_material([eid], "GL24h")` | Assign material by name |
| `ac.get_user_attribute(eid, n)` / `ac.set_user_attribute([eid], n, "…")` | User attribute slot 1–99 |
| `ac.get_element_type(eid)` | Returns a type object with `.is_rectangular_beam()` etc. |
| `ac.set_assembly_number([eid], "…")` | Assembly number |

Setters take a **list** of IDs (even if only one) and a single value.

## `geometry_controller` — sizes and positions

| Call | Purpose |
|------|---------|
| `gc.get_length(eid)` | mm along the element's local X (xl) |
| `gc.get_width(eid)` | mm along yl |
| `gc.get_height(eid)` | mm along zl |
| `gc.get_p1(eid)` / `gc.get_p2(eid)` | Start / end as `point_3d` |
| `gc.get_xl(eid)` / `gc.get_yl(eid)` / `gc.get_zl(eid)` | Local basis vectors |
| `gc.set_width_real([eid], mm)` | Change width (cross-section) |
| `gc.set_height_real([eid], mm)` | Change height |

## `visualization_controller` — see your results

| Call | Purpose |
|------|---------|
| `vc.set_color(ids, color_id)` | Paint elements (color id, integer) |
| `vc.set_active_element_ids(ids)` | Select in viewport |
| `vc.hide_elements(ids)` / `vc.show_elements(ids)` | Visibility |
| `vc.show_all_elements()` | Reset hidden state |
| `vc.show_view_standard_axo()` | Reset to isometric |
| `vc.show_view_positive_z()` | Top view |

Common color IDs: `1` black · `3` red · `4` green · `5` blue · `6` yellow · `9` grey.

## `utility_controller` — files & user I/O

| Call | Purpose |
|------|---------|
| `uc.get_3d_file_path()` | Folder of the current `.3d` file — useful for exports |
| `uc.get_3d_file_name()` | Current model filename |
| `uc.get_user_string(prompt)` | Ask the user for a string |
| `uc.get_user_double(prompt)` | Ask for a number |
| `uc.get_user_int(prompt)` | Ask for an integer |
| `uc.print_to_console(msg)` | Print to cadwork's script console |

## `cadwork` core — types & math

| Call | Purpose |
|------|---------|
| `cw.point_3d(x, y, z)` | Create a 3D point/vector |
| `p1 + p2`, `p1 - p2` | Vector arithmetic |
| `p * 2.5` | Scale |
| `p1.distance(p2)` | Length between |
| `v.normalized()` | Unit vector |
| `v.cross(other)` | Cross product (perpendicular) |
| `v.dot(other)` | Dot product (angle / projection) |
| `p.x`, `p.y`, `p.z` | Component access |

## Common Python helpers

```python
# Filter elements by name
joists = [eid for eid in all_ids if ac.get_name(eid) == "Joist"]

# Group by name and count
from collections import Counter
counts = Counter(ac.get_name(eid) for eid in all_ids)

# Unique values
materials = {ac.get_element_material_name(eid) for eid in all_ids}

# CSV export
import csv, os
path = os.path.join(uc.get_3d_file_path(), "report.csv")
with open(path, "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["ID", "Name"])
    for eid in all_ids:
        writer.writerow([eid, ac.get_name(eid)])

# Math
import math
math.floor(rim_length / spacing) + 1
```

## Workflow reminders

- Element IDs change between sessions — never hard-code them.
- All lengths are in **mm**.
- Setters take **lists** of IDs: `ac.set_name([eid], "Joist")`.
- `vc.set_color(ids, 3)` is your visual unit-test.
- Read tracebacks from the **bottom up** — last line tells you what broke.
- Save the model **before** running anything that creates or deletes elements.
