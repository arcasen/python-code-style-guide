### 缩进 

使用 4 个空格缩进代码块。

永远不要使用制表符。隐式行延续应使换行的元素垂直对齐（参见[行长度]示例），或使用4个空格的悬挂缩进。关闭的（圆形、方形或花括号）括号可以放在表达式的末尾，或者放在单独的行上，但如果是单独的行，则应与对应的开括号所在行缩进相同。

正确：

```python
# Aligned with opening delimiter.
foo = long_function_name(var_one, var_two,
                         var_three, var_four)
meal = (spam,
        beans)

# Aligned with opening delimiter in a dictionary.
foo = {
    'long_dictionary_key': value1 +
                           value2,
    ...
}

# 4-space hanging indent; nothing on first line.
foo = long_function_name(
    var_one, var_two, var_three,
    var_four)
meal = (
    spam,
    beans)

# 4-space hanging indent; nothing on first line,
# closing parenthesis on a new line.
foo = long_function_name(
    var_one, var_two, var_three,
    var_four
)
meal = (
    spam,
    beans,
)

# 4-space hanging indent in a dictionary.
foo = {
    'long_dictionary_key':
        long_dictionary_value,
    ...
}
```

错误：

```python
# Stuff on first line forbidden.
foo = long_function_name(var_one, var_two,
    var_three, var_four)
meal = (spam,
    beans)

# 2-space hanging indent forbidden.
foo = long_function_name(
  var_one, var_two, var_three,
  var_four)

# No hanging indent in a dictionary.
foo = {
    'long_dictionary_key':
    long_dictionary_value,
    ...
}
```

#### 序列项中的尾随逗号？ 

仅当序列中的结束容器标记 `]`、`)` 或 `}` 未与最后一个元素出现在同一行时，或者对于只有一个元素的元组时，才推荐使用尾随逗号。尾随逗号的存在也被用作提示，指导我们的 Python 代码自动格式化工具 Black 或 Pyink 在最后一个元素后存在逗号时，将容器中的项自动格式化为每行一项。

正确：

```python
golomb3 = [0, 1, 3]
golomb4 = [
    0,
    1,
    4,
    6,
]
```

错误：

```python
golomb4 = [
    0,
    1,
    4,
    6,]
```
