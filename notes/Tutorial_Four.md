# Python 教程（四）--Python的非正式介绍

## 原文链接

[3. An Informal Introduction to Python](https://docs.python.org/3.6/tutorial/introduction.html)

## 原文翻译

<h3 id='3.'>3. Python的非正式介绍</h3>

在以下例子中，输入和输出通过是否存在提示符来区分（>>>和...）：为了重复示例，必须在出现提示符时，在提示符后输入所有代码; 不以提示符开头的行是Python解释器的输出。请注意，示例中某一行上的次提示符意味着您必须再键入一个空行; 这用于结束多行命令。

本手册中的许多示例，即使是在交互模式下输入的示例，都包含注释。Python中的注释以哈希字符开头 #，并延伸到物理行的末尾。注释可能出现在行的开头或跟随空格或代码，但不会在字符串文字中。字符串文字中的哈希字符只是一个哈希字符。由于注释是为了解释代码而不交由Python解释器处理，因此在键入示例时可以省略它们。

一些例子：

    # this is the first comment
    spam = 1  # and this is the second comment
              # ... and now a third!
    text = "# This is not a comment because it's inside quotes."

<h4 id='3.1.'>3.1. 将Python作为计算器使用</h4>

让我们尝试一下简单的Python命令。启动解释器并等待主提示符出现`>>>`。（它不应当花很长时间。）

<h4 id='3.1.1.'>3.1.1. 数字Numbers</h4>

解释器可用作一个简单的计算器：您可以在其上键入表达式，它将返回结果值。表达式语法是直接的：`+`, `-`, `*`，`/`等操作符的工作原理和其他大部分语言相同。（例如,Pascal和C）；括号（`()`）可用于分组。例如：

    >>> 2 + 2
    4
    >>> 50 - 5*6
    20
    >>> (50 - 5*6) / 4
    5.0
    >>> 8 / 5  # division always returns a floating point number
    1.6
    
Python中使用整型`int`来表示整数（例如`2`,`4`,`20`），使用浮点型`float`表示带小数部分的数。我们将在后续教程中学习更多数字类型。

除法(`/`)永远返回一个浮点数；地板除法(`//`)永远返回一个整数（省略真实结果的小数部分)，使用(`%`)来计算取余：

    >>> 17 / 3  # classic division returns a float
    5.666666666666667
    >>>
    >>> 17 // 3  # floor division discards the fractional part
    5
    >>> 17 % 3  # the % operator returns the remainder of the division
    2
    >>> 5 * 3 + 2  # result * divisor + remainder
    17

使用(`**`)来计算幂运算，幂运算符比左操作数的单目运算符优先级高，比右操作数的单目运算符优先级低：

    >>> 5 ** 2  # 5 squared
    25
    >>> 2 ** 7  # 2 to the power of 7
    128

使用等号（`=`）为变量赋值。之后，在下一个交互式提示之前不会显示任何结果：

    >>> width = 20
    >>> height = 5 * 9
    >>> width * height
    900

若未给变量赋值，Python解释器会报错：

    >>> n  # try to access an undefined variable
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'n' is not defined

Python完全支持浮点数; 具有混合类型操作数的运算符将整数操作数转换为浮点数：

    >>> 4 * 3.75 - 1
    14.0
    
在交互模式下，最后打印的表达式将分配给变量`_`。这意味着当您使用Python作为桌面计算器时，继续计算会更容易一些，例如：

    >>> tax = 12.5 / 100
    >>> price = 100.50
    >>> price * tax
    12.5625
    >>> price + _
    113.0625
    >>> round(_, 2)
    113.06
    
该变量应被用户视为只读。不要为其显式赋值 - 您将创建一个具有相同名称的独立局部变量，以使用其魔术行为屏蔽内置变量。

除了整型[int](https://docs.python.org/3.6/library/functions.html#int)和浮点型[float](https://docs.python.org/3.6/library/functions.html#float)，Python还支持其他数字类型，例如十进制数[Decimal](https://docs.python.org/3.6/library/decimal.html#decimal.Decimal)和有理数[Fraction](https://docs.python.org/3.6/library/fractions.html#fractions.Fraction)。Python还内置支持复数[complex numbers](https://docs.python.org/3.6/library/stdtypes.html#typesnumeric)，并使用`j`或者`J`后缀来表示虚部。（例如，`3+5j`)。

<h4 id='3.1.2.'>3.1.2. 字符串Strings</h4>

除了数字之外，Python还可以操作字符串，字符串可以使用多种方式表达。它们可以用单引号（`'...'`）或双引号（`"..."`）括起来，结果是相同的。 `\`可用于转义引号：

    >>> 'spam eggs'  # single quotes
    'spam eggs'
    >>> 'doesn\'t'  # use \' to escape the single quote...
    "doesn't"
    >>> "doesn't"  # ...or use double quotes instead
    "doesn't"
    >>> '"Yes," they said.'
    '"Yes," they said.'
    >>> "\"Yes,\" they said."
    '"Yes," they said.'
    >>> '"Isn\'t," they said.'
    '"Isn\'t," they said.'

在交互式解释器中，输出字符串用引号括起来，特殊字符用反斜杠转义。虽然这可能有时看起来与输入不同（最外层的引号可能会改变），但这两个字符串是等价的。如果字符串包含单引号而没有双引号，则该字符串用双引号括起来，否则用单引号括起来。[print()](https://docs.python.org/3.6/library/functions.html#print)函数通过省略最外层引号并打印转义字符和特殊字符，产生更可读的输出：

    >>> '"Isn\'t," they said.'
    '"Isn\'t," they said.'
    >>> print('"Isn\'t," they said.')
    "Isn't," they said.
    >>> s = 'First line.\nSecond line.'  # \n means newline
    >>> s  # without print(), \n is included in the output
    'First line.\nSecond line.'
    >>> print(s)  # with print(), \n produces a new line
    First line.
    Second line.
    
如果您不希望将以`\`为前缀的字符解释为特殊字符，则可以在第一个引号之前添加`r`来使用原始字符串：

    >>> print('C:\some\name')  # here \n means newline!
    C:\some
    ame
    >>> print(r'C:\some\name')  # note the r before the quote
    C:\some\name

字符串文字可以跨越多行。一种方法是使用三引号： """..."""或'''...'''。换行符自动包含在字符串中，但可以通过在行尾添加`\`来去除换行符。以下示例：

    print("""\
    Usage: thingy [OPTIONS]
         -h                        Display this usage message
         -H hostname               Hostname to connect to
    """)

产生以下输出（请注意，不包括初始换行符）：

    Usage: thingy [OPTIONS]
         -h                        Display this usage message
         -H hostname               Hostname to connect to

字符串可以使用`+`操作符串联（粘合在一起），使用`*`操作符重复:

    >>> # 3 times 'un', followed by 'ium'
    >>> 3 * 'un' + 'ium'
    'unununium'

两个或多个彼此相邻的字符串文字（即引号之间的字符串）会自动连接。

    >>> 'Py' 'thon'
    'Python'

当您想要断开长字符串时，此功能特别有用：

    >>> text = ('Put several strings within parentheses '
    ...         'to have them joined together.')
    >>> text
    'Put several strings within parentheses to have them joined together.'

这仅适用于两个字符串文字，而不是字符串变量或字符串表达式：

    >>> prefix = 'Py'
    >>> prefix 'thon'  # can't concatenate a variable and a string literal
    ...
    SyntaxError: invalid syntax
    >>> ('un' * 3) 'ium'
      ...
    SyntaxError: invalid syntax

如果要连接字符串变量或连接字符串变量和字符串文字，请使用`+`：

    >>> prefix + 'thon'
    'Python'
    
字符串可以被索引（下标），第一个字符具有索引0.没有单独的字符类型; 一个字符只是一个大小为1的字符串：

    >>> word = 'Python'
    >>> word[0]  # character in position 0
    'P'
    >>> word[5]  # character in position 5
    'n'
    
索引也可以是负数，从右边开始计算：

    >>> word[-1]  # last character
    'n'
    >>> word[-2]  # second-last character
    'o'
    >>> word[-6]
    'P'

请注意，由于-0与0相同，因此负索引从-1开始。

除索引外，还支持切片。索引用于获取单个字符，切片允许您获取子字符串：

    >>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
    'Py'
    >>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
    'tho'

请注意分片始终包含开始，并始终排除结束。这确保`s[:i] + s[i:]`始终等于`s`:

    >>> word[:2] + word[2:]
    'Python'
    >>> word[:4] + word[4:]
    'Python'

切片索引具有有用的默认值; 省略第一个索引默认为零，省略第二个索引默认为要切片的字符串的大小。

    >>> word[:2]   # character from the beginning to position 2 (excluded)
    'Py'
    >>> word[4:]   # characters from position 4 (included) to the end
    'on'
    >>> word[-2:]  # characters from the second-last (included) to the end
    'on'

记住切片如何工作的一种方法是将索引视为字符之间的位置，第一个字符的左边缘编号为0.然后，长度为n的字符串的最后一个字符的右边缘具有索引n，例如：

     +---+---+---+---+---+---+
     | P | y | t | h | o | n |
     +---+---+---+---+---+---+
     0   1   2   3   4   5   6
    -6  -5  -4  -3  -2  -1
    
第一行数字给出了字符串中索引0 ... 6的位置; 第二行给出相应的负索引。从i到 j的切片分别由标记为i和j的边之间的所有字符组成。

对于非负索引，切片的长度是索引的差值，如果两者都在边界内。例如，长度word[1:3]为2。

尝试使用过大的索引将导致错误

    >>> word[42]  # the word only has 6 characters
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    IndexError: string index out of range

但是，切片能优雅地处理超出范围的切片索引：

    >>> word[4:42]
    'on'
    >>> word[42:]
    ''

Python字符串无法更改 - 它们是不可变对象[immutable](https://docs.python.org/3.6/glossary.html#term-immutable)。因此，分字符串中的索引位置赋值会导致错误：

    >>> word[0] = 'J'
      ...
    TypeError: 'str' object does not support item assignment
    >>> word[2:] = 'py'
      ...
    TypeError: 'str' object does not support item assignment

如果您需要不同的字符串，则应创建一个新字符串：

    >>> 'J' + word[1:]
    'Jython'
    >>> word[:2] + 'py'
    'Pypy'

内置函数[len()](https://docs.python.org/3.6/library/functions.html#len)返回字符串的长度：

    >>> s = 'supercalifragilisticexpialidocious'
    >>> len(s)
    34

See also:

[Text Sequence Type -- str](https://docs.python.org/3.6/library/stdtypes.html#textseq)

Strings are examples of sequence types, and support the common operations supported by such types.

[String Methods](https://docs.python.org/3.6/library/stdtypes.html#string-methods)

Strings support a large number of methods for basic transformations and searching.

[Formatted string literals](https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings)

String literals that have embedded expressions.

[Formatted string syntax](https://docs.python.org/3.6/library/string.html#formatstrings)

Information about string formatting with [str.format()](https://docs.python.org/3.6/library/stdtypes.html#str.format).

[printf-style String Formatting](https://docs.python.org/3.6/library/stdtypes.html#old-string-formatting)

The old formatting operations invoked when strings are the left operand of the `%` operator are described in more detail here.

<h4 id='3.1.3.'>3.1.3. 列表Lists</h4>

Python拥有许多复合数据类型，用于将其他值组合在一起。最通用的是列表，它可以写成方括号之间的一系列的逗号分隔值（项）。列表可以包含不同类型的元素，但通常元素都具有相同的类型。

    >>> squares = [1, 4, 9, 16, 25]
    >>> squares
    [1, 4, 9, 16, 25]

像字符串（以及所有其他内置序列[Sequence](https://docs.python.org/3.6/glossary.html#term-sequence)类型）一样，列表可以被索引和切片：

    >>> squares[0]  # indexing returns the item
    1
    >>> squares[-1]
    25
    >>> squares[-3:]  # slicing returns a new list
    [9, 16, 25]

所有切片操作都返回包含所请求元素的新列表。这意味着以下切片返回列表的新（浅）拷贝：

    >>> squares[:]
    [1, 4, 9, 16, 25]
    
列表还支持串联等操作：

    >>> squares + [36, 49, 64, 81, 100]
    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

与不可变对象[immutable](https://docs.python.org/3.6/glossary.html#term-immutable)的字符串不同，列表是可变对象[mutable](https://docs.python.org/3.6/glossary.html#term-mutable)类型，即可以更改其内容：

    >>> cubes = [1, 8, 27, 65, 125]  # something's wrong here
    >>> 4 ** 3  # the cube of 4 is 64, not 65!
    64
    >>> cubes[3] = 64  # replace the wrong value
    >>> cubes
    [1, 8, 27, 64, 125]

您还可以使用该append() 方法在列表末尾添加新项目（我们稍后会看到有关方法的更多信息）：

    >>> cubes.append(216)  # add the cube of 6
    >>> cubes.append(7 ** 3)  # and the cube of 7
    >>> cubes
    [1, 8, 27, 64, 125, 216, 343]

也可以给切片赋值，这甚至可以改变列表的大小或完全清除它：

    >>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
    >>> letters
    ['a', 'b', 'c', 'd', 'e', 'f', 'g']
    >>> # replace some values
    >>> letters[2:5] = ['C', 'D', 'E']
    >>> letters
    ['a', 'b', 'C', 'D', 'E', 'f', 'g']
    >>> # now remove them
    >>> letters[2:5] = []
    >>> letters
    ['a', 'b', 'f', 'g']
    >>> # clear the list by replacing all the elements with an empty list
    >>> letters[:] = []
    >>> letters
    []

内置函数len()也适用于列表：

    >>> letters = ['a', 'b', 'c', 'd']
    >>> len(letters)
    4
    
列表可以嵌套（创建包含其他列表的列表），例如：

    >>> a = ['a', 'b', 'c']
    >>> n = [1, 2, 3]
    >>> x = [a, n]
    >>> x
    [['a', 'b', 'c'], [1, 2, 3]]
    >>> x[0]
    ['a', 'b', 'c']
    >>> x[0][1]
    'b'

<h4 id='3.2.'>3.2. 编程的第一步</h4>

当然，我们可以使用Python完成更复杂的任务，而不仅仅是用于计算。例如，我们可以编写Fibonacci数列的初始子序列，如下所示：

    >>> # Fibonacci series:
    ... # the sum of two elements defines the next
    ... a, b = 0, 1
    >>> while b < 10:
    ...     print(b)
    ...     a, b = b, a+b
    ...
    1
    1
    2
    3
    5
    8

此示例介绍了几个新功能.

+ 第一行包含多元赋值：变量`a`并`b`同时被赋新值0和1.在最后一行再次使用多元赋值，证明在任何赋值发生之前，先计算等号右边的表达式。等号右边的表达式按从左到右的顺序进行计算。

+ [while](https://docs.python.org/3.6/reference/compound_stmts.html#while)循环：只要条件（此处为：`b < 10`）保持为真，循环就会执行。在Python中，就像在C中一样，任何非零整数值都是真值`True`; 零是假值`False`。条件也可以是字符串或列表，实际上是任何序列对象Sequence; 任何长度非零的东西都是真值`True`，空序列都是假值`False`。该示例中使用的测试是简单的比较。标准关系运算符的编写方式与C中的相同:`<`(小于），`>`（大于），`==`（等于），`<=`（小于或等于），`>=`（大于或等于）和`!=`（不等于）。

+ The body of the loop is indented: indentation is Python’s way of grouping statements. At the interactive prompt, you have to type a tab or space(s) for each indented line. In practice you will prepare more complicated input for Python with a text editor; all decent text editors have an auto-indent facility. When a compound statement is entered interactively, it must be followed by a blank line to indicate completion (since the parser cannot guess when you have typed the last line). Note that each line within a basic block must be indented by the same amount.

+ The [print()](https://docs.python.org/3.6/library/functions.html#print) function writes the value of the argument(s) it is given. It differs from just writing the expression you want to write (as we did earlier in the calculator examples) in the way it handles multiple arguments, floating point quantities, and strings. Strings are printed without quotes, and a space is inserted between items, so you can format things nicely, like this:

        >>> i = 256*256
        >>> print('The value of i is', i)
        The value of i is 65536

    The keyword argument end can be used to avoid the newline after the output, or end the output with a different string:
    
        >>> a, b = 0, 1
        >>> while b < 1000:
        ...     print(b, end=',')
        ...     a, b = b, a+b
        ...
        1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,

## 个人笔记

<h4>1. Numbers 数字类型（不可变对象immutable）</h4>

1. 内置数字类型：整型`int`表示整数，浮点型`float`表示带小数部分的数，布尔型`bool`表示逻辑真假，复数`complex`表示复数（后缀`j`或`J`表示虚部）；
2. 拓展数字类型：十进制浮点型`Decimal`和有理数`Fraction`；
3. 数字类型运算：
    + 运算符：`+`,`-`,`*`,`/`,`//`,`%`,`**`;除法(`/`)返回浮点数（精确结果），地板除(`//`)返回整型（精确结果向下取整）；
    + 优先级：`**` > `+`(正) = `-`(负) > `*` = `/` = `//` > `%` > `+`(加) = `-` (减)；幂运算符的优先级比左操作数的单目运算符高，比右操作数的单目运算符低。
4. 在交互解释器中，最后打印的表达式的结果赋值给变量`_`，该变量应被用户视作只读，不可为其显示赋值。

<h4>2. Strings 字符串类型（不可变对象immutable, 序列对象Sequence）</h4>

1. `print()`调用`str()`输出结果，交互解释器调用`repr()`输出结果；
2. `str()`和`repr()`比较：
    + `repr()`:面向程序开发者，除了输出数据内容，还会输出数据类型信息，不转义；
    + `str()`:面向用户，以简洁格式输出数据内容，转义；
    + 输出：
    
        | 同 | Numbers, Lists, Tuples, Dicts |
        | --- | --- |
        | 异 | Strings |


3. Python中有3种字符串文字String literal:
    + 普通字符串：单引号`'...'`或双引号`"..."`括起；
    + 原始字符串：以`r`开头的字符串，例如`r'...'`，不转义。 
    + 多行字符串：三个单引号或双引号括起来。换行符自动包含在字符串每行中，但可以通过在行尾加反斜线`\`来去除换行符；
    + 两个或多个相邻字符串文字会自动连接，过长的字符串文字可以如下跨行：
    
            ('...'
            '....')

4. 字符串特性：
    1. 正负索引，普通索引超过范围会报错；
    2. 切片
            
            a. 始终包含开头，不包含结束，即s[i:] + s[:i] = s；
            b. 第一个索引默认为0，第二个索引默认为序列长度；
            c. 切片索引超出范围不会报错
            d. 所有索引操作返回都是包含所请求元素的原序列的浅拷贝。
    
    4. 重载`+`, `*`;
    5. 内置方法：`len()`...

<h4>3. Lists 列表类型（可变对象mutable, 序列对象Sequence)</h4>

1. 列表特性：同上。




