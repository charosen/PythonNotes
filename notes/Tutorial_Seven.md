# Python 教程(七） -- 模块

## 原文链接

[6. Modules](https://docs.python.org/3.6/tutorial/modules.html)

## 原文翻译

<h3 id='6.'>6. 模块</h3>

如果退出Python解释器并再次输入，则所做的定义（函数和变量）将丢失。因此，如果您想编写一个稍长的程序，最好使用文本编辑器为解释器准备输入并将该文件作为输入运行，这称为创建脚本。随着程序变得越来越长，您可能希望将其拆分为多个文件以便于维护。您可能还想在无需将函数定义复制到每个程序的情况下，在多个程序中使用您所编写的函数。

为了支持这一点，Python提供一种方法：可以将定义放在一个文件中，并在脚本形式运行这个文件或交互式解释器中使用它们。这样的文件称为 *模块* ；模块中的定义可以导入到其他模块或主模块中（在顶级和计算器模式下执行的脚本中可以访问的变量集合the collection of variables that you have access to in a script executed at the top level and in calculator mode）。

模块是包含Python定义和语句的文件。文件名即模块名附加后缀`.py`。在模块中，可以使用全局变量 `__name__`的值来获取模块名称（是一个字符串）。例如，在当前目录中使用您喜欢的文本编辑器创建一个名为`fibo.py`的文件，其中包含以下内容：

    # Fibonacci numbers module

    def fib(n):    # write Fibonacci series up to n
        a, b = 0, 1
        while b < n:
            print(b, end=' ')
            a, b = b, a+b
        print()

    def fib2(n):   # return Fibonacci series up to n
        result = []
        a, b = 0, 1
        while b < n:
            result.append(b)
            a, b = b, a+b
        return result

现在进入Python解释器并使用以下命令导入此模块：

    >>> import fibo
    
这不会将`fibo`中定义的函数名称引入到当前符号表；它只引入模块名`fibo`到当前符号表。使用模块名可以访问这些函数：

    >>> fibo.fib(1000)
    1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
    >>> fibo.fib2(100)
    [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
    >>> fibo.__name__
    'fibo'

对于经常使用的模块变量，请将其赋值给局部变量：

    >>> fib = fibo.fib
    >>> fib(500)
    1 1 2 3 5 8 13 21 34 55 89 144 233 377

<h4 id='6.1.'>6.1. 更多模块信息</h4>

模块除了可以包含函数定义之外，还可以包含一些可执行语句。这些可执行语句用于初始化模块。它们仅在import语句中第一次遇到对应模块名时执行。（如果模块作为脚本运行，它们也会执行。）

每个模块都有自己的私有符号表，该表用作模块中定义的所有函数的全局符号表。因此，模块的作者可以在模块中使用全局变量，而不必担心与用户的全局变量的意外冲突。另一方面，如果您知道自己在做什么，则可以使用与引用模块函数相同的表达式`modname.itemname`来引用模块的全局变量。

模块可以导入其他模块。建议但不强制要求将所有 [import](https://docs.python.org/3.6/reference/simple_stmts.html#import)语句放在模块（或脚本）的开头。导入的模块名称放在被导入模块的全局符号表中。

存在另一种形式的[import](https://docs.python.org/3.6/reference/simple_stmts.html#import)语句，可以将模块中的名称直接导入到被导入模块的符号表。例如：

    >>> from fibo import fib, fib2
    >>> fib(500)
    1 1 2 3 5 8 13 21 34 55 89 144 233 377
    
这不会从本地符号表引入模块名称（因此在示例中，`fibo`未定义)。

还存在另外一种形式的[import](https://docs.python.org/3.6/reference/simple_stmts.html#import)语句，可以导入模块中定义的所有名称：

    >>> from fibo import *
    >>> fib(500)
    1 1 2 3 5 8 13 21 34 55 89 144 233 377
    
这将导入除了以下划线（`_`）开头的名称以外的所有名称。在大多数情况下，Python程序员不使用此工具，因为它在解释器中引入了一组未知的名称，这可能会覆盖或隐藏了您已定义的一些内容。

请注意，通常从模块或包导入所有名称的做法是不受欢迎的，因为它经常导致代码可读性差。但是，可以使用它来保存交互式会话中的输入。

如果模块名后跟[as](https://docs.python.org/3.6/reference/compound_stmts.html#as)，则[as](https://docs.python.org/3.6/reference/compound_stmts.html#as)之后的名称将直接绑定到被导入的模块名。

    >>> import fibo as fib
    >>> fib.fib(500)
    0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
    
这实际上是以与`import fibo`相同的方式高效地导入模块，唯一的不同在于模块将通过`fib`来访问。

在使用[from](https://docs.python.org/3.6/reference/simple_stmts.html#from)时也可以使用[as](https://docs.python.org/3.6/reference/compound_stmts.html#as)，则[as](https://docs.python.org/3.6/reference/compound_stmts.html#as)，它会产生同样的效果：

    >>> from fibo import fib as fibonacci
    >>> fibonacci(500)
    0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
    
**注意**：出于效率原因，每个模块仅在每个解释器会话中导入一次。因此，如果修改了模块，则必须重新启动解释器并重新导入，或者，如果只是对一个模块进行交互式测试，请使用[importlib.reload()](https://docs.python.org/3.6/library/importlib.html#importlib.reload)，例如。

    import importlib
    importlib.reload(modulename)

<h4 id='6.1.1.'>6.1.1. 将模块作为脚本运行</h4>

当你使用一下语句运行Python模块时

    python fibo.py <arguments>

将执行模块中的代码，就像您导入它一样，但`__name__`的值设置为`"__main__"`。这意味着通过在模块的末尾添加如下代码：

    if __name__ == "__main__":
        import sys
        fib(int(sys.argv[1]))

您可以使该文件可用作脚本以及可导入模块，因为解析命令行的代码仅在模块作为“main”文件执行时才会运行:

    $ python fibo.py 50
    1 1 2 3 5 8 13 21 34

如果是导入模块，则不运行代码：

    >>> import fibo
    >>>

这通常用于为模块提供方便的用户界面，或用于测试目的（将模块作为脚本运行时，执行测试套件）。

<h4 id='6.1.2.'>6.1.2. 模块搜索路径</h4>

当导入`spam`模块时，解释器首先搜索具有该名称的内置模块。如果未找到相应的内置模块，则会在由[sys.path](https://docs.python.org/3.6/library/sys.html#sys.path)变量给出的目录列表中搜索`spam.py`文件。 从以下路径初始化[sys.path](https://docs.python.org/3.6/library/sys.html#sys.path)变量：

+ 包含输入脚本（.py源文件）的目录（或未指定输入文件时，则是执行python指令的当前目录）；
+ [PYTHONPATH](https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONPATH)（该变量包含一系列目录名，该变量具有与shell变量`PATH`相同的语法）；
+ 依赖于安装的默认值。

**Note**: On file systems which support symlinks, the directory containing the input script is calculated after the symlink is followed. In other words the directory containing the symlink is not added to the module search path.

初始化后，Python程序可以修改[sys.path](https://docs.python.org/3.6/library/sys.html#sys.path)。包含正在运行的脚本的目录位于搜索路径的开头，位于标准库路径之前。这意味着将加载该目录中的脚本，而不是库目录中的同名模块。除非有意更换，否则这是一个错误。有关更多信息，请参见[标准模块](https://docs.python.org/3.6/tutorial/modules.html#tut-standardmodules)一节。

<h4 id='6.1.3.'>6.1.3. “编译”Python文件</h4>

为了加速加载模块，Python将每个模块的编译版本缓存在`__pycache__`目录下的名为`module.version.pyc`的文件，其中版本`version`对编译文件的格式进行编码; 它通常包含Python版本号。例如，在CPython release版本3.3中，`spam.py`的编译版本将被缓存为`__pycache__/spam.cpython-33.pyc`。此命名约定允许来自不同release版本和不同版本的Python的已编译模块共存。

Python检查源代码的修改日期，并与源代码的编译版本进行比对，以查看编译版本是否已过期，并是否需要重新编译。这是一个完全自动化的过程。此外，编译的模块与平台无关，因此可以在具有不同架构的系统之间共享同一个的库。

Python在两种情况下不检查缓存（的编译版本）。首先，它总是重新编译并且不存储直接从命令行加载的模块的结果。其次，如果没有源模块，它不会检查缓存（的编译版本）。为了支持无源（仅编译）版本，已编译的模块必须位于源目录中，并且不得有源文件版本模块。

给专家的一些提示：

+ 您可以在Python命令上使用[-O](https://docs.python.org/3.6/using/cmdline.html#cmdoption-o)或[-OO](https://docs.python.org/3.6/using/cmdline.html#cmdoption-oo)开关（选项）来减小已编译模块的大小。`-O`开关（选项）删除断言`assert`语句时，`-OO`开关（选项）和`__doc__`文档字符串。由于某些程序可能依赖于（使）这些功能（可用），因此只有当您知道自己在做什么时，才使用此选项。“优化”模块有一个`opt-`标签且“优化”模块大小通常更小。未来版本可能会改变优化的效果。
+ 从`.pyc`文件读取程序时，程序运行速度不比从`.py`文件读取程序时运行速度快; 对`.pyc`文件来说，唯一更快的是它们加载的速度。
+ [compileall](https://docs.python.org/3.6/library/compileall.html#module-compileall)模块可以为某个目录中的所有模块创建`.pyc`文件。
+ 有关此过程的更多详细信息，包括决策的流程图，请参阅[PEP 3147](https://www.python.org/dev/peps/pep-3147)。

<h4 id='6.2.'>6.2. 标准模块</h4>

Python附带了一个标准模块库，在一个单独的、称为Python库参考手册（以下称为“库参考手册”）的文档中对标准模块库进行了描述。一些模块内置于解释器中; 这些内置模块为不属于Python语言核心的但仍然内置于Python中的操作提供了访问权限，以提高效率或为系统调用等操作系统原生操作提供访问权限。内置模块的集合（有哪些模块是内置模块）是可以配置的，同时也取决于底层平台。例如，[winreg](https://docs.python.org/3.6/library/winreg.html#module-winreg)模块仅在Windows系统上提供。一个特定的模块值得注意：[sys](https://docs.python.org/3.6/library/sys.html#module-sys)，它是内置于每个Python解释器中的。变量`sys.ps1`和`sys.ps2`定义用作主提示符和次提示符的字符串：

    >>> import sys
    >>> sys.ps1
    '>>> '
    >>> sys.ps2
    '... '
    >>> sys.ps1 = 'C> '
    C> print('Yuck!')
    Yuck!
    C>

仅在交互解释器下才定义这两个变量。

变量`sys.path`是一个字符串列表，用于确定解释器的模块搜索路径。从环境变量[PYTHONPATH](https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONPATH)获取，或者当环境变量[PYTHONPATH](https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONPATH)没有设置时从一个内置默认值获取一个默认路径来初始化变量`sys.path`。你可以使用标准的列表操作来修改它。

    >>> import sys
    >>> sys.path.append('/ufs/guido/lib/python')
    
<h4 id='6.3.'>6.3. dir() 函数</h4>

内置函数[dir()](https://docs.python.org/3.6/library/functions.html#dir)用于列出模块定义的名称。它返回一个有序的字符串列表：

    >>> import fibo, sys
    >>> dir(fibo)
    ['__name__', 'fib', 'fib2']
    >>> dir(sys)  
    ['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
     '__package__', '__stderr__', '__stdin__', '__stdout__',
     '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
     '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
     'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
     'call_tracing', 'callstats', 'copyright', 'displayhook',
     'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
     'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
     'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
     'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
     'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
     'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
     'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
     'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
     'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
     'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
     'thread_info', 'version', 'version_info', 'warnoptions']

当不传入任何参数时，[dir()](https://docs.python.org/3.6/library/functions.html#dir)列出您当前定义的所有名称：

[dir()](https://docs.python.org/3.6/library/functions.html#dir)不列出内置函数和内置变量的名称。如果你想要列出内置函数和内置变量的名称，它们被定义在标准模块[builtins](https://docs.python.org/3.6/library/builtins.html#module-builtins)中：

    >>> import builtins
    >>> dir(builtins)  
    ['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException',
     'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning',
     'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError',
     'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning',
     'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False',
     'FileExistsError', 'FileNotFoundError', 'FloatingPointError',
     'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError',
     'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError',
     'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError',
     'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented',
     'NotImplementedError', 'OSError', 'OverflowError',
     'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError',
     'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning',
     'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError',
     'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError',
     'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError',
     'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning',
     'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__',
     '__debug__', '__doc__', '__import__', '__name__', '__package__', 'abs',
     'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable',
     'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits',
     'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit',
     'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr',
     'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass',
     'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview',
     'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property',
     'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice',
     'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars',
     'zip']

<h4 id='6.4.'>6.4. 包</h4>

包是一种使用“点模块名称dotted module names”构造Python模块命名空间的方法。例如，模块名称`A.B`指定在名为`A`的包中名为`B`的子模块。就像模块的使用使得不同模块的作者不必担心彼此的全局变量名称一样，包（点模块名称）的使用使得多模块软件包，例如，NumPy或Pillow等，的作者不必担心彼此的模块名称一样。

假设您要设计一组模块（“一个包”），用于统一处理声音文件和声音数据。世上有许多不同的声音文件格式（通常由它们的扩展名来识别，例如：`.wav`，`.aiff`，`.au`），所以你可能需要为不同的文件格式之间转换来创建和维护一组不断增长的模块。您可能还想要对声音数据执行许多不同的操作（例如混音，添加回声，应用均衡器功能，创建人工立体声效果），所以此外您还将编写一组用以执行这些操作的、永无止境的模块流。以下是您的包的可能结构（以分层文件系统的形式表示）：

    sound/                          Top-level package
          __init__.py               Initialize the sound package
          formats/                  Subpackage for file format conversions
                  __init__.py
                  wavread.py
                  wavwrite.py
                  aiffread.py
                  aiffwrite.py
                  auread.py
                  auwrite.py
                  ...
          effects/                  Subpackage for sound effects
                  __init__.py
                  echo.py
                  surround.py
                  reverse.py
                  ...
          filters/                  Subpackage for filters
                  __init__.py
                  equalizer.py
                  vocoder.py
                  karaoke.py
                  ...

导入包时，Python会搜遍变量`sys.path`中的目录及其以下目录（子目录），从中查找包的子目录。

只有在目录下创建`__init__.py`文件，Python才会将这些目录视为包含包(containing packages)；这样做是为了防止具有通用名称的目录，例如`string`，无意中覆盖（隐藏）稍后在模块搜索路径上出现的有效模块。在最简单的情况下，`__init__.py`可以只是一个空文件，但它也可以执行包的初始化代码或设置`__all__`变量，稍后描述。

用户可以从包中导入单个模块，例如：

    import sound.effects.echo
    
这会加载子模块`sound.effects.echo`。必须以其全名引用它。

    sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
    
导入子模块的另一种方法是：

    from sound.effects import echo
    
这也会加载子模块echo，并可以在没有包前缀的情况下引用子模块echo，因此可以按如下方式使用：

    echo.echofilter(input, output, delay=0.7, atten=4)
    
另一种直接导入所需的函数或变量的方法：

    from sound.effects.echo import echofilter

同样，这会加载子模块echo，但这会使其函数 `echofilter()`直接可用：

    echofilter(input, output, delay=0.7, atten=4)

请注意，在使用`from package import item`时，其中`item`可以是包的一个子模块（或者子包），也可以是包中定义一些其他名称，例如函数、类、变量。`import`语句先测试`item`是否在包中定义；如果没有，则假定它是一个模块并尝试加载它。如果找不到， 则会引发[ImportError](https://docs.python.org/3.6/library/exceptions.html#ImportError)异常。

相反地，当使用` import item.subitem.subsubitem`语法，除了最后一个`item`，每一个`item`都必须是包；最后一个`item`可以是一个包也可以是一个模块，但不可以是前一个`item`中定义的类或者函数或者变量。

<h4 id='6.4.1'>6.4.1 从包中导入所有(Importing * from Package)</h4>

现在当用户输入`from sound.effects import *`的时候会发生什么？理想情况下，人们希望上述语句会以某种方式，在文件系统中查找当前包中存在哪些子模块，并将这些子模块全部导入。这可能需要很长时间，导入子模块可能会产生不必要的副作用，这种副作用只有在显式导入子模块时才会发生。

唯一的解决方案是让包作者提供包的显式索引。[import](https://docs.python.org/3.6/reference/simple_stmts.html#import)语句使用以下约定：如果包的`__init__.py`代码定义了一个名为`__all__`的列表，则它将被视为使用`from package import *`时应导入的模块名称列表。在发布新版本的软件包时，由软件包作者决定是否保持此列表的最新状态。如果包装作者没有看到从包装中导入*的用途，他们也可能决定不支持它。例如，`sound/effects/__init__.py`文件可能包含以下代码：

    __all__ = ["echo", "surround", "reverse"]

这意味着`from sound.effects import *`将导入包的三个`sound`命名子模块`echo`,`surround`,`reverse`。

如果没有定义`__all__`，则`from sound.effects import *`语句也不会将`sound.effects`包中的所有子模块导入到当前命名空间；它只确保导入`sound.effects`包（可能运行`__init__.py`中的任何初始化代码）并导入包中定义的所有名称。这些名称包括由`__init__.py`定义的所有名称和由`__init__.py`显示加载的子模块。它还包括由先前[import](https://docs.python.org/3.6/reference/simple_stmts.html#import)语句显式加载的包的任何子模块。考虑如下代码：

    import sound.effects.echo
    import sound.effects.surround
    from sound.effects import *

在此示例中，在执行语句`from...import`时，`echo`和`surround`模块将导入当前命名空间，因为它们是在`sound.effects`包中定义的。（这在`__all__`定义时也有效 。）

<h4 id='6.4.2'>6.4.2 包内引用(Intra-package Reference)</h4>

当包被组织成子包时（与sound示例中的包一样），您可以使用绝对导入来引用兄弟包的子模块。例如，如果模块`sound.filters.vocoder`需要使用`sound.effects`包中的`echo`模块，则可以使用`from sound.effects import echo`。

您还可以使用相对导入，import语句的`from module import name`形式可以使用相对导入。相对导入使用点来指示相对导入中涉及的当前包和父包。 例如，在`surround`模块中，您可以使用：

    from . import echo
    from .. import formats
    from ..filters import equalizer

请注意，相对导入基于当前模块的名称。由于主模块的名称始终是`"__main__"`，因此用作Python应用程序主模块的模块必须始终使用绝对导入。

<h4 id='6.4.3'>6.4.3 存储在多个目录中的包</h4>

包支持另一个特殊属性[__path__](https://docs.python.org/3.6/reference/import.html#__path__)。This is initialized to be a list containing the name of the directory holding the package’s __init__.py before the code in that file is executed. This variable can be modified; doing so affects future searches for modules and subpackages contained in the package.

While this feature is not often needed, it can be used to extend the set of modules found in a package.

## 个人笔记