### Shebang 行 

大多数 .py 文件不需要以 #! 行开头。程序的主文件应以 `#!/usr/bin/env python3`（以支持虚拟环境）或 `#!/usr/bin/python3` 开头，遵循 PEP-394[^pep394]。

[^pep394]: <https://peps.python.org/pep-0394/>

这一行由内核用来查找 Python 解释器，但在导入模块时会被 Python 忽略。只有在打算直接执行的文件上才需要这一行。
