### 文件、套接字和类似的有状态资源 

在使用完文件和套接字后，显式地关闭它们。这一规则自然扩展到内部使用套接字的可关闭资源，例如数据库连接，以及其他需要类似方式关闭的资源。例如，这包括 mmap 映射[^mmap]、h5py 文件对象[^h5py]和 matplotlib.pyplot 图形窗口[^plt]等。

[^mmap]: <https://docs.python.org/3/library/mmap.html>
[^h5py]: <https://docs.h5py.org/en/stable/high/file.html>
[^plt]: <https://matplotlib.org/2.1.0/api/_as_gen/matplotlib.pyplot.close.html>

不必要地保持文件、套接字或其他此类有状态对象的开启状态有许多缺点：

- 它们可能消耗有限的系统资源，例如文件描述符。处理大量此类对象的代码如果在使用后不及时将资源归还系统，可能会不必要地耗尽这些资源。
- 保持文件开启可能会阻止其他操作，例如移动或删除文件，或者卸载文件系统。
- 在整个程序中共享的文件和套接字可能在逻辑上关闭后被意外读取或写入。如果它们被实际关闭，尝试读取或写入将引发异常，从而更快地暴露问题。

此外，尽管文件、套接字（以及一些行为类似的其他资源）在对象销毁时会自动关闭，但将对象的生命周期与资源的状态绑定是一种不好的做法：

- 无法保证运行时会在何时实际调用 `__del__` 方法。不同的 Python 实现使用不同的内存管理技术，例如延迟垃圾回收，可能会任意且无限期地延长对象的生命周期。
- 对文件的意外引用（例如在全局变量或异常跟踪中）可能使其存活时间超出预期。
- 依赖终结器（finalizers）进行具有可观察副作用的自动清理已被多次证明会导致重大问题，这种问题跨越了数十年和多种语言（参见关于 Java 的文章[^java]）。

[^java]: <https://wiki.sei.cmu.edu/confluence/display/java/MET12-J.+Do+not+use+finalizers>

管理文件和类似资源的首选方式是使用 `with` 语句[^with-stmt]：

[^with-stmt]: <http://docs.python.org/reference/compound_stmts.html#the-with-statement>

```python
with open("hello.txt") as hello_file:
    for line in hello_file:
        print(line)
```

对于不支持 `with` 语句的类文件对象，使用 `contextlib.closing()`：

```python
import contextlib

with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
    for line in front_page:
        print(line)
```

在极少数情况下，如果基于上下文的资源管理不可行，代码文档必须清楚地解释资源生命周期是如何管理的。
