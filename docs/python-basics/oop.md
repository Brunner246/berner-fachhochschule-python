# Object-Oriented Programming

Object-oriented programming (OOP) organizes code around objects that combine data and behavior.

![OOP](https://realpython.com/cdn-cgi/image/width=960,format=auto/https://files.realpython.com/media/Object-Oriented-Programming-OOP-in-Python-3_Watermarked.0d29780806d5.jpg)

## Core Concepts

The following diagram shows the four pillars of OOP:

```mermaid
mindmap
  root((OOP))
    Encapsulation
      Bundling data and methods
      Access control via properties
    Inheritance
      Reuse code from parent classes
      Extend with new behavior
    Polymorphism
      Same interface, different behavior
      Method overriding
    Abstraction
      Hide implementation details
      Expose only what is needed
```

## Classes and Objects

A **class** is a blueprint. An **object** is an instance of that class.

```mermaid
classDiagram
    class TimberElement {
        +str name
        +float width
        +float height
        +float length
        +volume() float
    }
```

```python
class TimberElement:
    def __init__(self, name, width, height, length):
        self.name = name
        self.width = width
        self.height = height
        self.length = length

    def volume(self):
        return self.width * self.height * self.length

beam = TimberElement("Beam_01", 120, 240, 5000)
print(f"{beam.name}: {beam.volume()} mm³")
```

## Inheritance

A child class inherits attributes and methods from its parent and can add or override them.

```mermaid
classDiagram
    class TimberElement {
        +str name
        +float width
        +float height
        +float length
        +volume() float
    }

    class Beam {
        +str profile
    }

    class Wall {
        +int layers
        +describe() str
    }

    class Panel {
        +float thickness
        +area() float
    }

    TimberElement <|-- Beam
    TimberElement <|-- Wall
    TimberElement <|-- Panel
```

```python
class Wall(TimberElement):
    def __init__(self, name, width, height, length, layers):
        super().__init__(name, width, height, length)
        self.layers = layers

    def describe(self):
        return f"{self.name} with {self.layers} layers"

wall = Wall("Wall_01", 160, 2800, 6000, 3)
print(wall.describe())
print(wall.volume())
```

## Properties

Properties provide controlled access to attributes — this is **encapsulation** in practice.

```mermaid
flowchart LR
    A["element.area"] -->|"calls @property"| B["_width * _height"]
    B --> C["returns 28800"]
```

```python
class Element:
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def area(self):
        return self._width * self._height

element = Element(120, 240)
print(element.area)  # 28800
```

## Object Lifecycle

```mermaid
flowchart TD
    A["Class Definition"] --> B["__init__ called"]
    B --> C["Object in Memory"]
    C --> D["Use object: call methods, read attributes"]
    D --> E{"Still referenced?"}
    E -->|Yes| D
    E -->|No| F["Garbage Collected"]
```

