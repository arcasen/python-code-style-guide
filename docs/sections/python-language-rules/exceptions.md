### 异常 

允许使用异常，但必须小心使用。

#### 定义 

异常是一种跳出正常控制流的手段，用于处理错误或其他异常情况。

#### 优点 

正常操作代码的控制流不会因错误处理（error-handling）代码而变得杂乱。它还允许当某些条件发生时，控制流跳过多个帧（frames），例如，从第 N 层嵌套函数中一步返回，而不是必须通过它们传递错误代码。

#### 缺点 

可能导致控制流令人困惑。在调用库时容易遗漏错误情况。

#### 决定 

异常必须遵循某些条件：

* 当合适时，使用内置异常类。例如，当违反前提条件时（如验证函数参数时可能发生的情况），引发 `ValueError` 以指示编程错误。

* 不要使用 `assert` 语句代替条件或验证前提条件。它们不得对应用程序逻辑至关重要。试金石是 `assert` 可以被移除而不破坏代码。`assert` 条件不能保证被求值[^assert]。对于基于 pytest[^pytest] 的测试，`assert` 是可以的，并且期望用于验证期望。例如：

  <!-- 必须缩进 否则将结束列表 -->
  [^assert]: <https://docs.python.org/3/reference/simple_stmts.html#the-assert-statement>

  [^pytest]: <https://pytest.org>
    
  正确：

  ```python
  def connect_to_next_port(self, minimum: int) -> int:
    """Connects to the next available port.

    Args:
      minimum: A port value greater or equal to 1024.

    Returns:
      The new minimum port.

    Raises:
      ConnectionError: If no available port is found.
    """
    if minimum < 1024:
      # Note that this raising of ValueError is not mentioned in the doc
      # string's "Raises:" section because it is not appropriate to
      # guarantee this specific behavioral reaction to API misuse.
      raise ValueError(f'Min. port must be at least 1024, not {minimum}.')
    port = self._find_next_open_port(minimum)
    if port is None:
      raise ConnectionError(
          f'Could not connect to service on port {minimum} or higher.')
    # The code does not depend on the result of this assert.
    assert port >= minimum, (
        f'Unexpected port {port} when minimum was {minimum}.')
    return port
  ```

  错误：
  
  ```python
  def connect_to_next_port(self, minimum: int) -> int:
    """Connects to the next available port.

    Args:
      minimum: A port value greater or equal to 1024.

    Returns:
      The new minimum port.
    """
    assert minimum >= 1024, 'Minimum port must be at least 1024.'
    # The following code depends on the previous assert.
    port = self._find_next_open_port(minimum)
    assert port is not None
    # The type checking of the return statement relies on the assert.
    return port
  ```
  
* 库或包可能定义自己的异常。当这样做时，它们必须从现有异常类继承。异常名称应以 `Error` 结尾，并且不应引入重复（`foo.FooError`）。

* 永远不要使用捕获所有异常的（catch-all） `except:` 语句，或捕获 `Exception` 或 `StandardError`，除非您是

  - 重新引发异常，或者
  - 在程序中创建一个隔离点，其中异常不传播但被记录和抑制，例如，保护线程免于崩溃，通过守护其最外层块。

  Python 在这方面非常宽容，`except:` 真的会捕获一切，包括拼写错误的名称、`sys.exit()` 调用、Ctrl+C 中断、unittest 失败以及您根本不想捕获的其他各种异常。

* 最小化 `try`/`except` 块中的代码量。`try` 的主体越大，您没想到会引发异常的代码行引发异常的可能性越大。在这些情况下，`try`/`except` 块会隐藏真正的错误。

* 使用 `finally` 子句无论 `try` 块中是否引发异常都执行代码。这通常用于清理，例如关闭文件。
