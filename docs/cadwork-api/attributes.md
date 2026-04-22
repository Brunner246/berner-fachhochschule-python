# Attributes

Attributes store metadata about elements such as name, group, material, and user-defined properties.

## Reading Attributes

```python
import attribute_controller as ac

element_id = 1001

name = ac.get_name(element_id)
group = ac.get_group(element_id)
subgroup = ac.get_subgroup(element_id)
material = ac.get_material(element_id)

print(f"{name} | {group} | {material}")
```

## Writing Attributes

```python
import attribute_controller as ac

element_ids = [1001, 1002]

for eid in element_ids:
    ac.set_name(eid, "Rafter")
    ac.set_group(eid, "Roof")
    ac.set_subgroup(eid, "Primary Structure")
```

## User Attributes

cadwork supports user-defined attributes (user attributes 1–99):

```python
import attribute_controller as ac

element_id = 1001

# Set a user attribute
ac.set_user_attribute(element_id, 1, "Custom Value")

# Read it back
value = ac.get_user_attribute(element_id, 1)
print(value)  # Custom Value
```

## Listing All Attributes

```python
import element_controller as ec
import attribute_controller as ac

for eid in ec.get_active_identifiable_element_ids():
    print(
        f"ID: {eid}, "
        f"Name: {ac.get_name(eid)}, "
        f"Material: {ac.get_material(eid)}"
    )
```

!!! warning
    Setting attributes on a large number of elements in a loop can be slow. Consider batching operations where possible.
