---
title: typing in Python
author: Lawrence (lawrence@cinnamon.is)
---

Why typing?
===
<!-- column_layout: [1, 1, 1] -->
<!-- column: 0 -->
<!-- column: 1 -->
---
# Philosophy

## Python

## We are developers, not coders

## Robustness and consistency

---

Naive
```rust +line_numbers
def find_center(coords):
    return (
        (coords[0] + coords[2]) / 2,
        (coords[1] + coords[3]) / 2,
    )
```

More cognition
```rust +line_numbers
def find_center(
    coords: tuple[float, float, float, float],
) -> tuple[float, float]:
    return (
        (coords[0] + coords[2]) / 2,
        (coords[1] + coords[3]) / 2,
    )
```

More into typing
```rust +line_numbers
type Point = tuple[float, float]
type Rectangle = tuple[float, float, float, float]

def find_center(coords: Rectangle]) -> Point:
    return (
        (coords[0] + coords[2]) / 2,
        (coords[1] + coords[3]) / 2,
    )
```
---
<!-- column: 2 -->
<!-- reset_layout -->
<!-- end_slide -->

PERIOD
===
---
# auto-completion
With proper typing, you should be PERIOD-able.

```
class AzureOpenAIChatModel:
    async def achat(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AssistantMessage:
        self.________________________
            |achat                  m|
            |astream                m|
            |async_client           v|
            |chat                   m|
            |cost                   v|
            |from_deployment        m|
            |gather_context         m|
            |model                  p|
            |settings               v|
            |sync_client            v|
            |to_cost                m|
            |to_openai_chat_message m|
            |_generate_messages     m|
            |_model                 v|
```

# typing is another kind of system

## structural (statis) typing vs. dynamic typing

## runtime

## typing

---
<!-- end_slide -->

typing basic
===
---
# Primitives
- numeric: int, float, complex
- sequence: str, list, tuple, range
- binary: bytes, bytearry, memoryview
- set: set, frozen
- mapping: dict
- None: None
- others: callable, iterator, generator, coroutine, ...

# Type alias
```rust +line_numbers
from typing import TypeAlias

# prior to python3.12
ManyInts: TypeAlias = list[int]

# now
type ManyInts = list[int]
```

# TypedDict
```rust +line_numbers
from typing import NotRequired, TypedDict


class Result(TypedDict):
    embedding: Sequence[float]
    vectordb_id: str


class APIResponse(TypedDict):
    id: UUID
    result: NotRequired[Result]
    finished_at: datetime
```

# Stub

```rust +line_numbers
class Spaceship:
    def reach_universe(self, universe_name: str) -> None: ...
    def memorize(self) -> None: ...
    def deploy_human(self, num_ppl: int) -> int: ...
```

# Special types

## Any

## None

## NoReturn

## Never

## type[]

---
<!-- end_slide -->

collections' types
===
---
Most common types only

# collections.abc
## Iterable
## Sequence
## Mapping
## [Async]Iterator
## [Async]Generator

# collections
## DefaultDict
## OrderedDict
## ChainMap
## Counter
## Deque

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
- Unpack (later)

# 3.12
- New generic syntax
- New type alias syntax

# Honorable mentions
- Annotated
    - [See more](https://typing.readthedocs.io/en/latest/spec/qualifiers.html#annotated)
- Literal

---
<!-- end_slide -->

Generic
===
---
# Why Generic?
```rust +line_numbers
class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: List[T] = []

    def push(self, item: T) -> None:
        """Push an item onto the stack."""
        self._items.append(item)

    def pop(self) -> T:
        """Pop an item off the stack."""
        if not self._items:
            raise IndexError("pop from an empty stack")
        return self._items.pop()

    def peek(self) -> T:
        """Peek at the top item without removing it."""
        if not self._items:
            raise IndexError("peek from an empty stack")
        return self._items[-1]

    def is_empty(self) -> bool:
        """Check if the stack is empty."""
        return not self._items
```

## Generic in other languages
- Compiling language
- Scripting language

# Built-in generic types
- list[T]
- tuple[T1, T2]
- tuple[T1, ...]
- dict[K, V]
- Iterable[T]
- Sequence[T]
- Mapping[K, V]
- type[C]
- Callable[[T1, T2, ...], RT]
- Coroutine[Any, Any, T]
- [Async]Generator[T]

---
<!-- end_slide -->

Functional programming support
===
---
# Callable

# ParamSpec

# Overload

# Concatenate

---
<!-- end_slide -->

OOP support
===
---
# @staticmethod and @classmethod

# @property & @property.setter

# @override and @overload

# ClassVar

# Final

---
<!-- end_slide -->

Interface
===

---
# Motivation
- Coupling between base class and derived class
- Circular import

# Interface in compiling languages
- Single inheritance
- Single inheritance + multiple implementations (interfaces)
- Multiple inheritances
---
<!-- end_slide -->

Interface (Example)
===

# Protocol
```rust +line_numbers
class IChatModel(Protocol):
    async def achat(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AssistantMessage: ...

    async def astream(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AsyncGenerator[AssistantMessage, None]: ...
```

## Subclassing
```rust +line_numbers
class BaseChatModel[ModelT]:
    @property
    def model(self) -> ModelT:
        raise NotImplementedError

    async def achat(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AssistantMessage:
        raise NotImplementedError

    async def astream(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AsyncGenerator[AssistantMessage, None]:
        raise NotImplementedError

class OpenAIChatModel(BaseChatModel[AzureOpenAI]):
    def __init__(self, model: AzureOpenAI) -> None:
        self.model: AzureOpenAI = model

    async def achat(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AssistantMessage: ...

    async def astream(
        self,
        message: UserMessage,
        history: Sequence[Message] | None = None,
        *args: Any,
        **kwargs: Any,
    ) -> AsyncGenerator[AssistantMessage, None]: ...
```

## Implicit subtype

---
<!-- end_slide -->

Polymorphism
===
---
```rust +line_numbers
from concurrent.futures import (
    Executor,
    ProcessPoolExecutor,
    ThreadPoolExecutor,
)
from typing import Literal, overload


class ExecutorFactory:
    @overload
    def create(
        self,
        executor_type: Literal["thread"],
    ) -> ThreadPoolExecutor: ...

    @overload
    def create(
        self,
        executor_type: Literal["process"],
    ) -> ProcessPoolExecutor: ...

    def create(
        self,
        executor_type: Literal["thread", "process"],
    ) -> Executor:
        match executor_type:
            case "thread":
                return ThreadPoolExecutor()
            case "process":
                return ProcessPoolExecutor()
            case _:
                raise RuntimeError("invalid executor type")
```

---
<!-- end_slide -->

Liskov
===

---

---
<!-- end_slide -->


TypeVar, TypeVarTuple, ParamSpec, Concatenate
===

---
# TypeVar

## Upperbound

# TypeVarTuple

# ParamSpec

# Concatenate

---
<!-- end_slide -->

mypy terminology
===

---
# Type narrowing
- isinstance
- issubclass
- type
- callable
## TypeGuard
## TypeIs
# overload
# Final and final
# reveal_type

---
<!-- end_slide -->

Covariant, Contravariant, Invariant
===

---
# Covariant

```rust +line_numbers
def count_lines(shapes: Sequence[Shape]) -> int:
    return sum(shape.num_sides for shape in shapes)

triangles: Sequence[Triangle]
count_lines(triangles)  # OK
```

# Contravariant

```rust +line_numbers
def cost_of_paint_required(
    triangle: Triangle,
    area_calculator: Callable[[Triangle], float]
) -> float:
    return area_calculator(triangle) * DOLLAR_PER_SQ_FT

# This straightforwardly works
def area_of_triangle(triangle: Triangle) -> float: ...
cost_of_paint_required(triangle, area_of_triangle)  # OK

# But this works as well!
def area_of_any_shape(shape: Shape) -> float: ...
cost_of_paint_required(triangle, area_of_any_shape)  # OK
```

# Invariant

```rust +line_numbers
class Circle(Shape):
    # The rotate method is only defined on Circle, not on Shape
    def rotate(self): ...

def add_one(things: list[Shape]) -> None:
    things.append(Shape())

my_circles: list[Circle] = []
add_one(my_circles)     # This may appear safe, but...
my_circles[-1].rotate()  # ...this will fail, since my_circles[0] is now a Shape, not a Circle
```

## Sequence vs. list


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

# Documentation
- [Official mypy cheatsheet](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)

---
# Mindset of using typing

## py.typed

## Write all functions with typing

## Think in OOP

## Satisfy both worlds

---
# Handling legacy code

## Start small

## Gradual adoption

## Design concern & improvement loop

---
<!-- end_slide -->

HKT
===

---

---
<!-- end_slide -->

<!-- jump_to_middle -->
Examples and Demos
---
<!-- end_slide -->

<!-- jump_to_middle -->
Thank you!
---
