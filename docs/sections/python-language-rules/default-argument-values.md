### 默认参数值 

在大多数情况下可以使用。

#### 定义 

您可以在函数参数列表末尾指定变量的值，例如，`def foo(a, b=0):`。如果仅用一个参数调用 `foo`，则 `b` 设置为 0。如果用两个参数调用，则 `b` 具有第二个参数的值。

#### 优点 

通常您有一个使用大量默认值的函数，但偶尔您想覆盖默认值。默认参数值提供了一种简单的方法来实现这一点，而无需为罕见的例外定义许多函数。由于 Python 不支持重载方法/函数，默认参数是一种“模拟”重载行为的简单方式。

#### 缺点 

默认参数在模块加载时求值一次。如果参数是可变对象，如列表或字典，这可能会导致问题。如果函数修改对象（例如，通过向列表追加项），默认值会被修改。

#### 决定 

可以使用，但有以下注意事项：

不要在函数或方法定义中使用可变对象作为默认值。

正确：

```python
def foo(a, b=None):
    if b is None:
        b = []
def foo(a, b: Sequence | None = None):
    if b is None:
        b = []
def foo(a, b: Sequence = ()):  # Empty tuple OK since tuples are immutable.
    ...
```

错误：

```python
from absl import flags
_FOO = flags.DEFINE_string(...)

def foo(a, b=[]):
    ...
def foo(a, b=time.time()):  # Is `b` supposed to represent when this module was loaded?
    ...
def foo(a, b=_FOO.value):  # sys.argv has not yet been parsed...
    ...
def foo(a, b: Mapping = {}):  # Could still get passed to unchecked code.
    ...
```
