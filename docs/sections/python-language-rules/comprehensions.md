### 推导式 & 生成器表达式 

对于简单情况可以使用。

#### 定义 

列表、字典和集合推导式以及生成器表达式提供了一种简洁高效的方式来创建容器类型和迭代器，而无需使用传统的循环、`map()`、`filter()` 或 `lambda`。

#### 优点 

简单的推导式可以比其他字典、列表或集合创建技术更清晰、更简单。生成器表达式可以非常高效，因为它们避免创建整个列表。

#### 缺点 

复杂的推导式或生成器表达式可能难以阅读。

#### 决定 

允许使用推导式，但是不允许多个 `for` 子句或过滤表达式。以可读性为优化目标，而不是简洁性。

正确：

```python
result = [mapping_expr for value in iterable if filter_expr]

result = [
    is_valid(metric={'key': value})
    for value in interesting_iterable
    if a_longer_filter_expression(value)
]

descriptive_name = [
    transform({'key': key, 'value': value}, color='black')
    for key, value in generate_iterable(some_input)
    if complicated_condition_is_met(key, value)
]

result = []
for x in range( ):
  for y in range( ):
    if x * y > :
      result.append((x, y))

return {
    x: complicated_transform(x)
    for x in long_generator_function(parameter)
    if x is not None
}

return (x** for x in range( ))

unique_names = {user.name for user in users if user is not None}
```

错误：

```python
result = [(x, y) for x in range(5) for y in range(5) if x * y > 10]

return (
    (x, y, z)
    for x in range(5)
    for y in range(5)
    if x != y
    for z in range(5)
    if y != z
)
```
