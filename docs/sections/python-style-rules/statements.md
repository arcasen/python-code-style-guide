### 语句 

通常每行只有一个语句。

然而，只有当整个语句适合放在一行时，你才可以将测试结果与测试放在同一行。特别是，`try/except` 语句永远不能这样做，因为 `try` 和 `except` 无法同时放在一行；对于 `if` 语句，只有在没有 `else` 的情况下才可以这样做。

正确：

```python
  if foo: bar(foo)
```

错误：

```python
if foo: bar(foo)
else:   baz(foo)

try:               bar(foo)
except ValueError: baz(foo)

try:
    bar(foo)
except ValueError: baz(foo)
```
