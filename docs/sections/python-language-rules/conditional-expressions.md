### 条件表达式 

对于简单情况可以使用。

#### 定义 

条件表达式（有时称为“三元运算符”）是为 if 语句提供更短语法的机制。例如：`x = 1 if cond else 2`。

#### 优点 

比 `if` 语句更短、更方便。

#### 缺点 

可能比 `if` 语句更难阅读。如果表达式很长，条件可能难以定位。

#### 决定 

对于简单情况可以使用。每部分必须在一行上：true-expression, if-expression, else-expression。当事情变得更复杂时，使用完整的 `if` 语句。

正确：

```python
one_line = 'yes' if predicate(value) else 'no'
slightly_split = ('yes' if predicate(value)
                    else 'no, nein, nyet')
the_longest_ternary_style_that_can_be_done = (
    'yes, true, affirmative, confirmed, correct'
    if predicate(value)
    else 'no, false, negative, nay')
```

错误：

```python
bad_line_breaking = ('yes' if predicate(value) else
                        'no')
portion_too_long = ('yes'
                    if some_long_module.some_long_predicate_function(
                        really_long_variable_name)
                    else 'no, false, negative, nay')
```
