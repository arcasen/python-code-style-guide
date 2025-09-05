### 导入 

仅使用 `import` 语句导入包和模块，而非单个类型、类或函数。

#### 定义 

从一个模块共享代码到另一个模块的重用机制。

#### 优点 

命名空间管理约定简单。每个标识符的来源以一致的方式指示；`x.Obj` 表示对象 `Obj` 在模块 `x` 中定义。

#### 缺点 

模块名称仍可能冲突。有些模块名称过于冗长。

#### 决定 

* 使用 `import x` 导入包和模块。
* 使用 `from x import y`，其中 `x` 是包前缀，`y` 是无前缀的模块名称。
* 在以下任何情况下使用 `from x import y as z`：
  - 需要导入两个名为 `y` 的模块。
  - `y` 与当前模块中定义的顶级名称冲突。
  - `y` 与公共 API 中常见的参数名称冲突（例如，`features`）。
  - `y` 是一个过于冗长的名称。
  - `y` 在您的代码上下文中过于通用（例如，`from storage.file_system import options as fs_options`）。
* 仅当 `z` 是标准缩写时使用 `import y as z`（例如，`import numpy as np`）。

例如，模块 `sound.effects.echo` 可以如下导入：

```python
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```

在导入中不要使用相对名称。即使模块在同一包中，也要使用完整的包名称。这有助于防止意外导入一个包两次。

##### 豁免 

此规则的豁免：

* 来自以下模块的符号用于支持静态分析和类型检查：
  - `typing` 模块（见[用于类型的导入]）
  - `collections.abc` 模块（见[用于类型的导入]）
  - `typing_extensions` 模块[^typ_ext]
* 来自 `six.moves` 模块[^moves] 的重定向。
  
[^typ_ext]: <https://github.com/python/typing_extensions/blob/main/README.md>
[^moves]: <https://six.readthedocs.io/#module-six.moves>
