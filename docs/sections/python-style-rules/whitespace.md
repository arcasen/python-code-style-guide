### 空格 

遵循标点符号周围空格使用的标准排版规则。

括号、方括号或大括号内部不加空格。

正确：

```python
spam(ham[1], {'eggs': 2}, [])
```

错误：

```python
spam( ham[ 1 ], { 'eggs': 2 }, [ ] )
```

在逗号、分号或冒号之前不加空格。在逗号、分号或冒号之后要加空格，除非是在行末。

正确：

```python
if x == 4:
    print(x, y)
x, y = y, x
```
错误：

```python
if x == 4 :
    print(x , y)
x , y = y , x
```

在开始参数列表、索引或切片的开括号/方括号之前不加空格。

正确：

```python
spam(1)
```

错误：

```python
spam (1)
```

正确：

```python
dict['key'] = list[index]
```

错误：

```python
dict ['key'] = list [index]
```

不要有尾随空格。

在赋值（`=`）、比较（`==`, `<`, `>`, `!=`, `<>`, `<=`, `>=`, `in`, `not in`, `is`, `is not`）和布尔运算（`and`, `or`, `not`）的二元运算符两侧各加一个空格。对于算术运算符（`+`, `-`, `*`, `/`, `//`, `%`, `**`, `@`）周围空格的插入，使用你的最佳判断。

正确：

```python
x == 1
```

错误：

```python
x<1
```

当 '`=`' 用于指示关键字参数或默认参数值时，如果存在类型注解，请不要在其周围使用空格。

正确：

```python
def complex(real, imag=0.0): return Magic(r=real, i=imag)
def complex(real, imag: float = 0.0): return Magic(r=real, i=imag)
```

错误：

```python
def complex(real, imag = 0.0): return Magic(r = real, i = imag)
def complex(real, imag: float=0.0): return Magic(r = real, i = imag)
```

不要使用空格来垂直对齐多行标记，因为它会成为维护负担（适用于 `:`, `#`, `=` 等）：

正确：

```python
foo = 1000  # comment
long_name = 2  # comment that should not be aligned

dictionary = {
    'foo': 1,
    'long_name': 2,
}
```

错误：

```python
foo       = 1000  # comment
long_name = 2     # comment that should not be aligned

dictionary = {
    'foo'      : 1,
    'long_name': 2,
}
```
