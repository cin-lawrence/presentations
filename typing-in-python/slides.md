---
title: typing in Python
sub_title: Getting started with typing!
author: Lawrence (lawrence@cinnamon.is)
---
#  why typing? (*motivation*)
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
#  PERIOD
---
<!-- end_slide -->

#  typing basic
---
<!-- end_slide -->

#  changes since 3.10, 3.11, 3.12
---
<!-- end_slide -->

#  functional support
---
<!-- end_slide -->

#  OOP support
---
<!-- end_slide -->

#  polymorphism
---
<!-- end_slide -->

#  liskov
---
<!-- end_slide -->

#  generic
---
<!-- end_slide -->

#  TypeVar, TypeVarTuple, ParamSpec
---
<!-- end_slide -->

#  collections.abc types
---
<!-- end_slide -->

#  covariant, contravariant
---
<!-- end_slide -->

#  getting started with typing
---
<!-- end_slide -->

#  mindset of using typing
---
<!-- end_slide -->

#  some mypy terms
---
<!-- end_slide -->

#  HKT
---
<!-- end_slide -->

<!-- jump_to_middle -->
Thank you!
---
