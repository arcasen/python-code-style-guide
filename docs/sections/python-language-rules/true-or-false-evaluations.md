### True/False 求值 

尽可能使用“隐式” false（有一些注意事项）。

#### 定义 

在布尔上下文中，Python 将某些值评估为 `False`。一个快速的“经验法则”是所有“空”值都被视为 false，因此 ` , None, [], {}, ''` 在布尔上下文中都求值为 false。

#### 优点 

使用 Python 布尔值的条件更容易阅读且不易出错。在大多数情况下，它们也更快。

#### 缺点 

对 C/C++ 开发人员可能看起来奇怪。

#### 决定 

尽可能使用“隐式” `false`，例如，`if foo:` 而非 `if foo != []:`。但有一些注意事项需要牢记：

* 始终使用 `if foo is None:`（或 `is not None`）检查 `None` 值。例如，当测试默认值为 `None` 的变量或参数是否设置为其他值时。其他值可能在布尔上下文中为 `false` ！

* 永远不要使用 `==` 将布尔变量与 `False` 比较。改为使用 `if not x:`。如果需要区分 `False` 和 `None`，则链式表达式，如 `if not x and x is not None:`。

* 对于序列（字符串、列表、元组），使用空序列为 `false` 的事实，因此 `if seq:` 和 `if not seq:` 分别优于 `if len(seq):` 和 `if not len(seq):`。

* 处理整数时，隐式 `false` 可能涉及比好处更多的风险（即，意外地将 `None` 处理为 0）。您可以将已知为整数的值（且不是 `len()` 的结果）与整数 0 比较。

  正确：

  ```python
  if not users:
     print('no users')

  if i % 10 == 0:
     self.handle_multiple_of_ten()

  def f(x=None):
     if x is None:
         x = []
  ```

  错误：

  ```python
  if len(users) == 0:
     print('no users')

  if not i % 10:
     self.handle_multiple_of_ten()

  def f(x=None):
     x = x or []
  ```

* 注意，`'0'`（即，字符串形式的 `0`）求值为 `true` 。

* 注意，Numpy 数组在隐式布尔上下文中可能引发异常。在测试 `np.array` 的空时，优先使用 `.size` 属性（例如 `if not users.size`）。