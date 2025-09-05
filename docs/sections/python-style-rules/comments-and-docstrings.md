### 注释和文档字符串 

确保您的代码有适当的注释和文档字符串。

#### 文档字符串 

Python 使用文档字符串（docstrings）来记录代码。文档字符串是包、模块、类或函数中的第一个语句。这些字符串可以通过对象的 `__doc__` 属性自动提取，并被 pydoc 使用。（可以尝试对你的模块运行 pydoc 来查看其效果。）始终使用三个双引号 `"""` 格式来编写文档字符串（根据 PEP 257[^pep257]）。文档字符串应组织为一个摘要行（一行不超过 80 个字符），以句号、问号或感叹号结束。如果需要编写更多内容（鼓励这样做），摘要行后必须跟一个空行，然后是文档字符串的其余部分，从与首行第一个引号相同的游标位置开始。以下还有更多关于文档字符串的格式指南。

[^pep257]: <https://peps.python.org/pep-0257/>


#### 模块

每个文件都应包含许可证样板代码。选择项目使用的许可证对应的适当样板代码（例如，Apache 2.0、BSD、LGPL、GPL）。

文件应以描述模块内容和用法的文档字符串开头。

```python
"""A one-line summary of the module or program, terminated by a period.

Leave one blank line.  The rest of this docstring should contain an
overall description of the module or program.  Optionally, it may also
contain a brief description of exported classes and functions and/or usage
examples.

Typical usage example:

  foo = ClassFoo()
  bar = foo.function_bar()
"""
```

##### 测试模块

测试文件的模块级文档字符串不是必需的。仅当可以提供额外信息时才应包含文档字符串。

示例包括：有关如何运行测试的具体细节、非常规设置模式的解释、对外部环境的依赖性等。

```python
"""This blaze test uses golden files.

You can update those files by running
`blaze run //foo/bar:foo_test -- --update_golden_files` from the `google3`
directory.
"""
```

不要使用没有提供任何新信息的文档字符串。

```python
"""Tests for foo.bar."""
```

#### 函数与方法

在本节中，“函数”指的是方法、函数、生成器或属性。

每个具有以下一个或多个特征的函数都必须包含文档字符串：

- 属于公共 API
- 非琐碎的规模
- 非显而易见的逻辑

文档字符串应提供足够的信息，以便在不阅读函数代码的情况下能够编写对该函数的调用。文档字符串应描述函数的调用语法及其语义，但通常不包括实现细节，除非这些细节与函数的使用方式相关。例如，如果函数作为副作用修改了其某个参数，应在文档字符串中注明。否则，与调用者无关的函数实现的微妙但重要的细节应作为代码旁边的注释，而不是在函数的文档字符串中表达。

文档字符串可以是描述性风格（`"""Fetches rows from a Bigtable."""`）或命令式风格（`"""Fetch rows from a Bigtable."""`），但在同一文件中风格应保持一致。`@property` 数据描述符的文档字符串应使用与属性或函数参数相同的风格（`"""The Bigtable path."""`，而不是 `"""Returns the Bigtable path."""`）。

函数的某些方面应在以下特定部分中记录。每个部分以一个标题行开始，标题行以冒号结尾。除了标题行外，所有部分应保持2或4个空格的悬挂缩进（在文件中保持一致）。如果函数的名称和签名足够清晰，可以通过一行文档字符串充分描述，则可以省略这些部分。

Args
: 按名称列出每个参数。参数名称后应跟一个描述，描述与名称之间用冒号加空格或换行符分隔。如果描述过长，无法在一行80个字符内适应，则使用比参数名称多2或4个空格的悬挂缩进（与文件中其他文档字符串保持一致）。如果代码中没有相应的类型注解，描述中应包含所需的类型信息。如果函数接受 `*foo`（可变长度参数列表）和/或 `**bar`（任意关键字参数），应将其列为 `*foo` 和 `**bar`。

Returns（或 Yields：对于生成器）
: 描述返回值（或生成器 yield 的值）的语义，包括类型注解未提供的任何类型信息。如果函数仅返回 None，则不需要此部分。如果文档字符串以“Return”、“Returns”、“Yield”或“Yields”开头（例如 `"""Returns row from Bigtable as a tuple of strings."""`），且开头的句子足以描述返回值，则也可以省略此部分。不要模仿较旧的“NumPy 风格”（示例），这种风格常将元组返回值描述为多个单独命名的返回值（从未提及元组）。相反，应描述为：“Returns: A tuple (mat_a, mat_b), where mat_a is …, and …”。文档字符串中的辅助名称不必与函数体中使用的任何内部名称对应（因为这些不是 API 的一部分）。如果函数使用 yield（是生成器），Yields: 部分应记录 next() 返回的对象，而不是调用时评估的生成器对象本身。

Raises
: 列出与接口相关的所有异常，并附上描述。使用与 Args: 中描述的类似异常名称 + 冒号 + 空格或换行符及悬挂缩进的风格。你不应记录因违反文档字符串中指定的 API 而引发的异常（因为这会矛盾地使违反 API 的行为成为 API 的一部分）。

```python{.numberLines}
def fetch_smalltable_rows(
    table_handle: smalltable.Table,
    keys: Sequence[bytes | str],
    require_all_keys: bool = False,
) -> Mapping[bytes, tuple[str, ...]]:
    """Fetches rows from a Smalltable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by table_handle.  String keys will be UTF-8 encoded.

    Args:
        table_handle: An open smalltable.Table instance.
        keys: A sequence of strings representing the key of each table
          row to fetch.  String keys will be UTF-8 encoded.
        require_all_keys: If True only rows with values set for all keys will be
          returned.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {b'Serak': ('Rigel VII', 'Preparer'),
         b'Zim': ('Irk', 'Invader'),
         b'Lrrr': ('Omicron Persei 8', 'Emperor')}

        Returned keys are always bytes.  If a key from the keys argument is
        missing from the dictionary, then that row was not found in the
        table (and require_all_keys must have been False).

    Raises:
        IOError: An error occurred accessing the smalltable.
    """
```

类似地，`Args:` 部分的这种带换行符的变体也是允许的：

```python{.numberLines}
def fetch_smalltable_rows(
    table_handle: smalltable.Table,
    keys: Sequence[bytes | str],
    require_all_keys: bool = False,
) -> Mapping[bytes, tuple[str, ...]]:
    """Fetches rows from a Smalltable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by table_handle.  String keys will be UTF-8 encoded.

    Args:
      table_handle:
        An open smalltable.Table instance.
      keys:
        A sequence of strings representing the key of each table row to
        fetch.  String keys will be UTF-8 encoded.
      require_all_keys:
        If True only rows with values set for all keys will be returned.

    Returns:
      A dict mapping keys to the corresponding table row data
      fetched. Each row is represented as a tuple of strings. For
      example:

      {b'Serak': ('Rigel VII', 'Preparer'),
       b'Zim': ('Irk', 'Invader'),
       b'Lrrr': ('Omicron Persei 8', 'Emperor')}

      Returned keys are always bytes.  If a key from the keys argument is
      missing from the dictionary, then that row was not found in the
      table (and require_all_keys must have been False).

    Raises:
      IOError: An error occurred accessing the smalltable.
    """
```

##### 重写方法

如果一个方法明确使用 [`@override`][^override] 装饰器（来自 `typing_extensions` 或 `typing` 模块）重写了基类中的方法，则不需要文档字符串，除非重写方法的行为实质上细化了基方法的契约，或者需要提供额外细节（例如，记录额外的副作用）。在这种情况下，重写方法的文档字符串至少需要描述这些差异。

[^override]: <https://typing-extensions.readthedocs.io/en/latest/#override>

```python
from typing_extensions import override

class Parent:
  def do_something(self):
    """Parent method, includes docstring."""

# Child class, method annotated with override.
class Child(Parent):
  @override
  def do_something(self):
    pass
```

```python
# Child class, but without @override decorator, a docstring is required.
class Child(Parent):
  def do_something(self):
    pass

# Docstring is trivial, @override is sufficient to indicate that docs can be
# found in the base class.
class Child(Parent):
  @override
  def do_something(self):
    """See base class."""
```

#### 类 

类定义下方应有一个文档字符串，用于描述该类。公共属性（不包括属性装饰器，见[属性]）应在此处的 `Attributes` 部分中记录，并遵循与函数 `Args` 部分相同的格式。

```python{.numberLines}
class SampleClass:
    """Summary of class here.

    Longer class information...
    Longer class information...

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam: bool = False):
        """Initializes the instance based on spam preference.

        Args:
          likes_spam: Defines if instance exhibits this preference.
        """
        self.likes_spam = likes_spam
        self.eggs = 0

    @property
    def butter_sticks(self) -> int:
        """The number of butter sticks we have."""
```

所有类的文档字符串应以一行摘要开头，描述类实例代表的内容。这意味着异常的子类也应描述异常代表的内容，而不是其可能发生的上下文。类的文档字符串不应重复不必要的信息，例如说明该类是一个类。

正确：

```python
class CheeseShopAddress:
  """The address of a cheese shop.

  ...
  """

class OutOfCheeseError(Exception):
  """No more cheese is available."""
```

错误：

```python
class CheeseShopAddress:
  """Class that describes the address of a cheese shop.

  ...
  """

class OutOfCheeseError(Exception):
  """Raised when no more cheese is available."""
```

#### 块注释和行内注释 

代码中复杂部分是添加注释的最后地方。如果你在下次[代码审查][^code-review]时需要解释某段代码，现在就应该为它添加注释。复杂的操作在开始前应有几行注释。非显而易见的代码应在行末添加注释。

[^code-review]: <http://en.wikipedia.org/wiki/Code_review>

```python
# We use a weighted dictionary search to find out where i is in
# the array.  We extrapolate position based on the largest num
# in the array and the array size and then do binary search to
# get the exact number.

if i & (i-1) == 0:  # True if i is 0 or a power of 2.
```

为了提高可读性，这些注释应至少与代码间隔2个空格，并以注释字符 `#` 开始，之后至少有一个空格，然后才是注释文本本身。

另一方面，永远不要描述代码本身。假设阅读代码的人比你更了解 Python（尽管他们不知道你试图做什么）。

```python
# BAD COMMENT: Now go through the b array and make sure whenever i occurs
# the next element is i+1
```

#### 标点、拼写和语法 

注意标点、拼写和语法；编写良好的注释比编写糟糕的注释更容易阅读。

注释应像叙述性文本一样易于阅读，使用正确的首字母大写和标点。在许多情况下，完整的句子比句子片段更具可读性。较短的注释，例如代码行末尾的注释，有时可以不太正式，但你应保持风格的一致性。

尽管代码审查者指出你使用了逗号而应使用分号可能会让人感到沮丧，但源代码保持高度清晰度和可读性非常重要。正确的标点、拼写和语法有助于实现这一目标。

