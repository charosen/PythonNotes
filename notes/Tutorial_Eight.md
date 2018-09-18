# Python 教程（八）-- 输入输出

## 原文链接

[7. Input and Output](https://docs.python.org/3.6/tutorial/inputoutput.html)

## 原文翻译

<h3 id='7.'>7. 输入与输出</h3>

Python有许多展示程序输出的方法; 数据能够以人类可读的形式输出，或写入文件以供将来使用。本章将讨论所有可能输出方法中的一部分。

<h4 id='7.1.'>7.1. 更高级的输出格式</h4>

到目前为止，我们遇到了两种写值的方法：*表达式语句* 和[print()](https://docs.python.org/3.6/library/functions.html#print)函数。（第三种方法是使用文件对象的 write() 方法;可以使用`sys.stdout`来引用标准输出文件。有关详细信息，请参阅库参考手册。）

通常，您需要更多地控制输出的格式，而不仅仅是打印空格分隔的值。有两种方法可以格式化输出; 第一种方法是自己完成所有的字符串处理; 使用字符串切片和连接操作，您可以创建任何您可以想象的输出排版。字符串类型有一些方法可以执行将字符串填充到给定列宽的有用操作; 这些将在稍后讨论。第二种方法是使用格式化的字符串文字[formatted string literals](https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings)或 [str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format)方法。

[string](https://docs.python.org/3.6/library/string.html#module-string)模块包含一个[Template](https://docs.python.org/3.6/library/string.html#string.Template)模版类，它提供了另一种将值替换到字符串中的方法。

当然还有一个问题：如何将值转换为字符串？幸运的是，Python有办法将任何值转换为字符串：将值传递给[repr()](https://docs.python.org/3.6/library/functions.html#repr)或[str()](https://docs.python.org/3.6/library/stdtypes.html#str)函数。

[str()](https://docs.python.org/3.6/library/stdtypes.html#str)函数以人类可读的格式返回数据值内容，同时，[repr()](https://docs.python.org/3.6/library/functions.html#repr)函数以解释器可读取的格式返回数据值内容（如果没有等效语法，则强制抛出[SyntaxError](https://docs.python.org/3.6/library/exceptions.html#SyntaxError)异常or will force a [SyntaxError](https://docs.python.org/3.6/library/exceptions.html#SyntaxError) if there is no equivalent syntax）。对于无法用人类可读的格式来表示的对象，[str()](https://docs.python.org/3.6/library/stdtypes.html#str)以与[repr()](https://docs.python.org/3.6/library/functions.html#repr)相同的格式返回数据值内容。

许多值，如数字，或结构，如列表和字典，使用上述任一函数都具有相同的表示。特别地，字符串使用上述两个函数时，有两种不同的表示。

例子：

    >>> s = 'Hello, world.'
    >>> str(s)
    'Hello, world.'
    >>> repr(s)
    "'Hello, world.'"
    >>> str(1/7)
    '0.14285714285714285'
    >>> x = 10 * 3.25
    >>> y = 200 * 200
    >>> s = 'The value of x is ' + repr(x) + ', and y is ' + repr(y) + '...'
    >>> print(s)
    The value of x is 32.5, and y is 40000...
    >>> # The repr() of a string adds string quotes and backslashes:
    ... hello = 'hello, world\n'
    >>> hellos = repr(hello)
    >>> print(hellos)
    'hello, world\n'
    >>> # The argument to repr() may be any Python object:
    ... repr((x, y, ('spam', 'eggs')))
    "(32.5, 40000, ('spam', 'eggs'))"

以下是两种编写数字平方、立方的表的方法：

    >>> for x in range(1, 11):
    ...     print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')
    ...     # Note use of 'end' on previous line
    ...     print(repr(x*x*x).rjust(4))
    ...
     1   1    1
     2   4    8
     3   9   27
     4  16   64
     5  25  125
     6  36  216
     7  49  343
     8  64  512
     9  81  729
    10 100 1000
    
    >>> for x in range(1, 11):
    ...     print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x))
    ...
     1   1    1
     2   4    8
     3   9   27
     4  16   64
     5  25  125
     6  36  216
     7  49  343
     8  64  512
     9  81  729
    10 100 1000

（请注意，在第一个示例中，每个列之间的一个空格是由[print()](https://docs.python.org/3.6/library/functions.html#print)函数添加的：默认情况下，它在参数之间添加空格。）

此示例演示了字符串对象的[str.rjust()](https://docs.python.org/3.6/library/stdtypes.html#str.rjust)方法，该方法通过在左侧填充空格，在给定宽度的字段（正负号也占一个字符位）中对字符串进行右对齐。类似的方法有[str.ljust()](https://docs.python.org/3.6/library/stdtypes.html#str.ljust)和[str.center()](https://docs.python.org/3.6/library/stdtypes.html#str.center)。这些方法不会写任何东西，它们只返回一个新字符串。如果输入字符串太长，它们不会截断它，反而会保持不变; 这会弄乱你的列布局，但这通常比截断字符串更好，截断字符串可能会返回错误数值。（如果你真的想要截断，你可以随时添加切片操作，如 `x.ljust(n)[:n]`。）

还有另一种方法，[str.zfill()](https://docs.python.org/3.6/library/stdtypes.html#str.zfill)，在数字字符串的左侧填充零。它能够识别正负号（正负号也占一个字符位）：

    >>> '12'.zfill(5)
    '00012'
    >>> '-3.14'.zfill(7)
    '-003.14'
    >>> '3.14159265359'.zfill(5)
    '3.14159265359'

[str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format)方法的基本用法如下所示：

    >>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))
    We are the knights who say "Ni!"

花括号和花括号中的字符（称为格式字段）将被替换为传递给[str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format)方法的对象。括号中的数字可用于表示传递给[str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format)方法的对象的位置 。

    >>> print('{0} and {1}'.format('spam', 'eggs'))
    spam and eggs
    >>> print('{1} and {0}'.format('spam', 'eggs'))
    eggs and spam

如果在[str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format)方法中使用关键字实参，则使用参数的名称引用它们的值。
    
    >>> print('This {food} is {adjective}.'.format(
    ...       food='spam', adjective='absolutely horrible'))
    This spam is absolutely horrible.

位置实参和关键字实参可以任意组合：
    
    >>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
                                                           other='Georg'))
    The story of Bill, Manfred, and Georg.

`'!a'`（使用[ascii()](https://docs.python.org/3.6/library/functions.html#ascii)），`'!s'`（使用[str()](https://docs.python.org/3.6/library/stdtypes.html#str)）和`'!r'`（使用[repr()](https://docs.python.org/3.6/library/functions.html#repr)）可用于在格式化输出之前转换值：

    >>> contents = 'eels'
    >>> print('My hovercraft is full of {}.'.format(contents))
    My hovercraft is full of eels.
    >>> print('My hovercraft is full of {!r}.'.format(contents))
    My hovercraft is full of 'eels'.

字段名称之后可以跟随可选的`':'`冒号和格式说明符。这样可以更好地控制值的格式化方式。以下示例将`Pi`舍入到小数点后三位。

    >>> import math
    >>> print('The value of PI is approximately {0:.3f}.'.format(math.pi))
    The value of PI is approximately 3.142.

在`':'`冒号之后传递一个整数将设置该字段的最小字符宽度数。这对于工整地输出表格很有用。

    >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
    >>> for name, phone in table.items():
    ...     print('{0:10} ==> {1:10d}'.format(name, phone))
    ...
    Jack       ==>       4098
    Dcab       ==>       7678
    Sjoerd     ==>       4127
    
如果你有一个非常长的格式字符串，但你不想拆分它，那么您最好使用名称来引用准备格式化输出的变量。这可以通过简单地传递字典实参并使用方括号'[]'来访问字典的键来完成：

    >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
    >>> print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '
    ...       'Dcab: {0[Dcab]:d}'.format(table))
    Jack: 4098; Sjoerd: 4127; Dcab: 8637678
    
这也可以通过将表作为带有'**'的关键字实参传递完成。
    
    >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
    >>> print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
    Jack: 4098; Sjoerd: 4127; Dcab: 8637678

这与内置函数结合使用特别有用，内置函数[vars()](https://docs.python.org/3.6/library/functions.html#vars)返回包含所有局部变量的字典。

有关使用[str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format)格式化字符串的完整概述，请参阅[Format String Syntax](https://docs.python.org/3.6/library/string.html#formatstrings)。

<h4 id='7.1.1.'>7.1.1. 旧的(过去的)字符串格式</h4>

`%`操作符还可以用于格式化字符串。`%`操作符将左操作数解释为类似`sprintf()`风格的格式字符串,并用右操作数来格式化字符串，并返回由此格式化操作产生的字符串。例如：

    >>> import math
    >>> print('The value of PI is approximately %5.3f.' % math.pi)
    The value of PI is approximately 3.142.

更多详情，请参考[printf-style String Formatting](https://docs.python.org/3.6/library/stdtypes.html#old-string-formatting)小节。

<h4 id='7.2.'>7.2. 读写文件</h4>

[open()](https://docs.python.org/3.6/library/functions.html#open)返回一个文件对象[file object](https://docs.python.org/3.6/glossary.html#term-file-object)，[open()](https://docs.python.org/3.6/library/functions.html#open)最常用的有两个参数：`open(filename, mode)`。

    >>> f = open('workfile', 'w')

第一个参数是包含文件名的字符串。第二个参数是另一个字符串，其中包含一些描述文件打开方式的字符。`mode`可以是`'r'`（当仅读取文件时），可以是`'w'`(当仅写入文件时（将删除同名的现有文件）)，可以是`'a'`(当打开文件进行追加时; 写入文件的任何数据都会自动添加到最后。）可以是'r+'(当打开文件进行读写时）。所述模式`mode`参数是可选的,其默认值为`'r'`; 当`mode`取默认值`'r'`时，可以省略。

通常，文件以 *文本模式* 打开，这意味着您从(向)文件读取(写入)字符串，这些文件以特定编码进行编码。如果未指定编码，则默认编码取决于平台（请参阅[open()](https://docs.python.org/3.6/library/functions.html#open)）。当`'b'`附加到`mode`模式参数后，以 *二进制模式* 打开文件 ：以bytes对象的形式读取和写入数据。此模式应用于所有不包含文本的文件。

在文本模式下，读取时的默认设置是将平台特定的行结尾符（`\n`在Unix上，`\r\n`在Windows上）全转换为`\n`。文本模式下，写入时的默认设置是将`\n`转换为特定于平台的行结尾符。这对文件数据的隐式修改对于文本文件来说影响不大，但会破坏`JPEG`或`EXE`文件中二进制数据。在读取和写入此类文件（`JPEG`或`EXE`）时要非常小心地使用二进制模式。

在处理文件对象时，最好使用关键字[with](https://docs.python.org/3.6/reference/compound_stmts.html#with)。优点是在对文件执行一系列操作后会正确关闭文件，即使在某个时刻引发了异常，也会正确关闭文件。使用[with](https://docs.python.org/3.6/reference/compound_stmts.html#with)也比写等价的[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)-[finally](https://docs.python.org/3.6/reference/compound_stmts.html#finally)语句块要短得多：

    >> with open('workfile') as f:
    ...     read_data = f.read()
    >>> f.closed
    True

如果您没有使用[with](https://docs.python.org/3.6/reference/compound_stmts.html#with)关键字，那么您应该调用 `f.close()`以关闭该文件并立即释放它使用的任何系统资源。如果您没有显式关闭文件，Python的垃圾收集器最终将销毁该对象并为您关闭打开的文件，但该文件可能会保持打开状态一段时间。另一个风险是不同的Python版本将在不同的时间进行清理。

通过[with](https://docs.python.org/3.6/reference/compound_stmts.html#with)语句或通过调用`f.close()`关闭文件对象后，任何尝试使用该文件对象将自动失败。

    >>> f.close()
    >>> f.read()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: I/O operation on closed file.

<h4 id='7.2.1.'>7.2.1. 文件对象的方法</h4>

本节中的其余示例将假定已创建一个名为`f`的文件对象。

要读取文件的内容，请调用`f.read(size)`，读取一定数量数据并将其作为字符串（在文本模式下）或字节对象（在二进制模式下）返回。 `size`是可选的数字参数。当`size`被省略或为负时，将读取并返回文件的全部内容; 如果文件的大小是机器内存的两倍，那么这就是你的问题。当`size`为正时，最多读取并返回`size`字节。如果已到达文件末尾，`f.read()`则返回空字符串（`''`）。

    >>> f.read()
    'This is the entire file.\n'
    >>> f.read()
    ''

`f.readline()`从文件中读取一行; 换行符（`\n`）留在字符串的末尾，如果文件没有以换行符结尾，则只在文件的最后一行省略换行符。这使得返回值明确无误; 如果`f.readline()`返回一个空字符串，则表示已到达文件末尾，而空行表示为一个只包含一个换行符的字符串`'\n'`。

    >>> f.readline()
    'This is the first line of the file.\n'
    >>> f.readline()
    'Second line of the file\n'
    >>> f.readline()
    ''

要从文件中读取多行，可以循环遍历文件对象。这样做是内存高效的，快速的，并且代码简单：

    >>> for line in f:
    ...     print(line, end='')
    ...
    This is the first line of the file.
    Second line of the file

如果要读取文件的所有行并返回一个由文件各行组成的字符串列表，也可以使用`list(f)`或`f.readlines()`。

`f.write(string)`将`string`的内容写入文件，返回写入的字符数。

    >>> f.write('This is a test\n')
    15
    
在写入其他类型的对象之前，需要将其他类型的对象转换为字符串（在文本模式下）或字节对象（在二进制模式下）：

    >>> value = ('the answer', 42)
    >>> s = str(value)  # convert the tuple to string
    >>> f.write(s)
    18

`f.tell()`返回一个整数，这个整数给出在文件中文件对象的当前位置，此当前位置使用二进制模式下从文件开头到当前位置的字节数和文本模式下的不透明（难懂的）数字来表示。

要更改文件对象的位置，请使用`f.seek(offset, from_what)`。当前位置通过向参考点添加偏移量(`offset`)来计算; 参数点由`from_what`参数指定。`from_what`为0时，参考点为文件的开头；为1时，参考点为当前文件位置；为2时，参考点为文件的末尾。`from_what`默认值为0，默认使用文件的开头作为参考点，当取默认值时，`from_what`参数可以省略。

    >>> f = open('workfile', 'rb+')
    >>> f.write(b'0123456789abcdef')
    16
    >>> f.seek(5)      # Go to the 6th byte in the file
    5
    >>> f.read(1)
    b'5'
    >>> f.seek(-3, 2)  # Go to the 3rd byte before the end
    13
    >>> f.read(1)
    b'd'

在文本文件中（那些模式参数中没有`b`而打开的文件），只允许相对于文件开头的搜索（寻找文件结尾`seek(0, 2)`除外），唯一有效的偏移值`offset`是从`f.tell()`获得的或者是零。任何其他偏移值都会产生未定义的行为。

文件对象还有一些额外的方法，例如`isatty()`和`truncate()`，它们使用得较不频繁;有关文件对象的完整指南，请参阅“库参考手册”。

<h4 id='7.2.2.'>7.2.2. 使用json格式存储结构化数据</h4>

从（向）文件读取（写入）字符串十分容易。但读取和写入数字类型Numbers就有些困难，因为`read()`方法只返回字符串，所以`read()`方法的返回值必须再传递给类似[int()](https://docs.python.org/3.6/library/functions.html#int)的函数，这些函数接收一个`'123'`的字符串为参数，并返回其数值`123`。当你想要保存更为复杂的数据类型时，例如嵌套列表和字典，手动解析和序列化输出将会变得十分复杂。

与其让用户不断编写和调试代码以将复杂的数据类型保存到文件中，Python允许您使用称为十分流行的数据交换格式[JSON (JavaScript Object Notation)](http://json.org/)。标准模块[json](https://docs.python.org/3.6/library/json.html#module-json)可以接收Python数据结构，并将它们转换为字符串表示形式; 这个过程称为序列化。从字符串表示中重建数据称为反序列化。在序列化和反序列化之间，表示对象的字符串可能已存储在文件或数据中，或通过网络连接发送到某个远程机器。

**注意**:JSON格式通常被现代应用程序用于允许数据交换。许多程序员已经熟悉它，这使其成为互操作性的良好选择。

如果您有一个对象`x`，则可以使用一行简单的代码查看其JSON字符串表示：

    >>> import json
    >>> json.dumps([1, 'simple', 'list'])
    '[1, "simple", "list"]'

调用[dumps()](https://docs.python.org/3.6/library/json.html#json.dumps)函数的另一个变体[dump()](https://docs.python.org/3.6/library/json.html#json.dump)，[dump()](https://docs.python.org/3.6/library/json.html#json.dump)将对象序列化并存储到文本文件。因此，如果`f`是一个以'w'打开的[文本文件](https://docs.python.org/3.6/glossary.html#term-text-file)对象，我们可以这样做：

    json.dump(x, f)
   
 
如果`f`是一个以'r'打开的[文本文件](https://docs.python.org/3.6/glossary.html#term-text-file)对象，要再次解码对象：

    x = json.load(f)
    
这种简单的序列化技术可以处理列表和字典，但是在JSON中序列化任意类实例需要额外的努力。[json](https://docs.python.org/3.6/library/json.html#module-json)模块的参考手册包含对此的解释。

See also: [pickle](https://docs.python.org/3.6/library/pickle.html#module-pickle) - the pickle module

与[JSON](https://docs.python.org/3.6/tutorial/inputoutput.html#tut-json)相反， *pickle* 是一种允许对任意复杂Python对象进行序列化的协议。因此，它特定于Python，不能用于与其他语言编写的应用程序通信。默认情况下它也是不安全的：如果数据是由熟练的攻击者精心设计的，则反序列化来自不受信任来源的pickle数据可以执行任意代码。


## 个人笔记