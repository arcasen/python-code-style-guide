### 词法作用域 

可以使用。

#### 定义 

嵌套的 Python 函数可以引用封闭函数中定义的变量，但不能为其赋值。变量绑定使用词法作用域解析，即基于静态程序文本。对块中名称的任何赋值都会导致 Python 将对该名称的所有引用视为局部变量，即使使用在赋值之前。如果发生全局声明，则名称被视为全局变量。

此功能的示例是：

```python
def get_adder(summand1: float) -> Callable[[float], float]:
    """Returns a function that adds numbers to a given number."""
    def adder(summand2: float) -> float:
        return summand1 + summand2

    return adder
```

#### 优点 

通常会导致更清晰、更优雅的代码。尤其是对有经验的 Lisp 和 Scheme（以及 Haskell 和 ML 等）程序员来说很舒适。

#### 缺点 

可能导致令人困惑的 bug，例如基于 PEP 0227^[<https://peps.python.org/pep-0227/>] 的此示例：

```python
i = 4
def foo(x: Iterable[int]):
    def bar():
        print(i, end='')
    # ...
    # A bunch of code here
    # ...
    for i in x:  # Ah, i *is* local to foo, so this is what bar sees
        print(i, end='')
    bar()
```

所以 `foo([1, 2, 3])` 将打印 `1 2 3 3`，而不是 `1 2 3 4`。

#### 决定 

可以使用。
