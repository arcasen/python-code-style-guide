### 类型注解 

#### 一般规则 

* 熟悉类型提示[^type-hints]。
  
  [^type-hints]: <https://docs.python.org/3/library/typing.html>

* 通常不需要对 `self` 或 `cls` 进行类型注解。如果为了提供正确的类型信息而有必要，可以使用 `Self`[^self]，例如：

  [^self]: <https://docs.python.org/3/library/typing.html#typing.Self>

  ```python
  from typing import Self

  class BaseClass:
    @classmethod
    def create(cls) -> Self:
      ...

    def difference(self, other: Self) -> float:
      ...
  ```

* 同样，不要觉得必须为 `__init__` 的返回值添加类型注解（因为 `None` 是唯一有效的选项）。

* 如果某个变量或返回类型不宜明确表达，请使用 `Any`。

* 你无需为模块中的所有函数都添加类型注解。

  - 至少为你的公共 API 添加类型注解。
  - 在安全性和清晰度与灵活性之间找到一个良好的平衡，需运用判断力。
  - 为容易出现类型相关错误的代码（例如之前的 bug 或复杂代码）添加类型注解。
  - 为难以理解的代码添加类型注解。
  - 为从类型角度来看已经稳定的代码添加类型注解。在许多情况下，你可以为成熟代码中的所有函数添加类型注解，而不会损失太多灵活性。

#### 换行 

遵循现有的缩进规则，见[缩进]。

在添加类型注解后，许多函数签名会变成“每行一个参数”。为了确保返回类型也独占一行，可以在最后一个参数后添加逗号。

```python
def my_method(
    self,
    first_var: int,
    second_var: Foo,
    third_var: Bar | None,
) -> int:
    ...
```

总是优先在变量之间换行，而不是例如在变量名和类型注解之间换行。然而，如果所有内容都能适应同一行，就直接写在一行。

```python
def my_method(self, first_var: int) -> int:
    ...
```

如果函数名、最后一个参数和返回类型组合起来太长，则在新行中缩进 4 个空格。使用换行时，优先将每个参数和返回类型放在单独的行，并将右括号与 `def` 对齐：

正确：

```python
def my_method(
    self,
    other_arg: MyLongType | None,
) -> tuple[MyLongType1, MyLongType1]:
    ...
```

可选地，返回类型可以与最后一个参数放在同一行：

可以：

```python
def my_method(
    self,
    first_var: int,
    second_var: int) -> dict[OtherLongType, MyLongType]:
    ...
```

pylint 允许将右括号移到新行并与左括号对齐，但这种方式可读性较差。

错误：

```python
def my_method(self,
              other_arg: MyLongType | None,
             ) -> dict[OtherLongType, MyLongType]:
    ...
```

如上例所示，尽量不要拆分类型。然而，有时类型太长，无法放在一行（尽量保持子类型不被拆分）。

```python
def my_method(
    self,
    first_var: tuple[list[MyLongType1],
                     list[MyLongType2]],
    second_var: list[dict[
        MyLongType3, MyLongType4]],
) -> None:
    ...
```

如果单个变量名和类型组合太长，考虑为类型使用别名。最后的解决办法是在冒号后换行并缩进 4 个空格。

正确：

```python
def my_function(
    long_variable_name:
        long_module_name.LongTypeName,
) -> None:
    ...
```

错误：

```python
def my_function(
    long_variable_name: long_module_name.
        LongTypeName,
) -> None:
    ...
```

#### 前向声明 

如果需要使用尚未定义的类名（来自同一模块），例如在类声明中需要使用该类名，或者使用在代码中稍后定义的类，可以选择以下两种方式之一：使用 `from __future__ import annotations` 或将类名作为字符串。

正确：

```python
from __future__ import annotations

class MyClass:
    def __init__(self, stack: Sequence[MyClass], item: OtherClass) -> None:
        ...

class OtherClass:
    ...
```

正确：

```python
class MyClass:
    def __init__(self, stack: Sequence['MyClass'], item: 'OtherClass') -> None:
        ...

class OtherClass:
    ...
```

#### 默认值 

根据 PEP 008[^pep008]，只有在参数同时具有类型注解和默认值时，才在 `=` 周围使用空格。

[^pep008]: <https://peps.python.org/pep-0008/#other-recommendations>

正确：

```python
def func(a: int = 0) -> int:
    ...
```

错误：

```python
def func(a:int=0) -> int:
    ...
```

#### NoneType 

在 Python 类型系统中，`NoneType` 是一个“一级”类型，在类型注解中，`None` 是 `NoneType` 的别名。如果一个参数可以是 `None`，必须明确声明！你可以使用 `|` 联合类型表达式（在 Python 3.10+ 的新代码中推荐），或者使用较旧的 `Optional` 和 `Union` 语法。

应使用显式的 `X | None` 而非隐式的。早期的类型检查器允许将 `a: str = None` 解释为 `a: str | None = None`，但这不再是推荐的行为。

正确：

```python
def modern_or_union(a: str | int | None, b: str | None = None) -> str:
    ...
def union_optional(a: Union[str, int, None], b: Optional[str] = None) -> str:
    ...
```

错误：

```python
def nullable_union(a: Union[None, str]) -> str:
    ...
def implicit_optional(a: str = None) -> str:
    ...
```

#### 类型别名 

你可以声明复杂类型的别名。别名的名称应采用 `CapWorded` 形式。如果别名仅在本模块中使用，它应该是 `_Private`。

注意，`: TypeAlias` 注解仅在 Python 3.10+ 版本中支持。

```python
from typing import TypeAlias

_LossAndGradient: TypeAlias = tuple[tf.Tensor, tf.Tensor]
ComplexTFMap: TypeAlias = Mapping[str, _LossAndGradient]
```

#### 忽略类型 

你可以使用特殊注释 `# type: ignore` 来禁用某一行上的类型检查。

`pytype` 提供了一个针对特定错误的禁用选项（类似于 lint）：

```python
# pytype: disable=attribute-error
```

#### 类型化变量 

带注解的赋值
: 如果一个内部变量的类型难以或无法推断，请使用带注解的赋值来指定其类型——在变量名和值之间使用冒号和类型（与为具有默认值的函数参数指定类型的方式相同）：

  ```python
  a: Foo = SomeUndecoratedFunction()
  ```

类型注释
: 虽然在代码库中可能仍会看到类型注释（在 Python 3.6 之前是必需的），但不要再添加行末的 `# type: <type name>` 注释：

  ```python
  a = SomeUndecoratedFunction()  # type: Foo
  ```

#### 元组 vs 列表 

类型化的列表只能包含单一类型的对象。类型化的元组可以具有单一重复类型，或者具有固定数量的、类型各异的元素。后者通常用作函数的返回类型。

```python
a: list[int] = [1, 2, 3]
b: tuple[int, ...] = (1, 2, 3)
c: tuple[int, str, float] = (1, "2", 3.5)
```

#### 类型变量 

Python 类型系统支持泛型，见[泛型]。类型变量（如 `TypeVar` 和 `ParamSpec`）是使用泛型的常用方式。

示例：

```python
from collections.abc import Callable
from typing import ParamSpec, TypeVar

_P = ParamSpec("_P")
_T = TypeVar("_T")

def next(l: list[_T]) -> _T:
    return l.pop()

def print_when_called(f: Callable[_P, _T]) -> Callable[_P, _T]:
    def inner(*args: _P.args, **kwargs: _P.kwargs) -> _T:
        print("Function was called")
        return f(*args, **kwargs)
    return inner
```

类型变量的约束：

`TypeVar` 可以被约束：

```python
AddableType = TypeVar("AddableType", int, float, str)

def add(a: AddableType, b: AddableType) -> AddableType:
    return a + b
```

### 预定义类型变量 `AnyStr`：
`typing` 模块中常用的预定义类型变量是 `AnyStr`，用于可以是 `bytes` 或 `str` 且必须是同一类型的多个注解。

```python
from typing import AnyStr

def check_length(x: AnyStr) -> AnyStr:
    if len(x) <= 42:
        return x
    raise ValueError()
```

类型变量命名规则：

类型变量必须具有描述性名称，除非它满足以下所有条件：
- 不在外部可见
- 未被约束

正确：

```python
_T = TypeVar("_T")
_P = ParamSpec("_P")
AddableType = TypeVar("AddableType", int, float, str)
AnyFunction = TypeVar("AnyFunction", bound=Callable)
```

错误：

```python
T = TypeVar("T")
P = ParamSpec("P")
_T = TypeVar("_T", int, float, str)
_F = TypeVar("_F", bound=Callable)
```

#### 字符串类型 

在新代码中不要使用 `typing.Text`，它仅用于 Python 2/3 兼容性。

对于字符串/文本数据，使用 `str`。对于处理二进制数据的代码，使用 `bytes`。

```python
def deals_with_text_data(x: str) -> str:
    ...

def deals_with_binary_data(x: bytes) -> bytes:
    ...
```

如果函数的所有字符串类型始终相同，例如上述代码中返回类型与参数类型相同，使用 `AnyStr`。

```python
from typing import AnyStr

def process_string(x: AnyStr) -> AnyStr:
    ...
```

#### 用于类型的导入

对于用于支持静态分析和类型检查的 `typing` 或 `collections.abc` 模块中的符号（包括类型、函数和常量），始终直接导入符号本身。这样可以使常用注解更简洁，并与全球通用的类型注解实践保持一致。明确允许从 `typing` 和 `collections.abc` 模块在一行中导入多个特定符号。例如：

```python
from collections.abc import Mapping, Sequence
from typing import Any, Generic, cast, TYPE_CHECKING
```

由于这种导入方式会将符号添加到本地命名空间，`typing` 或 `collections.abc` 中的名称应被视为类似于关键字的地位，不应在 Python 代码中（无论是否带类型注解）定义这些名称。如果模块中存在类型与现有名称的冲突，使用 `import x as y` 方式导入。

```python
from typing import Any as AnyType
```

在为函数签名添加类型注解时，优先使用抽象容器类型（如 `collections.abc.Sequence`），而不是具体类型（如 `list`）。如果需要使用具体类型（例如，带类型元素的元组），优先使用内置类型（如 `tuple`），而不是 `typing` 模块中的参数化类型别名（例如，`typing.Tuple`）。

```python
from typing import List, Tuple

def transform_coordinates(original: List[Tuple[float, float]]) -> List[Tuple[float, float]]:
    ...
```

```python
from collections.abc import Sequence

def transform_coordinates(original: Sequence[tuple[float, float]]) -> Sequence[tuple[float, float]]:
    ...
```

#### 条件导入 

仅在特殊情况下使用条件导入，即当需要避免在运行时加载仅用于类型检查的额外导入时。这种模式不被鼓励，应优先考虑替代方案，例如重构代码以允许顶层导入。

仅用于类型注解的导入可以放在 `if TYPE_CHECKING:` 块中。

条件导入的类型需要以字符串形式引用，以确保与 Python 3.6 的向前兼容，因为在 Python 3.6 中注解表达式会被实际执行。
只有专门用于类型注解的实体（包括别名）才应在此定义；否则会导致运行时错误，因为该模块在运行时不会被导入。
该块应紧跟在所有常规导入之后。
类型导入列表中不应有空行。
该列表的排序应如同常规导入列表一样。

```python
import typing

if typing.TYPE_CHECKING:
    import sketch

def f(x: "sketch.Sketch") -> None:
    ...
```

#### 循环依赖 

由于类型注解导致的循环依赖是一种代码异味（code smell）。此类代码是重构的理想候选。虽然技术上可以保留循环依赖，但许多构建系统不允许这样做，因为每个模块都必须依赖另一个模块。

将导致循环依赖导入的模块替换为 `Any`。使用一个有意义的别名，并使用该模块中的真实类型名称（`Any` 的任何属性都是 `Any`）。别名定义应与最后一个导入之间隔一行。

```python
from typing import Any

some_mod = Any  # some_mod.py imports this module.

def my_method(self, var: "some_mod.SomeType") -> None:
    ...
```

#### 泛型 

在进行类型注解时，优先为泛型类型的参数列表指定类型参数；否则，泛型的参数将被假定为 `Any`。

正确：

```python
def get_names(employee_ids: Sequence[int]) -> Mapping[int, str]:
    ...
```

错误：

```python
# 这被解释为 get_names(employee_ids: Sequence[Any]) -> Mapping[Any, Any]
def get_names(employee_ids: Sequence) -> Mapping:
    ...
```

如果泛型的最佳类型参数是 `Any`，请明确指定，但请记住，在许多情况下 `TypeVar` 可能更合适：

错误：

```python
def get_names(employee_ids: Sequence[Any]) -> Mapping[Any, str]:
    """Returns a mapping from employee ID to employee name for given IDs."""
```

正确：

```python
_T = TypeVar('_T')

def get_names(employee_ids: Sequence[_T]) -> Mapping[_T, str]:
    """Returns a mapping from employee ID to employee name for given IDs."""
```
