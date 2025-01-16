---
title: typing in Python
sub_title: Getting started with typing!
author: Lawrence (lawrence@cinnamon.is)
---
Why typing?
===

---
<!-- column_layout: [1, 1, 1] -->
<!-- column: 0 -->
```py
def find_center(coords):
    return (
        (coords[0] + coords[2]) / 2,
        (coords[1] + coords[3]) / 2,
    )
```
<!-- column: 1 -->
```py
def find_center(
    coords: tuple[float, float, float, float],
) -> tuple[float, float]:
    return (
        (coords[0] + coords[2]) / 2,
        (coords[1] + coords[3]) / 2,
    )
```
<!-- column: 2 -->
```py
type Point = tuple[float, float]
type Rectangle = tuple[float, float, float, float]

def find_center(coords: Rectangle]) -> Point:
    return (
        (coords[0] + coords[2]) / 2,
        (coords[1] + coords[3]) / 2,
    )
```
<!-- reset_layout -->
---
<!-- end_slide -->
PERIOD
===

---
---
<!-- end_slide -->

typing basic
===

---
# Primitives
# Type alias
# TypedDict
# Stub
---
<!-- end_slide -->

Changes since 3.10, 3.11, 3.12
===

---

# 3.10
- Union syntax (|)
- TypeGuard (later)
- ParamSpec (later)
- TypeVarTuple (later)

# 3.11
- Self
- Required / NotRequired
- LiteralString

# 3.12
- New generic syntax
- New type alias syntax
-

---
<!-- end_slide -->

Functional programming support
===

---

# ParamSpec

---
<!-- end_slide -->

OOP support
===

---

---
<!-- end_slide -->

Polymorphism
===

---

---
<!-- end_slide -->

Liskov
===

---

---
<!-- end_slide -->

Generic
===

---

---
<!-- end_slide -->

TypeVar, TypeVarTuple, ParamSpec
===

---

---
<!-- end_slide -->

collections.abc types
===

---
# Sequence
# Mapping
# [Async]Generator

---
<!-- end_slide -->

Covariant, Contravariant, Invariant
===

---

---
<!-- end_slide -->

Getting started with typing
===

---
# Tools for the job
- python>=3.11
- mypy
- ruff
- pyright

---
<!-- end_slide -->

Mindset of using typing
===

---
# py.typed

---
<!-- end_slide -->

mypy terminology
===

---
# TypeGuard

---
<!-- end_slide -->

HKT
===

---

---
<!-- end_slide -->

<!-- jump_to_middle -->
Thank you!
---
