### main 函数 

在 Python 中，`pydoc` 和单元测试要求模块是可导入的。如果一个文件旨在作为可执行文件使用，其主要功能应放在 `main()` 函数中，并且你的代码应始终在执行主程序前检查 `if __name__ == '__main__'`，以确保模块被导入时不会执行主程序。

当使用 `absl` 时，使用 `app.run`：

```python
from absl import app
...

def main(argv: Sequence[str]):
    # process non-flag arguments
    ...

if __name__ == '__main__':
    app.run(main)
```

否则，使用以下结构：

```python
def main():
    ...

if __name__ == '__main__':
    main()
```

模块顶级的代码在模块被导入时会被执行。注意不要在文件被 `pydoc` 处理时调用函数、创建对象或执行其他不应执行的操作。
