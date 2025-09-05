### 字符串 

使用 f-string、`%` 运算符或 `format` 方法来格式化字符串，即使参数都是字符串。使用你的最佳判断来选择字符串格式化选项。使用 `+` 进行单次拼接是可以的，但不要使用 `+` 进行格式化。

```python
Yes: x = f'name: {name}; score: {n}'
     x = '%s, %s!' % (imperative, expletive)
     x = '{}, {}'.format(first, second)
     x = 'name: %s; score: %d' % (name, n)
     x = 'name: %(name)s; score: %(score)d' % {'name':name, 'score':n}
     x = 'name: {}; score: {}'.format(name, n)
     x = a + b
```

```python
No: x = first + ', ' + second
    x = 'name: ' + name + '; score: ' + str(n)
```

避免在循环中使用 `+` 和 `+=` 运算符来累积字符串。在某些情况下，使用加法累积字符串可能导致二次方而不是线性运行时间复杂度。尽管在 CPython 上这种常见的累积操作可能会被优化，但这是一个实现细节。优化适用的条件不易预测且可能会发生变化。相反，应将每个子字符串添加到一个列表中，并在循环结束后使用 `''.join` 方法连接列表，或者将每个子字符串写入 `io.StringIO` 缓冲区。这些技术始终具有摊销线性运行时间复杂度。

```python
Yes: items = ['<table>']
     for last_name, first_name in employee_list:
         items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
     items.append('</table>')
     employee_table = ''.join(items)
```

```python
No: employee_table = '<table>'
    for last_name, first_name in employee_list:
        employee_table += '<tr><td>%s, %s</td></tr>' % (last_name, first_name)
    employee_table += '</table>'
```

在一个文件中保持字符串引号字符的选择一致。选择单引号 `'` 或双引号 `"` 并坚持使用。可以在字符串中使用另一种引号字符，以避免在字符串中对引号字符进行反斜杠转义。

```python
Yes:
  Python('Why are you hiding your eyes?')
  Gollum("I'm scared of lint errors.")
  Narrator('"Good!" thought a happy Python reviewer.')
```

```python
No:
  Python("Why are you hiding your eyes?")
  Gollum('The lint. It burns. It burns us.')
  Gollum("Always the great lint. Watching. Watching.")
```

优先使用 `"""` 来表示多行字符串，而不是 `'''`。项目可以选择对所有非文档字符串的多行字符串使用 `'''`，但前提是它们也对普通字符串使用 `'`。无论如何，文档字符串必须使用 `"""`。


多行字符串不会跟随程序其余部分的缩进。如果需要避免在字符串中嵌入额外的空格，可以使用拼接的单行字符串，或者使用 [`textwrap.dedent()`][^dedent] 处理多行字符串以移除每行的初始空格：

[^dedent]: <https://docs.python.org/3/library/textwrap.html#textwrap.dedent>

```python
No:
  long_string = """This is pretty ugly.
Don't do this.
"""
```

```python
Yes:
  long_string = """This is fine if your use case can accept
      extraneous leading spaces."""
```

```python
Yes:
  long_string = ("And this is fine if you cannot accept\n" +
                 "extraneous leading spaces.")
```

```python
Yes:
  long_string = ("And this too is fine if you cannot accept\n"
                 "extraneous leading spaces.")
```

```python
Yes:
  import textwrap

  long_string = textwrap.dedent("""\
      This is also fine, because textwrap.dedent()
      will collapse common leading spaces in each line.""")
```

请注意，在此处使用反斜杠并不违反禁止显式行延续的规定；在这种情况下，反斜杠是在字符串字面量中转义换行符。

#### 日志记录 

对于期望将模式字符串（包含 %-占位符）作为第一个参数的日志函数：始终使用字符串字面量（而非 f-string！）作为第一个参数，并将模式参数作为后续参数。某些日志实现会收集未展开的模式字符串作为一个可查询字段。这还可以避免花费时间渲染一条没有配置输出目标的日志消息。

```python
Yes:
  import tensorflow as tf
  logger = tf.get_logger()
  logger.info('TensorFlow Version is: %s', tf.__version__)
```

```python
Yes:
  import os
  from absl import logging

  logging.info('Current $PAGER is: %s', os.getenv('PAGER', default=''))

  homedir = os.getenv('HOME')
  if homedir is None or not os.access(homedir, os.W_OK):
    logging.error('Cannot write to home directory, $HOME=%r', homedir)
```

```python
No:
  import os
  from absl import logging

  logging.info('Current $PAGER is:')
  logging.info(os.getenv('PAGER', default=''))

  homedir = os.getenv('HOME')
  if homedir is None or not os.access(homedir, os.W_OK):
    logging.error(f'Cannot write to home directory, $HOME={homedir!r}')
```

#### 错误消息

错误消息（例如：ValueError 等异常中的消息字符串，或显示给用户的消息）应遵循以下三条准则：

1. 消息需要精确匹配实际的错误条件。
2. 插值部分需要始终清晰可辨。
3. 应便于简单的自动化处理（例如通过 grep 查找）。

```python
  Yes:
  if not 0 <= p <= 1:
    raise ValueError(f'Not a probability: {p=}')

  try:
    os.rmdir(workdir)
  except OSError as error:
    logging.warning('Could not remove directory (reason: %r): %r',
                    error, workdir)
```

```python
No:
  if p < 0 or p > 1:  # PROBLEM: also false for float('nan')!
    raise ValueError(f'Not a probability: {p=}')

  try:
    os.rmdir(workdir)
  except OSError:
    # PROBLEM: Message makes an assumption that might not be true:
    # Deletion might have failed for some other reason, misleading
    # whoever has to debug this.
    logging.warning('Directory already was deleted: %s', workdir)

  try:
    os.rmdir(workdir)
  except OSError:
    # PROBLEM: The message is harder to grep for than necessary, and
    # not universally non-confusing for all possible values of `workdir`.
    # Imagine someone calling a library function with such code
    # using a name such as workdir = 'deleted'. The warning would read:
    # "The deleted directory could not be deleted."
    logging.warning('The %s directory could not be deleted.', workdir)
```