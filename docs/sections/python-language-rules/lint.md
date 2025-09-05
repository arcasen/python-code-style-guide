### Lint 

使用本项目的 pylintrc[^pylintrc]， 并运行 `pylint` 检查您的代码。

[^pylintrc]: <https://github.com/google/styleguide/blob/gh-pages/pylintrc>

#### 定义 

`pylint` 是一个用于查找 Python 源代码中 bug 和风格问题的工具。它会发现通常由非动态的语言如 C 和 C++ 编译器捕获的问题。由于 Python 的动态特性，有些警告可能不正确；然而，虚假警告应该很少见。

#### 优点 

捕获容易遗漏的错误，如拼写错误、使用变量前未赋值等。

#### 缺点 

`pylint` 并不完美。要利用它，有时我们需要绕过它、抑制其警告或修复它。

#### 决定 

确保运行 `pylint` 检查您的代码。

如果警告不合适，请抑制它们，以免隐藏其他问题。您可以在行级注释中设置：

```python
def do_PUT(self):  # WSGI name, so pylint: disable=invalid-name
  ...
```

`pylint` 警告每个都由符号名称（`empty-docstring`）标识。Google 特定的警告以 `g-` 开头。

如果抑制的原因从符号名称中不清楚，请添加解释。

以这种方式抑制的好处是我们可以轻松搜索抑制并重新审视它们。

您可以通过以下方式获取 `pylint` 警告列表：

```shell
pylint --list-msgs
```

要获取特定消息的更多信息，请使用：

```shell
pylint --help-msg=invalid-name
```

优先使用 `pylint: disable` 而非已弃用的旧形式 `pylint: disable-msg`。

可以通过在函数开头删除变量来抑制未使用参数警告。请始终包含解释为什么删除它的注释。“Unused.” 就足够了。例如：

```python
def viking_cafe_order(spam: str, beans: str, eggs: str | None = None) -> str:
    del beans, eggs  # Unused by vikings.
    return spam + spam + spam
```

抑制此警告的其他常见形式包括使用 '`_`' 作为未使用参数的标识符，或在参数名称前添加 '`unused_`'，或将它们分配给 '`_`'。这些形式允许但不再鼓励。这些会破坏按名称传递参数的调用者，并且不强制参数实际上未被使用。
