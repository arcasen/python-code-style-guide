---
lang: zh
has-frontmatter: false
top-level-division: section
documentclass: ctexart
---

# Google Python 风格指南

<!-- https://github.com/google/styleguide/blob/gh-pages/pyguide.md -->

<!--

- [1 背景](#s1-background)
- [2 Python语言规则](#s2-python-language-rules)
  * [2.1 代码检查（Lint）](#s2.1-lint)
  * [2.2 导入](#s2.2-imports)
  * [2.3 包](#s2.3-packages)
  * [2.4 异常](#s2.4-exceptions)
  * [2.5 可变全局状态](#s2.5-global-variables)
  * [2.6 嵌套/局部/内部类和函数](#s2.6-nested)
  * [2.7 推导式与生成器表达式](#s2.7-comprehensions)
  * [2.8 默认迭代器和操作符](#s2.8-default-iterators-and-operators)
  * [2.9 生成器](#s2.9-generators)
  * [2.10 Lambda函数](#s2.10-lambda-functions)
  * [2.11 条件表达式](#s2.11-conditional-expressions)
  * [2.12 默认参数值](#s2.12-default-argument-values)
  * [2.13 属性](#s2.13-properties)
  * [2.14 True/False评估](#s2.14-truefalse-evaluations)
  * [2.16 词法作用域](#s2.16-lexical-scoping)
  * [2.17 函数和方法装饰器](#s2.17-function-and-method-decorators)
  * [2.18 线程](#s2.18-threading)
  * [2.19 高级功能](#s2.19-power-features)
  * [2.20 现代Python：来自__future__的导入](#s2.20-modern-python)
  * [2.21 类型注解代码](#s2.21-type-annotated-code)
- [3 Python风格规则](#s3-python-style-rules)
  * [3.1 分号](#s3.1-semicolons)
  * [3.2 行长度](#s3.2-line-length)
  * [3.3 括号](#s3.3-parentheses)
  * [3.4 缩进](#s3.4-indentation)
    + [3.4.1 序列项中的尾随逗号？](#s3.4.1-trailing-commas)
  * [3.5 空行](#s3.5-blank-lines)
  * [3.6 空格](#s3.6-whitespace)
  * [3.7 Shebang行](#s3.7-shebang-line)
  * [3.8 注释和文档字符串](#s3.8-comments-and-docstrings)
    + [3.8.1 文档字符串](#s3.8.1-comments-in-doc-strings)
    + [3.8.2 模块](#s3.8.2-comments-in-modules)
    + [3.8.2.1 测试模块](#s3.8.2.1-test-modules)
    + [3.8.3 函数和方法](#s3.8.3-functions-and-methods)
    + [3.8.3.1 重写方法](#s3.8.3.1-overridden-methods)
    + [3.8.4 类](#s3.8.4-comments-in-classes)
    + [3.8.5 块注释和行内注释](#s3.8.5-block-and-inline-comments)
    + [3.8.6 标点、拼写和语法](#s3.8.6-punctuation-spelling-and-grammar)
  * [3.10 字符串](#s3.10-strings)
    + [3.10.1 日志记录](#s3.10.1-logging)
    + [3.10.2 错误消息](#s3.10.2-error-messages)
  * [3.11 文件、套接字和类似的有状态资源](#s3.11-files-sockets-closeables)
  * [3.12 TODO注释](#s3.12-todo-comments)
  * [3.13 导入格式](#s3.13-imports-formatting)
  * [3.14 语句](#s3.14-statements)
  * [3.15 访问器](#s3.15-accessors)
  * [3.16 命名](#s3.16-naming)
    + [3.16.1 应避免的名称](#s3.16.1-names-to-avoid)
    + [3.16.2 命名约定](#s3.16.2-naming-conventions)
    + [3.16.3 文件命名](#s3.16.3-file-naming)
    + [3.16.4 基于Guido的推荐指南](#s3.16.4-guidelines-derived-from-guidos-recommendations)
  * [3.17 主程序](#s3.17-main)
  * [3.18 函数长度](#s3.18-function-length)
  * [3.19 类型注解](#s3.19-type-annotations)
    + [3.19.1 通用规则](#s3.19.1-general-rules)
    + [3.19.2 换行](#s3.19.2-line-breaking)
    + [3.19.3 前向声明](#s3.19.3-forward-declarations)
    + [3.19.4 默认值](#s3.19.4-default-values)
    + [3.19.5 NoneType](#s3.19.5-nonetype)
    + [3.19.6 类型别名](#s3.19.6-type-aliases)
    + [3.19.7 忽略类型](#s3.19.7-ignoring-types)
    + [3.19.8 类型变量](#s3.19.8-typing-variables)
    + [3.19.9 元组与列表](#s3.19.9-tuples-vs-lists)
    + [3.19.10 类型变量](#s3.19.10-typevars)
    + [3.19.11 字符串类型](#s3.19.11-string-types)
    + [3.19.12 类型导入](#s3.19.12-imports-for-typing)
    + [3.19.13 条件导入](#s3.19.13-conditional-imports)
    + [3.19.14 循环依赖](#s3.19.14-circular-dependencies)
    + [3.19.15 泛型](#s3.19.15-generics)
    + [3.19.16 构建依赖](#s3.19.16-build-dependencies)
- [4 结束语](#4-parting-words)

-->


![[ sections/1-background.md ]]

![[ sections/2-python-language-rules.md ]]

![[ sections/3-python-style-rules.md ]]

![[ sections/4-parting-words.md ]]










