### 导入格式

导入语句应独占一行，但 `typing` 和 `collections.abc` 的导入可以例外（见[用于类型的导入]）。

示例：

正确：

```python
from collections.abc import Mapping, Sequence
import os
import sys
from typing import Any, NewType
```

错误：

```python
import os, sys
```

导入语句始终放在文件顶部，仅在模块注释和文档字符串之后，模块全局变量和常量之前。导入应按从最通用到最不通用的顺序分组：

1. Python 未来导入语句。例如：
   ```python
   from __future__ import annotations
   ```
   有关详情见[现代 Python：`from __future__ import`]。

2. Python 标准库导入。例如：
   ```python
   import sys
   ```

3. 第三方模块或包导入。例如：
   ```python
   import tensorflow as tf
   ```

4. 代码仓库子包导入。例如：
   ```python
   from otherproject.ai import mind
   ```

5. 已弃用：与当前文件属于同一顶级子包的应用程序特定导入。例如：
   ```python
   from myproject.backend.hgwells import time_machine
   ```
   你可能会在较旧的 Google Python 风格代码中看到这种做法，但这不再是必需的。鼓励新代码不再遵循此方式，简单地将应用程序特定的子包导入视为与其他子包导入相同。

在每个分组内，导入应按模块的完整包路径（`from path import ...` 中的路径）进行字典序排序，忽略大小写。代码可以在导入分组之间选择性地添加空行。

```python
import collections
import queue
import sys

from absl import app
from absl import flags
import bs4
import cryptography
import tensorflow as tf

from book.genres import scifi
from myproject.backend import huxley
from myproject.backend.hgwells import time_machine
from myproject.backend.state_machine import main_loop
from otherproject.ai import body
from otherproject.ai import mind
from otherproject.ai import soul

# Older style code may have these imports down here instead:
#from myproject.backend.hgwells import time_machine
#from myproject.backend.state_machine import main_loop
```
