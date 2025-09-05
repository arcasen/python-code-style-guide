### 行长度 

最大行长度为 80 个字符。

例外：

* 长导入模块语句。
* 注释中的 URL、路径名和长标志。
* 长字符串文字（参见[字符串]）。
  - Pylint 禁用注释。（例如：`# pylint: disable=invalid-name`）

不要使用反斜杠来显式续行[^explicit]。

使用 Python 的 `()` `[]` `{}` 来隐式续行[^implicit]。如有必要，您可以用添加额外的 `()` 包裹表达式。

[^explicit]: <https://docs.python.org/3/reference/lexical_analysis.html#explicit-line-joining>

[^implicit]: <https://docs.python.org/3/reference/lexical_analysis.html#implicit-line-joining>

注意：此规则不禁止在字符串中使用反斜杠转义的换行符（见[字符串]）。

正确：

```python
foo_bar(self, width, height, color='black', design=None, x='foo',
    emphasis=None, highlight=0)
```

正确：

```python
if (width == 0 and height == 0 and
    color == 'red' and emphasis == 'strong'):

(bridge_questions.clarification_on
.average_airspeed_of.unladen_swallow) = 'African or European?'

with (
    very_long_first_expression_function() as spam,
    very_long_second_expression_function() as beans,
    third_thing() as eggs,
):
    place_order(eggs, beans, spam, beans)
```

错误：

```python
if width == 0 and height == 0 and \
    color == 'red' and emphasis == 'strong':

bridge_questions.clarification_on \
    .average_airspeed_of.unladen_swallow = 'African or European?'

with very_long_first_expression_function() as spam, \
        very_long_second_expression_function() as beans, \
        third_thing() as eggs:
    place_order(eggs, beans, spam, beans)
```

当字符串字面量无法在一行内适应时，使用括号进行隐式行连接：

```python
x = ('This will build a very long long '
    'long long long long long long string')
```

优先在尽可能高的语法级别断行。如果必须两次断行，两次断行应保持在同一语法级别。

正确：

```python
bridgekeeper.answer(
    name="Arthur", quest=questlib.find(owner="Arthur", perilous=True))

answer = (a_long_line().of_chained_methods()
        .that_eventually_provides().an_answer())

if (
    config is None
    or 'editor.language' not in config
    or config['editor.language'].use_spaces is False
):
    use_tabs()
```

错误：

```python
bridgekeeper.answer(name="Arthur", quest=questlib.find(
    owner="Arthur", perilous=True))

answer = a_long_line().of_chained_methods().that_eventually_provides(
    ).an_answer()

if (config is None or 'editor.language' not in config or config[
        'editor.language'].use_spaces is False):
    use_tabs()
```

在注释中，如有需要，将长 URL 单独放在一行：

正确：

```python
# See details at
# http://www.example.com/us/developer/documentation/api/content/v2.0/csv_file_name_extension_full_specification.html
```

错误：

```python
# See details at
# http://www.example.com/us/developer/documentation/api/content/\
# v2.0/csv_file_name_extension_full_specification.html
```

注意上述行延续示例中元素的缩进；有关缩进的解释，请参见[缩进]部分。

文档字符串（Docstring）摘要行必须保持在 80 个字符的限制内（见[文档字符串]）。

在所有其他情况下，如果一行超过 80 个字符，并且 Black 或 Pyink 自动格式化工具无法将行限制在该范围内，则允许该行超过此最大值。鼓励作者在合理的情况下按照上述说明手动断行。
