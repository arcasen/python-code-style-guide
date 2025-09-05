### 命名

命名应具有描述性。这包括函数、类、变量、属性、文件以及任何其他类型的命名实体。

避免使用缩写。特别是，不要使用对项目外部读者来说模糊或不熟悉的缩写，也不要通过删除单词中的字母来缩写。

始终使用 `.py` 文件扩展名。不要使用破折号。

命名示例：

- 模块名：`module_name`
- 包名：`package_name`
- 类名：`ClassName`
- 方法名：`method_name`
- 异常名：`ExceptionName`
- 函数名：`function_name`
- 全局常量名：`GLOBAL_CONSTANT_NAME`
- 全局变量名：`global_var_name`
- 实例变量名：`instance_var_name`
- 函数参数名：`function_parameter_name`
- 局部变量名：`local_var_name`
- 查询专有名词：`query_proper_noun_for_thing`
- 通过 HTTPS 发送缩写：`send_acronym_via_https`

#### 要避免的名称 

- 除非在特定允许的情况下，避免使用单字符命名，允许的单字符命名情况：

  - 计数器或迭代器（例如：`i`, `j`, `k`, `v` 等）
  - `try/except` 语句中的异常标识符 `e`
  - `with` 语句中的文件句柄 `f`
  - 无约束的私有类型变量（例如：`_T = TypeVar("_T")`, `_P = ParamSpec("_P")`）
  - 与参考论文或算法中已建立的数学符号相匹配的名称（参见[数学符号]）

  请注意不要滥用单字符命名。一般来说，命名的描述性应与名称的可见性范围成正比。例如，`i` 对于一个 5 行的代码块可能是合适的，但在多个嵌套作用域中，它可能过于模糊。

- 任何包/模块名中使用破折号（`-`）
- `__double_leading_and_trailing_underscore__` 的名称（Python 保留）
- 冒犯性术语
- 不必要地包含变量类型的名称（例如：`id_to_name_dict`）

#### 命名约定 

- “内部”指的是模块内部，或者类中的受保护或私有成员。

- 在模块变量或函数名前添加单个下划线（`_`）在一定程度上可以保护这些成员（代码检查工具会标记对受保护成员的访问）。注意，单元测试访问被测试模块中的受保护常量是可以的。

- 在实例变量或方法名前添加双下划线（`__`，即“dunder”）会通过名称改编（name mangling）使其对类有效私有；我们不鼓励使用这种方式，因为它会影响可读性和可测试性，而且实际上并非真正私有。优先使用单个下划线。

- 将相关的类和顶级函数放在同一个模块中。与 Java 不同，Python 不需要限制每个模块只包含一个类。

- 类名使用 `CapWords` 风格，模块名使用 `lower_with_under.py` 风格。尽管有一些旧模块使用 `CapWords.py` 命名，但现在不鼓励这样做，因为当模块名与类名相同时会引起混淆。（例如：“等等——我写的是 `import StringIO` 还是 `from StringIO import StringIO`？”）

- 新的单元测试文件应遵循 PEP 8 兼容的 `lower_with_under` 方法命名，例如 `test_<method_under_test>_<state>`。为了与遵循 `CapWords` 函数命名的旧模块保持一致性（*），方法名中可以包含下划线以分隔名称的逻辑组件。一个可能的模式是 `test<MethodUnderTest>_<state>`。

#### 文件命名 

Python 文件名必须使用 `.py` 扩展名，且不得包含破折号（`-`）。这确保它们可以被导入和进行单元测试。如果希望可执行文件无需扩展名即可访问，请使用符号链接或一个包含 `exec "$0.py" "$@"` 的简单 bash 包装脚本。

#### 来自 [Guido](https://en.wikipedia.org/wiki/Guido_van_Rossum) 推荐的指导原则 

| Type                       | Public               | Internal                          |
| -------------------- | ---------------------- | ---------------------- |
| Packages                   | `lower_with_under`   |                                   |
| Modules                    | `lower_with_under`   | `_lower_with_under`               |
| Classes                    | `CapWords`           | `_CapWords`                       |
| Exceptions                 | `CapWords`           |                                   |
| Functions                  | `lower_with_under()` | `_lower_with_under()`             |
| Global/Class Constants     | `CAPS_WITH_UNDER`    | `_CAPS_WITH_UNDER`                |
| Global/Class Variables     | `lower_with_under`   | `_lower_with_under`               |
| Instance Variables         | `lower_with_under`   | `_lower_with_under` (protected)   |
| Method Names               | `lower_with_under()` | `_lower_with_under()` (protected) |
| Function/Method Parameters | `lower_with_under`   |                                   |
| Local Variables            | `lower_with_under`   |                                   |

: Guido's Recommendations {.striped}

#### 数学符号

对于数学计算密集的代码，如果变量名符合参考论文或算法中的既定符号，优先使用短变量名，即使这些名称可能违反风格指南。

在使用基于既定符号的名称时：

- 在注释或文档字符串中引用所有命名约定的来源，最好包含指向学术资源本身的超链接。如果来源不可访问，需清晰记录命名约定。
- 对于公共 API，优先使用符合 PEP 8 的描述性名称（`descriptive_names`），因为这些名称更可能在无上下文的情况下被遇到。
- 使用范围狭窄的 `pylint: disable=invalid-name` 指令来抑制警告。对于仅几个变量的情况，在每个变量的行末使用该指令作为注释；对于更多变量，在代码块开头应用该指令。
