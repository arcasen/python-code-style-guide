### 包 

使用模块的完整路径位置导入每个模块。

#### 优点 

避免模块名称冲突或由于模块搜索路径不是作者预期的那样而导致的错误导入。更容易找到模块。

#### 缺点 

使部署代码更难，因为您必须复制包层次结构。使用现代部署机制并不是真正的问题。

#### 决定 

所有新代码都应通过其完整包名称导入每个模块。

导入应如下：

正确：
```python
# Reference absl.flags in code with the complete name (verbose).
import absl.flags
from doctor.who import jodie

_FOO = absl.flags.DEFINE_string(...)
```

正确：
```python
# Reference flags in code with just the module name (common).
from absl import flags
from doctor.who import jodie

_FOO = flags.DEFINE_string(...)
```

*(假设此文件位于 `doctor/who/` 中，其中 `jodie.py` 也存在)*

错误：
```python
# Unclear what module the author wanted and what will be imported.  The actual
# import behavior depends on external factors controlling sys.path.
# Which possible jodie module did the author intend to import?
import jodie
```

主二进制文件所在的目录不应假设在 `sys.path` 中，尽管在某些环境中会发生这种情况。既然如此，代码应假设 `import jodie` 指的是名为 `jodie` 的第三方或顶级包，而不是本地的 `jodie.py`。