### `TODO` 注释 

对临时代码、短期解决方案或足够好但不完美的代码使用 `TODO` 注释。

`TODO` 注释以全大写的 `TODO` 开头，后跟冒号，以及一个指向包含上下文的资源的链接，最好是一个错误（bug）引用。错误引用更可取，因为错误会被跟踪并有后续评论。在上下文之后，紧跟一个以连字符 `-` 开头的解释性字符串。目的是使用一致的 `TODO` 格式，以便搜索以获取更多细节。

示例：

```python
# TODO: crbug.com/192795 - Investigate cpufreq optimizations.
```

旧风格（以前推荐但不鼓励在新代码中使用）：

```python
# TODO(crbug.com/192795): Investigate cpufreq optimizations.
# TODO(yourusername): Use a "*" here for concatenation operator.
```

避免添加以个人或团队作为上下文的 `TODO`：

```python
# TODO: @yourusername - File an issue and use a '*' for repetition.
```

如果你的 `TODO` 是“在未来某个日期做某事”的形式，确保包含非常具体的日期（例如“2009 年 11 月前修复”）或非常具体的事件（例如“当所有客户端都能处理 XML 响应时移除此代码”），以便未来的代码维护者能够理解。使用问题（issues）来跟踪这些情况是理想的。
