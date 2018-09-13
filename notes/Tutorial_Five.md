# Python 教程（五） -- 更多控制流工具

## 原文链接

[4. More Control Flow Tools](https://docs.python.org/3.6/tutorial/controlflow.html)

## 原文翻译

<h3 id='4.'>4. 更多控制流工具</h3>

除了刚刚介绍的[while](https://docs.python.org/3.6/reference/compound_stmts.html#while)语句，Python拥有其他语言中常见的控制流语句，并且有一些曲折。

<h4 id='4.1.'>4.1. if 语句</h4>

也许最广为人知的语句类型就是[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)语句，例如：

    >>> x = int(input("Please enter an integer: "))
    Please enter an integer: 42
    >>> if x < 0:
    ...     x = 0
    ...     print('Negative changed to zero')
    ... elif x == 0:
    ...     print('Zero')
    ... elif x == 1:
    ...     print('Single')
    ... else:
    ...     print('More')
    ...
    More

if语句可以有零个或多个[elif](https://docs.python.org/3.6/reference/compound_stmts.html#elif)部分，[else](https://docs.python.org/3.6/reference/compound_stmts.html#else)也是可选的。关键字'[elif](https://docs.python.org/3.6/reference/compound_stmts.html#elif)'是'else if'的缩写，这有助于避免过度缩进（elif刚好对应4个空格缩进）。一个 [if](https://docs.python.org/3.6/reference/compound_stmts.html#if)... ... [elif](https://docs.python.org/3.6/reference/compound_stmts.html#elif)... ... [elif](https://docs.python.org/3.6/reference/compound_stmts.html#elif)... ...序列用来替代其它语言中的`switch`或`case`语句。

<h4 id='4.2.'>4.2. for 语句</h4>

Python中的[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)语句与在C或Pascal中的[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)语句略有不同。Python的[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)语句按序列中元素出现的顺序迭代任意序列对象（列表或者字符串）中的元素，而不是像Pascal语言中总是迭代数字的等差数列arithmetic progression，或者像C语言中允许用户定义迭代步进iteration step和终止条件halting condition。例如：

    >>> # Measure some strings:
    ... words = ['cat', 'window', 'defenestrate']
    >>> for w in words:
    ...     print(w, len(w))
    ...
    cat 3
    window 6
    defenestrate 12

如果你需要修改循环中正在迭代的序列对象（例如复制所选元素），建议你先复制一份序列对象。迭代序列对象并不会自动复制序列对象。切片表示法可以十分方便的复制序列对象：

    >>> for w in words[:]:  # Loop over a slice copy of the entire list.
    ...     if len(w) > 6:
    ...         words.insert(0, w)
    ...
    >>> words
    ['defenestrate', 'cat', 'window', 'defenestrate']
    
如果使用`for w in words:`，该例子会创建一个无限长的列表，不断的往原序列中插入`defenestrate`。    

<h4 id='4.3.'>4.3. range() 函数</h4>

如果你需要迭代一系列数字，内置函数[range()](https://docs.python.org/3.6/library/stdtypes.html#range)就派上用场了。它生成等差数列：

    >>> for i in range(5):
    ...     print(i)
    ...
    0
    1
    2
    3
    4

`range`生成的序列不包含给定的终点；`range(10)`生成10个值，这10个值是一个长度为10的序列对象元素的合法索引值。可以让`range`生成的序列从另一个数字开始，或者指定不同的增量（甚至是负数;有时这称为“步进”）：

    range(5, 10)
       5, 6, 7, 8, 9

    range(0, 10, 3)
       0, 3, 6, 9

    range(-10, -100, -30)
      -10, -40, -70

可以组合[range()](https://docs.python.org/3.6/library/stdtypes.html#range)和[len()](https://docs.python.org/3.6/library/functions.html#len)要遍历序列的索引，如下：

    >>> a = ['Mary', 'had', 'a', 'little', 'lamb']
    >>> for i in range(len(a)):
    ...     print(i, a[i])
    ...
    0 Mary
    1 had
    2 a
    3 little
    4 lamb
    
但是，在大多数此类情况下，使用[enumerate()](https://docs.python.org/3.6/library/functions.html#enumerate) 函数很方便，请参阅[循环技巧](https://docs.python.org/3.6/tutorial/datastructures.html#tut-loopidioms)。

如果你输出一个`range`，就会发生一件奇怪的事：

    >>> print(range(10))
    range(0, 10)
    
在许多方面，[range()](https://docs.python.org/3.6/library/stdtypes.html#range)返回的对象表现得很像是一个列表，但事实上并非是一个列表。它是一个对象，只有当您迭代它时，它才连续地返回所需序列的元素，但它并不创建列表，从而节省空间。

这样的对象称作可迭代对象`iterable`，也就是说，可迭代对象适合用于某些函数和结构（比如`for`循环），这些函数和结构期望能够连续地获得序列元素直到序列被耗尽。我们已经看到[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)语句就是这样的迭代器（结构）。函数[list()](https://docs.python.org/3.6/library/stdtypes.html#list) 是另一种这样的函数，它从可迭代对象中创建列表：

    >>> list(range(5))
    [0, 1, 2, 3, 4]

稍后我们将看到更多返回iterables的函数，以及将iterables作为参数的函数。

<h4 id='4.4.'>4.4. break和continue语句，以及循环的else从句</h4>

Python的break语句，类似于C中的，结束（跳出）最内层的 [for](https://docs.python.org/3.6/reference/compound_stmts.html#for)或[while](https://docs.python.org/3.6/reference/compound_stmts.html#while)循环。

Python的循环语句可以有一个else从句; 当循环因为列表被耗尽（使用[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)循环时）或着因为循环条件变为false（使用[while](https://docs.python.org/3.6/reference/compound_stmts.html#while)循环时）而终止，而不是被break语句而终止时，则执行`else`从句。通过以下搜索素数的循环来举例说明：

    >>> for n in range(2, 10):
    ...     for x in range(2, n):
    ...         if n % x == 0:
    ...             print(n, 'equals', x, '*', n//x)
    ...             break
    ...     else:
    ...         # loop fell through without finding a factor
    ...         print(n, 'is a prime number')
    ...
    2 is a prime number
    3 is a prime number
    4 equals 2 * 2
    5 is a prime number
    6 equals 2 * 3
    7 is a prime number
    8 equals 2 * 4
    9 equals 3 * 3

（是的，这是正确的代码，仔细一看：该`else`从句属于[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)循环，而不属于[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)语句）

比起[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)语句中的`else`部分，循环的`else`从句更类似于[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句中`else`从句：[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句中的`else`从句只有在没有异常发生时被执行，循环的`else`从句只有在没有`beak`发生时被执行。关于[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句和异常的更多内容，请参考[异常处理](https://docs.python.org/3.6/tutorial/errors.html#tut-handling)。

也类似于C中，Python的[continue](https://docs.python.org/3.6/reference/simple_stmts.html#continue)语句跳过当前循环的后续语句,执行循环的下一次迭代：

    >>> for num in range(2, 10):
    ...     if num % 2 == 0:
    ...         print("Found an even number", num)
    ...         continue
    ...     print("Found a number", num)
    Found an even number 2
    Found a number 3
    Found an even number 4
    Found a number 5
    Found an even number 6
    Found a number 7
    Found an even number 8
    Found a number 9

<h4 id='4.5.'>4.5. pass 语句</h4>

[pass](https://docs.python.org/3.6/reference/simple_stmts.html#pass)语句什么都不做，可用于当语法上需要一个语句但是程序不需要执行操作时。例如：

    >>> while True:
    ...     pass  # Busy-wait for keyboard interrupt (Ctrl+C)
    ...
    
`pass`常用于创建最小类：

    >>> class MyEmptyClass:
    ...     pass
    ...
    
使用[pass](https://docs.python.org/3.6/reference/simple_stmts.html#pass)另一个地方就是当您处理新代码时，使用`pass`作为函数体或条件体的占位符，这允许您在更抽象的层次上继续思考。[pass](https://docs.python.org/3.6/reference/simple_stmts.html#pass)将会被自动忽略：

    >>> def initlog(*args):
    ...     pass   # Remember to implement this!
    ...

<h4 id='4.6.'>4.6. 定义函数</h4>

我们可以创建一个将Fibonacci数列输出到任意边界的函数：

    >>> def fib(n):    # write Fibonacci series up to n
    ...     """Print a Fibonacci series up to n."""
    ...     a, b = 0, 1
    ...     while a < n:
    ...         print(a, end=' ')
    ...         a, b = b, a+b
    ...     print()
    ...
    >>> # Now call the function we just defined:
    ... fib(2000)
    0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597

关键字[def](https://docs.python.org/3.6/reference/compound_stmts.html#def)引入了一个函数定义。它必须后跟函数名称和带括号的形式参数列表。构成函数体的语句从下一行开始，并且必须缩进。

函数体的第一个语句可以选择是字符串文字; 此字符串文字是函数的文档字符串或docstring。（有关文档字符串的更多信息，请参阅[文档字符串](https://docs.python.org/3.6/tutorial/controlflow.html#tut-docstrings)部分。）有些工具使用文档字符串自动生成在线文档或输出文档，或者让用户以交互方式浏览代码; 在编写的代码中包含文档字符串是一种很好的做法，所以要养成写文档字符串的习惯。

函数的*执行*引入了用于函数局部变量的新符号表。更准确地说，所有在函数中赋值的变量都将值存储在本地（局部）符号表中; 而变量引用首先在本地（局部）符号表中查找，再依次在外层函数的本地（局部）符号表中查找，然后在全局符号表中查找，最后在内置名称built-in names中查找。因此，尽管可以在函数中可以读取全局变量的值，但是全局变量不能直接在函数内赋值（除非使用global语句）。

当函数被调用时，调用函数的实参被引入到被调函数的本地（局部）符号表。所以，参数是使用call by value（value是对象引用，而不是对象的值）传递给函数的。当函数调用另一个函数时，将为该调用创建一个新的本地（局部）符号表。

函数定义在当前符号表中引入函数名称。函数名称的值具有一种类型，这种类型被解释器识别为用户自定义函数。函数名的值可以赋值给其他变量，这个变量也可以用作函数，这是一般的重命名机制。

    >>> fib
    <function fib at 10042ed0>
    >>> f = fib
    >>> f(100)
    0 1 1 2 3 5 8 13 21 34 55 89

当你刚开始从其他语言转向python语言，你可能会反对`fib`是一个函数，反而是一个过程，因为它不返回值。实际上，即使是没有[return](https://docs.python.org/3.6/reference/simple_stmts.html#return)语句的函数也会返回一个值，尽管它是一个相当无聊的值。这个值是`None`（它是内置名称built-in name）。如果单独写入`None`，则解释器通常会禁止写入值。如果你真的想对它使用[print()](https://docs.python.org/3.6/library/functions.html#print)，你可以看到它：

    >>> fib(0)
    >>> print(fib(0))
    None
    
编写一个返回而不是打印Fibonacci数列的函数，十分简单：

    >>> def fib2(n):  # return Fibonacci series up to n
    ...     """Return a list containing the Fibonacci series up to n."""
    ...     result = []
    ...     a, b = 0, 1
    ...     while a < n:
    ...         result.append(a)    # see below
    ...         a, b = b, a+b
    ...     return result
    ...
    >>> f100 = fib2(100)    # call it
    >>> f100                # write the result
    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
    
此示例演示了一些新的Python功能：

+ [return]()语句从函数返回值。 没有表达式参数[return]()语句返回`None`；
+ `result.append(a)`语句调用列表对象`result`的方法。方法是“属于”对象并被命名为`obj.methodname`的函数 ，其中`obj`是某个对象（可能是表达式），并且methodname是由对象的类型定义的方法的名称。不同类型定义不同的方法。不同类型的方法可以具有相同的名称而不会引起歧义。（可以使用类定义自己的对象类型和方法，请参阅[类](https://docs.python.org/3.6/tutorial/classes.html#tut-classes)）示例中显示的方法`append()`是为列表对象定义的; 它在列表的末尾添加了一个新元素。在这个例子中它相当于`result = result + [a]`，但效率更高。
    

<h4 id='4.7.'>4.7. 更多定义函数</h4>

也可以定义具有可变数量的参数的函数。有三种形式可以组合。

<h4 id='4.7.1.'>4.7.1. 默认参数值</h4>

最有用的形式是为一个或多个参数指定默认值。这创建了一个可以使用比定义需要的参数更少的参数调用的函数，例如：

    def ask_ok(prompt, retries=4, reminder='Please try again!'):
        while True:
            ok = input(prompt)
            if ok in ('y', 'ye', 'yes'):
                return True
            if ok in ('n', 'no', 'nop', 'nope'):
                return False
            retries = retries - 1
            if retries < 0:
                raise ValueError('invalid user response')
            print(reminder)

可以通过多种方式调用此函数：

+ 只给出强制性参数： `ask_ok('Do you really want to quit?')`
+ 给出一个可选参数： `ask_ok('OK to overwrite the file?', 2)`
+ 甚至给出所有参数： `ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')`

此示例还介绍了[in](https://docs.python.org/3.6/reference/expressions.html#in)关键字。[in](https://docs.python.org/3.6/reference/expressions.html#in)测试序列是否包含某个值。

默认值是在定义作用域中的函数定义处进行计算的，所以

    i = 5

    def f(arg=i):
        print(arg)

    i = 6
    f()

将打印`5`。

**重要警告：** 默认值仅被计算一次。但是当默认值是可变对象（例如列表，字典或大多数类的实例）时，这会有所不同。例如，以下函数会累积在后续调用中传递给它的参数：

    def f(a, L=[]):
        L.append(a)
        return L

    print(f(1))
    print(f(2))
    print(f(3))
    
这将输出

    [1]
    [1, 2]
    [1, 2, 3]
    
如果您不希望在后续调用中共享默认值，则可以编写如下函数：

    def f(a, L=None):
        if L is None:
            L = []
        L.append(a)
        return L


<h4 id='4.7.2.'>4.7.2. 关键字参数</h4>

可以使用形如`kwarg=value`的[关键字参数](https://docs.python.org/3.6/glossary.html#term-keyword-argument)来调用函数。例如下述函数：

    def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
        print("-- This parrot wouldn't", action, end=' ')
        print("if you put", voltage, "volts through it.")
        print("-- Lovely plumage, the", type)
        print("-- It's", state, "!")

接受一个必需参数（`voltage`）和三个可选参数（`state`，`action`，和`type`）。可以通过以下任何方式调用此函数：

    parrot(1000)                                          # 1 positional argument
    parrot(voltage=1000)                                  # 1 keyword argument
    parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
    parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
    parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
    parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
    
但以下所有调用都将无效：

    parrot()                     # required argument missing
    parrot(voltage=5.0, 'dead')  # non-keyword argument after a keyword argument
    parrot(110, voltage=220)     # duplicate value for the same argument
    parrot(actor='John Cleese')  # unknown keyword argument
    
在函数调用中，关键字参数必须跟着位置参数（出现在位置参数之后）。传递的所有关键字参数必须与函数接受的参数之一匹配（例如`actor`不是函数`parrot`的有效参数 ），并且关键字参数的顺序并不重要。这还包括非可选参数（位置参数也可以使用关键字参数形式传递，此时顺序不重要）（例如parrot(voltage=1000)也是有效的）。没有参数可以多次赋值。以下就是多次赋值而失败的示例：

    >>> def function(a):
    ...     pass
    ...
    >>> function(0, a=0)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: function() got multiple values for keyword argument 'a'
    
当形如`**name`的最终形式参数出现时，它接收除去那些形式参数中定义的关键字参数的剩余所有关键字参数的字典（参见[映射类型 - 字典](https://docs.python.org/3.6/library/stdtypes.html#typesmapping)）。这可以与形如`*name`的形式参数（将在下个子节介绍）组合使用，可变参数接受除去形参列表中定义的位置参数的剩余所有位置参数的元组。（`*name`必须定义在`**name`之前）。例如，如果我们定义一个这样的函数：

    def cheeseshop(kind, *arguments, **keywords):
        print("-- Do you have any", kind, "?")
        print("-- I'm sorry, we're all out of", kind)
        for arg in arguments:
            print(arg)
        print("-" * 40)
        for kw in keywords:
            print(kw, ":", keywords[kw])

它可以像这样调用：

    cheeseshop("Limburger", "It's very runny, sir.",
               "It's really very, VERY runny, sir.",
               shopkeeper="Michael Palin",
               client="John Cleese",
               sketch="Cheese Shop Sketch")

当然它会打印：

    -- Do you have any Limburger ?
    -- I'm sorry, we're all out of Limburger
    It's very runny, sir.
    It's really very, VERY runny, sir.
    ----------------------------------------
    shopkeeper : Michael Palin
    client : John Cleese
    sketch : Cheese Shop Sketch

请注意，打印关键字参数的顺序与函数调用中提供它们的顺序相匹配。

<h4 id='4.7.3.'>4.7.3. 任意参数列表</h4>

最后，最不常用的选项是指定一个可以使用任意数量的参数调用函数。这些参数将被组装在一个元组中（参见[元组和序列](https://docs.python.org/3.6/tutorial/datastructures.html#tut-tuples)）。在可变参数之前，可以有零个或多个正常参数（位置参数）。

通常，这些可变参数将在位置参数列表中排在最后，因为它们会挖掘传递给函数的所有剩余输入位置参数。在`*args`参数之后出现的任何形式参数都是“仅关键字”参数，这意味着它们只能是关键字参数而不是位置参数。

    >>> def concat(*args, sep="/"):
    ...     return sep.join(args)
    ...
    >>> concat("earth", "mars", "venus")
    'earth/mars/venus'
    >>> concat("earth", "mars", "venus", sep=".")
    'earth.mars.venus'

<h4 id='4.7.4.'>4.7.4. 解压参数列表</h4>

当参数已经在一个列表或元组中，为了获得函数调用所必需的位置参数，此时必须解压参数列表。例如，内置[range()](https://docs.python.org/3.6/library/stdtypes.html#range)函数需要单独的 `start`和`stop`参数。如果它们不是独立各自传入的，请使用*-operator调用函数以从列表或元组中解压参数：

    >>> list(range(3, 6))            # normal call with separate arguments
    [3, 4, 5]
    >>> args = [3, 6]
    >>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]

以相同的方式，**-operator可以接收字典来提供关键字参数：

    >>> def parrot(voltage, state='a stiff', action='voom'):
    ...     print("-- This parrot wouldn't", action, end=' ')
    ...     print("if you put", voltage, "volts through it.", end=' ')
    ...     print("E's", state, "!")
    ...
    >>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
    >>> parrot(**d)
    -- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !

<h4 id='4.7.5.'>4.7.5. Lambda 表达式</h4>

可以使用[lambda]()关键字创建小的匿名函数。函数`lambda a, b: a+b`返回其两个参数的总和。Lambda函数可以在任何需要函数对象的地方使用。在语法上,Lambda函数仅仅是单个表达式。从语义上讲，Lambda只是正常函数定义的语法糖。与嵌套函数定义一样，lambda函数可以引用包含范围的变量：

    >>> def make_incrementor(n):
    ...     return lambda x: x + n
    ...
    >>> f = make_incrementor(42)
    >>> f(0)
    42
    >>> f(1)
    43

上面的示例使用lambda表达式返回一个函数。另一个用途是传递一个小函数作为参数：

    >>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
    >>> pairs.sort(key=lambda pair: pair[1])
    >>> pairs
    [(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]


<h4 id='4.7.6.'>4.7.6. 文档字符串</h4>

Here are some conventions about the content and formatting of documentation strings.

The first line should always be a short, concise summary of the object’s purpose. For brevity, it should not explicitly state the object’s name or type, since these are available by other means (except if the name happens to be a verb describing a function’s operation). This line should begin with a capital letter and end with a period.

If there are more lines in the documentation string, the second line should be blank, visually separating the summary from the rest of the description. The following lines should be one or more paragraphs describing the object’s calling conventions, its side effects, etc.

The Python parser does not strip indentation from multi-line string literals in Python, so tools that process documentation have to strip indentation if desired. This is done using the following convention. The first non-blank line after the first line of the string determines the amount of indentation for the entire documentation string. (We can’t use the first line since it is generally adjacent to the string’s opening quotes so its indentation is not apparent in the string literal.) Whitespace “equivalent” to this indentation is then stripped from the start of all lines of the string. Lines that are indented less should not occur, but if they occur all their leading whitespace should be stripped. Equivalence of whitespace should be tested after expansion of tabs (to 8 spaces, normally).

Here is an example of a multi-line docstring:
    
    >>> def my_function():
    ...     """Do nothing, but document it.
    ...
    ...     No, really, it doesn't do anything.
    ...     """
    ...     pass
    ...
    >>> print(my_function.__doc__)
    Do nothing, but document it.

        No, really, it doesn't do anything.

<h4 id='4.7.7.'>4.7.7. 函数注释</h4>

[函数注释](https://docs.python.org/3.6/reference/compound_stmts.html#function)是关于用户自定义函数所使用的数据类型的完全可选元数据信息（请参阅[PEP 3107](https://www.python.org/dev/peps/pep-3107)和[PEP 484](https://www.python.org/dev/peps/pep-0484)了解更多信息）。

注释以字典形式存储在函数的`__annotations__`属性中，对函数的任何其他部分没有影响。参数注释由参数名称后面的冒号定义，后跟一个表达式，用于评估注释的值。返回注释由->定义，位于参数列表和表示def语句结尾的冒号之间，后跟表达式。以下示例具有位置参数，关键字参数和注释的返回值：

    >>> def f(ham: str, eggs: str = 'eggs') -> str:
    ...     print("Annotations:", f.__annotations__)
    ...     print("Arguments:", ham, eggs)
    ...     return ham + ' and ' + eggs
    ...
    >>> f('spam')
    Annotations: {'ham': <class 'str'>, 'return': <class 'str'>, 'eggs': <class 'str'>}
    Arguments: spam eggs
    'spam and eggs'

<h4 id='4.8.'>4.8. 间奏曲:编码风格</h4>

既然您将要编写更长，更复杂的Python代码，现在是讨论编码风格的好时机。大多数语言都可以用不同的风格编写（或更简洁，格式化）; 有些风格比其他风格更具可读性。让其他人轻松阅读您的代码总是一个好主意，采用一种好的编码风格对此有很大帮助。

对于Python， [PEP 8](https://www.python.org/dev/peps/pep-0008)已成为大多数项目坚持的风格指南; 它提倡了一种非常易读且令人赏心悦目的编码风格。每个Python开发人员都应该在某个时候阅读它; 以下是为您提取的最重要的要点：

+ 使用4空格缩进，不要使用制表符Tab。
    
    4个空格是小缩进（允许更大的嵌套深度）和大缩进（更容易阅读）之间的良好折衷。制表符Tab引入混淆，最好不用。
    
+ 换行，使其不超过79个字符。
    
    这有助于用户使用小型显示器，并且可以在较大的显示器上并排放置和显示多个代码文件。

+ 使用空行分隔函数和类，以及函数内的较大代码块。
+ 如果可能，在代码同一行上写注释。
+ 使用文档字符串。
+ 在操作符周围和逗号后面使用空格，但不要直接在括号中使用：`a = f(1, 2) + g(3, 4)`。
+ 一致地命名您的类和函数; 约定`CamelCase`用于类,`lower_case_with_underscores`用于函数和方法。始终使用self作为方法的第一个参数（有关类和方法的更多信息，请参阅[A First Look at Classes](https://docs.python.org/3.6/tutorial/classes.html#tut-firstclasses)）。
+ 如果您的代码旨在用于国际环境，请不要使用花哨的编码。Python的默认值，UTF-8甚至纯ASCII在任何情况下都能最好地工作.
+ 同样，如果可能存在使用不同语言的人阅读或维护代码的可能，请不要在标识符中使用非ASCII字符。


## 个人笔记

<h4>1. if 语句</h4>

1. if语句有0个或多个elif从句，有0个或1个else从句；
2. elif对应4个空格缩进，有助于避免过度缩进；
3. if...elif...elif...语句对应其他语言中的switch...case...语句。

<h4>2. for 语句</h4>

若需要修改循环中正在迭代的序列对象，最好先复制一份序列对象，切片可以十分方便地复制序列对象(`a[:]`)；

栗子：

    >>> # Measure some strings:
    ... words = ['cat', 'window', 'defenestrate']
    >>> for w in words:
    ...     print(w, len(w))
    ...
    cat 3
    window 6
    defenestrate 12

<h4>3. range() 函数</h4>

1. range函数返回可迭代对象`Iterable`，而不是列表(`print`试试)；
2. 可迭代对象只有在被迭代时，才会连续按序地返回序列中的元素，不创建列表，节省空间；

<h4>4. break、continue以及循环的else从句</h4>

1. `break`语句：跳出最内层循环；
2. `continue`语句：跳过当前循环的后续语句，执行下一次循环；
3. 循环else从句：当循环被耗尽(for)或循环条件变为`false`(while)而终止，而不是被`break`语句而终止时，执行循环的`else`从句

<h4>5. pass 语句</h4>

常用于：
1. 构建最小类（类内容为`pass`）；
2. 用作函数体、方法体、条件体的占位符；

<h4>6. 函数定义</h4>

1. 函数被调用时，解释器会创建用于函数局部变量的新符号表；调用函数的实参被引入到被调函数的本地（局部）符号表；所有在函数中赋值的变量都将值存储在函数的本地（局部）符号表中；
2. 函数局部变量引用顺序：本地符号表 -> 外层函数的本地符号表 -> 全局符号表 -> 内置名称built-in；
3. 可以在函数中引用全局变量的值，但不可以在函数中为全局变量赋值（除非使用`global`语句）；
4. 函数定义在当前符号表中引入函数名称，函数名称是一种用户自定义函数的类型（类型对象，Python中类型也是一种对象），函数名称可以赋值给其他变量；
5. 函数参数：
    + 形参(parameters, formal arguments):函数定义时的参数；
        1. 位置或关键字形参(positional or keyword): 函数调用时，既可以传递位置实参，又可以传递关键字实参；
            
                def func(foo, bar=None): ...
            
        2. 位置形参(positional-only): 函数调用时，只能传递位置实参；Python未指定positional-only语法。但是，一些内嵌函数使用位置形参（例如，[abs()](https://docs.python.org/3.6/library/functions.html#abs)）；
        3. 命名关键字形参(keyword-only): 函数调用时，只能传递关键字实参，位于可变位置形参或者`*`符号之后；

                def func(arg, *, 
kw_only1, kw_only2): ...
        
        4. 可变位置形参(var-positional): 函数调用时，传递任意数量的位置实参，除了那些位置形参(positional-only)已经指定的参数，形如`*args`；

                def func(*args, **kwargs): ...
        
        5. 可变关键字形参(var-keyword): 函数调用时，传递任意数量的关键字实参，除了那些命名关键字形参(keyword-only)指定的参数，形如`**kw`。
                
    + 实参(arguments): 函数调用时的参数；
        1. 关键字实参(keyword argument)：使用关键字传入或者`**`开头的字典传入的参数值；
        2. 位置实参(positional argument): 使用位置传入或者`*`开头的可迭代对象传入的参数值；
    
    + 函数参数默认值是在函数定义处进行计算的，默认值仅计算一次，但是当默认值是可变对象（例如列表，字典或大多数类的实例）时，这会有所不同。

6. lambda表达式：
    用途：
    1. 饭回一个函数；
    2. 传递一个小函数作为参数。

7. 函数注释：
    1. 参数注释：参数名称 + `:` + 类型；
    2. 返回注释：参数列表 + `->` + 类型；
    3. 函数注释以字典形式存储在函数`__annotations__`属性。

        