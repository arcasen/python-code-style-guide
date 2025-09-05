### 括号 

谨慎使用括号。

不要在返回语句或条件语句中使用它们，除非使用括号来实现行续接或指示优先级。例外情况是元组语法（`return x, y`），其中括号是必需的；和 f-字符串，其中括号用于格式化或多行字符串。

正确：

```python
if foo:
    bar()
while x:
    x = bar()
if x and y:
    bar()
if not x:
    bar()
# For a 1 item tuple the ()s are more visually obvious than the comma.
onesie = (foo,)
return foo
return spam, beans
return (spam, beans)
for (x, y) in dict.items(): ...
```

错误：

```python
if (x):
    bar()
if not(x):
    bar()
return (foo)
```