# Python 教程（三）--使用Python解释器

## 原文链接

[2. Using the Python Interpreter](https://docs.python.org/3.6/tutorial/interpreter.html)

## 原文翻译

<h3 id='2.'>2. 使用Python解释器</h3>

<h4 id='2.1.'>2.1. 唤醒解释器</h4>

Python解释器通常安装在存在`/usr/local/bin`路径的机器中的`/usr/local/bin/python3.6`路径；
Unix shell的环境变量PATH添加路径`/usr/local/bin`使得在shell中输入一下指令来启动Python解释器：

    python3.6

由于选择解释器所在的目录是一个安装选项，其他存储路径也是可选的。请咨询您本地的Python管理器或者系统管理员来确定Python的安装位置。（例如，`/usr/local/python`是一个受欢迎的替代位置。）

在Windows机器上， Python默认安装在`C:\Python36`路径，但是您可以在Python安装器中改变存储路径。为了将此目录添加到环境变量path中，你可以在Dos命令行中输入一下命令：

    set path=%path%;C:\python36

**在Python主提示符下键入文件结束符（EOF）（Unix上是`Control-D`，Windows上是`Control-Z`）会导致解释器以0退出状态退出。如果这不起作用，您可以输入以下命令退出解释器：`quit()`。**

在支持readline的系统上，解释器的行编辑特点包括交互式编辑interactive editing，历史替换history substitution和代码补全code completion。**检查是否支持命令行编辑的最快检查方法就是在Python解释器的第一个提示符下键入`Controal-P`；如果发出蜂鸣声，则支持命令行编辑；有关键的介绍，请参考附录[Interactive Input Editing and History Substitution](https://docs.python.org/3.6/tutorial/interactive.html#tut-interacting)。如果没有任何事情发生或者`^P`回显，则不支持命令行编辑。你只能使用退格键从当前行中删除字符。

Python解释器的操作有点像Unix Shell：当使用连接到tty设备的标准输入调用时，它以交互方式读取和执行命令；当使用文件名参数或文件作为标准输入调用时，它会从该文件中读取并执行脚本。

第二种启动解释器的方法是`python -c command [arg] ...`，执行command参数中的语句，类似于shell中的-c选项。由于Python语句中经常包含空格或者其他shell中的特殊字符，因此通常建议使用单引号括住command参数。

一些Python模块也可用作脚本。可以使用`python -m module [arg] ...`来调用解释器，此时执行模块的源文件，就像在命令行中拼写出它的全名一样。

使用脚本文件时，有时可以运行脚本并在之后进入交互模式。这可以通过-i 在脚本之前传递来完成。

[Command line and environment](https://docs.python.org/3.6/using/cmdline.html#using-on-general)中描述了所有命令行选项。

<h4 id='2.1.1.'>2.1.1. 参数传递</h4>

向解释器传递参数时，脚本文件名和其后的附加参数将变成元素是字符串的列表，并赋值给`sys`模块的`argv`变量。你可以通过执行`import sys`来获取这个参数列表。参数列表的长度至少是1；当没有给出脚本文件名和参数时，`sys.argv[0]`是一个空字符串。当给出的文件名是`'-'`（意味着标准输入），`sys.argv[0]`被设置成`'-'`。当使用`-c command`时，`sys.argv[0]`被设置成`'-c'`。当使用`-m module`时，`sys.argv[0]`被设置成模块的绝对路径+文件名。`-c command`和`-m module`后的参数不会被Python解释器处理而是存储在`sys.argv`中，以进一步供命令或者模块处理。

<h4 id='2.1.2.'>2.1.2. 交互模式</h4>

当从tty读取命令时，解释器被称为处于交互模式。在这种模式下，它会使用主提示符（通常是三个大于号`>>>`）来提示下一命令; 它会使用次提示符(默认为三个点`...`）来提示连续行。在打印第一个提示符之前，解释程序会打印一条欢迎消息，说明其版本号和版权声明：

    $ python3.6
    Python 3.6 (default, Sep 16 2015,09:25:04)
    [GCC 4.8.2] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

进入多行结构时需要连续行。举个例子，看看这个`if`语句：

    >>> the_world_is_flat = True
    >>> if the_world_is_flat:
    ...     print("Be careful not to fall off!")
    ...
    Be careful not to fall off!

有关交互模式的更多信息，请参考[交互模式](https://docs.python.org/3.6/tutorial/appendix.html#tut-interac)

<h4 id='2.2.'>2.2. 解释器及其环境</h4>

<h4 id='2.2.1.'>2.2.1. 源代码编码</h4>

默认情况下，Python源文件被视为以UTF-8编码。在该编码中，世界上大多数语言的字符可以在字符串文字，标识符和注释中同时使用 - 尽管标准库仅使用ASCII字符作为标识符，这是任何可移植代码都应遵循的约定。要正确显示所有这些字符，编辑器必须识别文件是UTF-8，并且必须使用支持文件中所有字符的字体。

要声明非默认编码，应添加一个特殊注释行作为文件的第一行。语法如下：

    # -*- coding: encoding -*-

其中encoding是[codecs](https://docs.python.org/3.6/library/codecs.html#module-codecs)Python支持的有效编码之一。

例如，要声明要使用Windows-1252编码，源代码文件的第一行应为：

    # -*- coding: cp1252 -*-

第一行规则的一个例外是源代码以[UNIX“shebang”](https://docs.python.org/3.6/tutorial/appendix.html#tut-scripts)行开头 。在这种情况下，应将编码声明添加为文件的第二行。例如：

    #!/usr/bin/env python3
    # -*- coding: cp1252 -*-

## 个人笔记

1. Python解释器主提示符下键入文件结束符（Unix下`Control-D`，Windows下`Control-Z`）或者`quit()`来退出Python解释器;
2. `python -m module [arg] ...`执行模块的源文件,等价于`python module.py [arg] ...`
3. `python -c 'command' [arg] ...`，执行command参数中的语句;
4. 向解释器传递参数时，脚本文件名和其后的附加参数将变成字符串列表，并赋值给`sys`模块的`argv`变量。`sys.argv`的长度至少是1；
    + 当没有传递参数时，`sys.argv[0]`是空字符串`''`；
    + 当使用`-c command`时，`sys.argv[0]`被设置成`'-c'`。
    + 当使用`-m module`时，`sys.argv[0]`被设置成模块的绝对路径+文件名。
    + `-c command`和`-m module`后的参数不会被Python解释器处理而是存储在`sys.argv`中，以进一步供命令或者模块处理。

5. 在Python源码第一行加入Unix Shebang起始行，可以将源码转换成可执行文件；第二行加入`# -*- coding: 编码方式 -*-`可以设置源码的编码方式。