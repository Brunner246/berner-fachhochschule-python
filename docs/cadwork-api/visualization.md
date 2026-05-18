# Visualization

The `visualization_controller` lets your script give **visible feedback** in the cadwork 3d viewport. Color elements, hide them, change the camera, or programmatically select a subset — perfect for confirming that a filter or generator did what you expected.

## Coloring elements

```python
import element_controller as ec
import attribute_controller as ac
import visualization_controller as vc

all_ids = ec.get_all_identifiable_element_ids()
joists = [eid for eid in all_ids if ac.get_name(eid) == "Joist"]

vc.set_color(joists, 3)   # color id 3 = red in the default palette
```

Color IDs are integers referring to the cadwork color palette.

!!! tip
Open the cadwork color dialog to see the full palette and the ID assigned to each swatch.

## Programmatic selection

```python
import visualization_controller as vc

vc.set_active(joists)   # selects the elements in the viewport
```

The user sees the joists highlighted as if they had clicked them. Useful when your script identifies a subset and you want the user to inspect them next.

## Visibility

```python
import visualization_controller as vc

vc.set_invisible(insulation_ids)   # remove from view
vc.set_visible(insulation_ids)   # bring back
vc.show_all_elements()             # reset
```

Hide insulation while you work on the structural frame, then bring it back at the end.

## Camera views

```python
import visualization_controller as vc

vc.show_view_positive_x()   # look along +X
vc.show_view_positive_y()   # look along +Y
vc.show_view_positive_z()   # top view
vc.show_view_standard_axo() # standard isometric
```

Set the view at the start of a script so screenshots come out consistent.

## Worked example — color by material

```python
import element_controller as ec
import attribute_controller as ac
import visualization_controller as vc

COLOR_BY_MATERIAL = {
    "GL24h": 3,   
    "GL28h": 4,   
    "C24":   5,   
    "OSB":   6,   
}

all_ids = ec.get_all_identifiable_element_ids()

for eid in all_ids:
    material = ac.get_element_material_name(eid)
    color = COLOR_BY_MATERIAL.get(material)
    if color is not None:
        vc.set_color([eid], color)
```

Every element with a known material gets a distinct color. Elements with unknown materials keep their original color (because `dict.get` returns `None` and we skip them).

!!! question "Quick Check: Color by length"
Write the loop body to color **long** beams (length > 5000 mm) red (`3`) and **short** beams green (`4`). Assume `lengths[eid]` is already populated.

    ??? success "Show answer"
        ```python
        for eid, length in lengths.items():
            if length > 5000:
                vc.set_color([eid], 3)
            else:
                vc.set_color([eid], 4)
        ```

        Or in one pass without a dict:

        ```python
        for eid in all_ids:
            length = gc.get_length(eid)
            vc.set_color([eid], 3 if length > 5000 else 4)
        ```

!!! question "Quick Check: Selection vs. color"
What is the difference between `vc.set_color(ids, 3)` and `vc.set_active(ids)`?

    ??? success "Show answer"
        - `set_color` **changes** an element's color attribute — the change persists until you set it again (or undo).
        - `set_active` only **selects** the elements in the viewport — like clicking on them. No attribute is modified.

        Use color when you want a lasting visual mark. Use selection when you want the user to "be in" that subset (e.g., to apply a manual operation next).

