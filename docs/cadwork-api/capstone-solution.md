---
password: cwapi3dPythonBFH
---

# Capstone — Full Reference Solution

This page contains the **full assembled solution** for the [Capstone exercise](capstone.md). Try the staged hints there first.

```python
import csv
import os
from pathlib import Path

import cadwork as cw
import element_controller as ec
import attribute_controller as ac
import geometry_controller as gc
import visualization_controller as vc
import utility_controller as uc

# --- Parameters ---
JOIST_LENGTH   = 5000.0
JOIST_COUNT    = 10
JOIST_SPACING  = 625.0
JOIST_WIDTH    = 60.0
JOIST_HEIGHT   = 240.0

COLOR_JOIST    = 3
COLOR_RIM      = 4
COLOR_BLOCKING = 5

x_direction = cw.point_3d(1, 0, 0)
y_direction = cw.point_3d(0, 1, 0)

# 1. Joists along X
joist_ids = []
for i in range(JOIST_COUNT):
    p1 = cw.point_3d(0, i * JOIST_SPACING, 0)
    new_id = ec.create_rectangular_beam_vectors(
        JOIST_WIDTH, JOIST_HEIGHT, JOIST_LENGTH, p1, x_direction, x_direction.cross(y_direction)
    )
    ac.set_name([new_id], "Joist")
    joist_ids.append(new_id)

# 2. Rim joists along Y at both ends
rim_start_y = -JOIST_WIDTH / 2
rim_length  = (JOIST_COUNT - 1) * JOIST_SPACING + JOIST_WIDTH

rim_ids = []
for x in (0.0, JOIST_LENGTH):
    p1 = cw.point_3d(x, rim_start_y, 0)
    new_id = ec.create_rectangular_beam_vectors(
        JOIST_WIDTH, JOIST_HEIGHT, rim_length, p1, y_direction, y_direction.cross(x_direction)
    )
    ac.set_name([new_id], "Rim Joist")
    rim_ids.append(new_id)

# 3. Blocking at mid-span between adjacent joists
mid_x = JOIST_LENGTH / 2
block_length = JOIST_SPACING - JOIST_WIDTH

blocking_ids = []
for i in range(JOIST_COUNT - 1):
    y_start = i * JOIST_SPACING + JOIST_WIDTH / 2
    p1 = cw.point_3d(mid_x, y_start, 0)
    new_id = ec.create_rectangular_beam_vectors(
        JOIST_WIDTH, JOIST_HEIGHT, block_length, p1, y_direction, y_direction.cross(x_direction)
    )
    ac.set_name([new_id], "Blocking")
    blocking_ids.append(new_id)

# 4. Group, subgroup, sequential numbering
all_new = joist_ids + rim_ids + blocking_ids
for eid in all_new:
    ac.set_group([eid], "Slab")
    ac.set_subgroup([eid], "Structure")

for i, eid in enumerate(joist_ids, start=1):
    ac.set_user_attribute([eid], 1, f"J-{i:03d}")

# 5. Color
vc.set_color(joist_ids,    COLOR_JOIST)
vc.set_color(rim_ids,      COLOR_RIM)
vc.set_color(blocking_ids, COLOR_BLOCKING)

# 6. BoM CSV
groups = {
    "Joist":     joist_ids,
    "Rim Joist": rim_ids,
    "Blocking":  blocking_ids,
}
rows = []
for name, ids in groups.items():
    total_length_mm = sum(gc.get_length(e) for e in ids)
    total_volume_mm3 = sum(
        gc.get_length(e) * gc.get_width(e) * gc.get_height(e) for e in ids
    )
    rows.append({
        "Name": name,
        "Count": len(ids),
        "Total Length (m)": round(total_length_mm / 1000, 2),
        "Total Volume (m3)": round(total_volume_mm3 / 1_000_000_000, 4),
    })

file_path = Path(uc.get_3d_file_path()).parent / "bom.csv"
with open(file_path, "w", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=rows[0].keys())
    writer.writeheader()
    writer.writerows(rows)

print(f"Generated {len(joist_ids)} joists, {len(rim_ids)} rim joists, {len(blocking_ids)} blocking elements")
print(f"BoM exported to {file_path}")
```
