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
```rust +line_numbers
class Veggie: ...

class Herbivore[VegT: Veggie]:
    def chew(self, veg: VegT) -> None: ...
```
- New type alias syntax

```rust +line_numbers
type Number = int | float
```

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
        self._items: list[T] = []

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


TypeVar, TypeVarTuple, ParamSpec, Concatenate
===
---
# TypeVar

## Upperbound

# TypeVarTuple

# ParamSpec

# Concatenate

---
```rust +line_numbers
from typing import ParamSpec, TypeVar

P = ParamSpec("P")
T = TypeVar("T")
RT = TypeVar("RT")
FileStorageT = TypeVar("FileStorageT", bound="IFileStorage[Any]")

def auto_catch_native_exc(
    func: Callable[Concatenate[FileStorageT, P], RT],
) -> Callable[Concatenate[FileStorageT, P], RT]:
    @wraps(func)
    def wrapper(
        self: FileStorageT,
        /,
        *args: P.args,
        **kwargs: P.kwargs,
    ) -> RT:
        try:
            return func(self, *args, **kwargs)
        except ValueError as value_error:
            raise InvalidArgumentsError(value_error) from value_error
        except FileStorageError:
            raise
        except Exception as exc:
            logger.exception("Unexpected error occurred")
            raise OperationalError(exc) from exc

    return wrapper
```

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
```rust +line_numbers
from typing import TypeGuard

# TypeGuard function to check if an object is an int
def is_list_of_ints(items: list[object]) -> TypeGuard[list[int]]:
    return all(isinstance(item, int) for item in items)

# Example function that processes a list, narrowing its type
def process_items(items: list[object]) -> None:
    if is_list_of_ints(items):
        # The type of `items` is now narrowed to `list[int]` within this block
        total = sum(items)  # Safe because items are guaranteed to be integers
        print(f"Sum of integers: {total}")
    else:
        # Here, `items` is still `list[object]`
        print("The list is not a list of integers.")
```

## TypeIs
TypeIs narrows the type in both the if and else branches, whereas TypeGuard narrows only in the if branch.
```rust +line_numbers
from typing import assert_type, final, TypeIs

class Parent: pass
class Child(Parent): pass
@final
class Unrelated: pass

def is_parent(val: object) -> TypeIs[Parent]:
    return isinstance(val, Parent)

def run(arg: Child | Unrelated):
    if is_parent(arg):
        # Type of ``arg`` is narrowed to the intersection
        # of ``Parent`` and ``Child``, which is equivalent to
        # ``Child``.
        assert_type(arg, Child)
    else:
        # Type of ``arg`` is narrowed to exclude ``Parent``,
        # so only ``Unrelated`` is left.
        assert_type(arg, Unrelated)
```
---
<!-- end_slide -->

mypy terminology (cont.)
===
---

# overload

# Final and final
```rust +line_numbers
from typing import final

@final
class Settings:
    """A class representing application settings that must not be subclassed."""
    def __init__(self, app_name: str, version: str):
        self.app_name = app_name
        self.version = version

    def get_details(self) -> str:
        return f"{self.app_name} v{self.version}"

# Attempting to subclass Settings will raise a type checker error
# class AdvancedSettings(Settings):  # This will fail with tools like mypy or Pyright
#     pass

class Config:
    """A class representing configurable properties."""

    @final
    def get_value(self, key: str) -> str:
        """This method must not be overridden."""
        return f"Fetching value for {key}"

class MyConfig(Config):
    # This will cause an error with static type checkers
    # def get_value(self, key: str) -> str:
    #     return f"Overridden value for {key}"
    def some_other_method(self) -> None:
        print("This is allowed because it doesn't override get_value.")
```

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

Liskov
===
---
# The Liskov Substitution Principle (LSP)
states that objects of a superclass should be replaceable with objects of a subclass without altering the correctness of the program. In Python, we can demonstrate this principle with typing by using Protocol to define a contract for subclasses to follow.

---
```rust +line_numbers
from typing import Protocol, TypeVar

# Base class for all animals
class Animal:
    def speak(self) -> str:
        return "Some generic animal sound"

# Subclass: Dog
class Dog(Animal):
    def speak(self) -> str:
        return "Bark"

# Subclass: Cat
class Cat(Animal):
    def speak(self) -> str:
        return "Meow"

# Protocol for a Trainer
class Trainer:
    def train(self, animal: Animal) -> str:
        """Train the given animal"""
        raise NotImplementedError

# Specialized Trainer for Dogs
class DogTrainer(Trainer):
    def train(self, animal: Dog) -> str:  # error [override]
        return f"Training a dog to bark: {animal.speak()}"

# Specialized Trainer for Dogs
class CatTrainer(Trainer):
    def train(self, animal: Cat) -> str:  # error [override]
        return f"Training a cat to meow: {animal.speak()}"

# Function that accepts a Trainer of any type
def start_training(trainer: Trainer[Animal], animals: list[Animal]) -> None:
    for animal in animals:
        print(trainer.train(animal))
```
---
```
error: Argument 1 of "train" is incompatible with supertype "Trainer"; supertype defines the argument type as "Animal"  [override]
note: This violates the Liskov substitution principle
note: See https://mypy.readthedocs.io/en/stable/common_issues.html#incompatible-overrides
```
---
<!-- end_slide -->

Liskov (Generic)
===
---
```rust +line_numbers
from typing import Protocol, TypeVar

# Base class for all animals
class Animal:
    def speak(self) -> str:
        return "Some generic animal sound"

# Subclass: Dog
class Dog(Animal):
    def speak(self) -> str:
        return "Bark"

# Subclass: Cat
class Cat(Animal):
    def speak(self) -> str:
        return "Meow"

# Define a contravariant TypeVar
T = TypeVar('T', bound=Animal, contravariant=True)

# Protocol for a Trainer
class Trainer(Protocol[T]):
    def train(self, animal: T) -> str:
        """Train the given animal"""
        raise NotImplementedError

# Specialized Trainer for Dogs
class DogTrainer(Trainer[Dog]):
    def train(self, animal: Dog) -> str:
        return f"Training a dog to bark: {animal.speak()}"

# Specialized Trainer for Dogs
class CatTrainer(Trainer[Cat]):
    def train(self, animal: Cat) -> str:
        return f"Training a cat to meow: {animal.speak()}"

# Function that accepts a Trainer of any type
def start_training(trainer: Trainer[Animal], animals: list[Animal]) -> None:
    for animal in animals:
        print(trainer.train(animal))
```

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
Placed in the "module folder" to indicate that typing in the module can be used for type-checking.

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
Polymorphism abstracts types, just as functions abstract values. Higher-kinded
polymorphism takes things a step further, abstracting both types and type constructors,

# Examples
<!-- column_layout: [1, 2, 1] -->
<!-- column: 0 -->
<!-- column: 1 -->
---
```rust +line_numbers
from typing import Iterable

T = TypeVar('T', bound=Iterable)

def stringify_iterable_items(arg: T[int]) -> T[str]:
    return type(arg)(str(item) for item in arg)
```
---

```rust +line_numbers
class Language(Enum): ...

class Params[LanguageT: Language]:
    target_language: LanguageT

class Tasks2[LanguageT: Language, ParamsListT: list[Params[Language]]]:
    source_language: LanguageT
    params: ParamsListT
```
---
```rust +line_numbers
# The following generates no compiler error, but a type checker
# should generate an error because an upper bound type must be concrete,
# and ``Sequence[S]`` is generic. Future extensions to the type system may
# eliminate this limitation.
class ClassA[S, T: Sequence[S]]: ...

# The following generates no compiler error, because the bound for ``S``
# is lazily evaluated. However, type checkers should generate an error.
class ClassB[S: Sequence[T], T]: ...
```
---
<!-- column: 2 -->
<!-- reset_layout -->
<!-- end_slide -->

<!-- jump_to_middle -->
Examples, Demos and FAQs
---
<!-- end_slide -->

<!-- jump_to_middle -->
Thank you!
---
