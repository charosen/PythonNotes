# Python 教程(九) -- 错误和异常

## 原文链接

[8. Errors and Exceptions](https://docs.python.org/3.6/tutorial/errors.html)

## 原文翻译

<h3 id='8.'>8. 错误和异常</h3>

到目前为止，教程还没有提到错误消息，但是如果你已经尝试过教程的例子，你可能已经看过了一些错误信息。存在（至少）两种可区分的错误：*语法错误* 和*异常*。

<h4 id='8.1.'>8.1. 语法错误Syntax Errors</h4>

语法错误（也称为解析错误）可能是您在学习Python时最常见的错误：

    >>> while True print('Hello world')
      File "<stdin>", line 1
        while True print('Hello world')
                       ^
    SyntaxError: invalid syntax

解析器重复输出违规行（错误行），并显示一个“箭头”，该箭头指向行中检测到错误的最早点。该错误是由箭头前面的标记引起的（或至少在箭头前面的标记处检测到的）：在该示例中，在函数`print()`处检测到错误，因为在它之前缺少冒号（`':'`）。文件名和行号将一同被输出，以便在输入脚本中查找错误位置。

<h4 id='8.2.'>8.2. 异常Exceptions</h4>

即使语句或表达式在语法上是正确的，但在尝试执行它时仍可能会导致错误。在执行期间检测到的错误称为异常，并且异常不是无条件致命的：您将很快学会如何在Python程序中处理它们。但是，程序没有处理大多数异常，并导致如下错误消息：

    >>> 10 * (1/0)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ZeroDivisionError: division by zero
    >>> 4 + spam*3
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'spam' is not defined
    >>> '2' + 2
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: Can't convert 'int' object to str implicitly

错误消息的最后一行说明发生了什么。异常有不同的类型，类型作为错误消息的一部分打印出来：示例中的类型是[ZeroDivisionError](https://docs.python.org/3.6/library/exceptions.html#ZeroDivisionError)，[NameError](https://docs.python.org/3.6/library/exceptions.html#NameError)和[TypeError](https://docs.python.org/3.6/library/exceptions.html#TypeError)。作为异常类型打印的字符串是发生的内置异常的名称。对于所有内置异常都是如此，但对于用户自定义的异常不一定是这样（尽管它是一个有用的约定）。标准异常名称是内置标识符（不是保留关键字）。

该行的其余部分根据异常类型及其原因提供详细信息。

错误消息的前一部分以堆栈回溯的形式显示发生异常的上下文。通常它包含列出源代码行的堆栈回溯; 但是，它不会显示从标准输入读取的行。

[Built-in Exceptions](https://docs.python.org/3.6/library/exceptions.html#bltin-exceptions)列出了内置异常及其含义。

<h4 id='8.3.'>8.3. 异常处理Handling Exceptions</h4>

可以编写处理所选异常的程序。请查看以下示例，该示例要求用户不断输入，直到输入有效整数，但允许用户中断程序（使用`Control-C`或操作系统支持的任何内容）; 请注意，通过引发[KeyboardInterrupt](https://docs.python.org/3.6/library/exceptions.html#KeyboardInterrupt)异常来发出用户生成中断的中断信号。

    >>> while True:
    ...     try:
    ...         x = int(input("Please enter a number: "))
    ...         break
    ...     except ValueError:
    ...         print("Oops!  That was no valid number.  Try again...")
    ...

[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句的工作原理如下。

+ 首先，执行*try*子句（[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)和[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)关键字之间的语句）。
+ 如果没有发生异常，则跳过*except*子句并完成[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句的执行。
+ 如果在执行*try*子句期间发生异常，则跳过*try*子句的后续部分。然后，如果异常类型与*except*子句中指定的异常匹配，则执行*except*子句，然后继续执行*try*语句（不是子句）的后续语句。
+ 如果发生的异常与*except*子句中指定的异常不匹配，则将其传递给外部*try*语句; 如果没有找到处理程序，则它是一个未处理的异常，程序将停止执行并显示如上所示的错误消息。

一个[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句可能有不止一个*except*子句，每一个*except*子句为不同的异常指定异常处理程序。最多执行一个异常处理程序。异常处理程序仅处理相应try子句中发生的异常，且，不处理同一[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句的其他异常处理程序中的异常。*except*子句可以将多个异常命名为带括号的元组(一个*except*子句同时处理多个异常)，例如：

    ... except (RuntimeError, TypeError, NameError):
    ...     pass

[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)子句会捕获其指定的异常类及该异常类的派生类。（反之不行，[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)子句不会捕获其指定的异常类的父类）例如，以下代码将按顺序打印B，C，D：

    class B(Exception):
        pass
    
    class C(B):
        pass
    
    class D(C):
        pass
    
    for cls in [B, C, D]:
        try:
            raise cls()
        except D:
            print("D")
        except C:
            print("C")
        except B:
            print("B")

请注意，如果*except*子句被颠倒（第一个*except*子句是`except B`），它将打印B，B，B - 触发第一个匹配的*except*子句。

最后一个*except*子句可以省略异常名称，以用作通配符（匹配所有异常）。请谨慎使用，因为以这种方式很容易掩盖真正的编程错误！它还可以用于打印错误消息，然后重新抛出异常（允许调用者也处理异常）：

    import sys
    
    try:
        f = open('myfile.txt')
        s = f.readline()
        i = int(s.strip())
    except OSError as err:
        print("OS error: {0}".format(err))
    except ValueError:
        print("Could not convert data to an integer.")
    except:
        print("Unexpected error:", sys.exc_info()[0])
        raise

[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)...[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)语句有一个可选的*else*子句，*else*子句必须在所有*except*子句之后。*else*子句对于必须在*try*子句不引发异常时才执行的代码很有用。例如：

    for arg in sys.argv[1:]:
        try:
            f = open(arg, 'r')
        except OSError:
            print('cannot open', arg)
        else:
            print(arg, 'has', len(f.readlines()), 'lines')
            f.close()

使用[else](https://docs.python.org/3.6/reference/compound_stmts.html#else)子句比向[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)子句添加其他代码更好，因为[else](https://docs.python.org/3.6/reference/compound_stmts.html#else)子句避免意外捕获并非由[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)...[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)语句保护的代码引发的异常。

当异常发生时，异常可能具有关联值，也称为异常参数。异常参数的存在和类型取决于异常类型。

*except*子句可以在异常名称后面指定一个变量。该变量绑定到*except*指定的异常实例，异常实例带有参数，可以通过`instance.args`来访问异常实例的异常参数。为方便起见，异常实例定义了`__str__()`方法，所以异常参数可以直接打印而无需通过`.args`引用。用户也可以在抛出异常之前首先实例化异常，并根据需要向异常实例添加任何属性。

    >>> try:
    ...     raise Exception('spam', 'eggs')
    ... except Exception as inst:
    ...     print(type(inst))    # the exception instance
    ...     print(inst.args)     # arguments stored in .args
    ...     print(inst)          # __str__ allows args to be printed directly,
    ...                          # but may be overridden in exception subclasses
    ...     x, y = inst.args     # unpack args
    ...     print('x =', x)
    ...     print('y =', y)
    ...
    <class 'Exception'>
    ('spam', 'eggs')
    ('spam', 'eggs')
    x = spam
    y = eggs

如果异常有参数，则它们将作为未处理异常的消息的最后一部分（“详细信息”）打印。

异常处理程序不仅处理直接发生在*try*子句中异常，而且还处理try子句调用（甚至间接）的函数内部发生的异常。例如：

    >>> def this_fails():
    ...     x = 1/0
    ...
    >>> try:
    ...     this_fails()
    ... except ZeroDivisionError as err:
    ...     print('Handling run-time error:', err)
    ...
    Handling run-time error: division by zero

<h4 id='8.4.'>8.4. 抛出异常Raising Exceptions</h4>

[raise](https://docs.python.org/3.6/reference/simple_stmts.html#raise)语句允许程序员强制引发指定的异常。例如：

    >>> raise NameError('HiThere')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: HiThere

[raise](https://docs.python.org/3.6/reference/simple_stmts.html#raise)语句唯一的参数是要抛出的异常。这必须是异常实例或异常类（派生自[Exception](https://docs.python.org/3.6/library/exceptions.html#Exception)的类）。如果传递了一个异常类，它将通过调用没有参数的构造函数来隐式实例化：

    raise ValueError  # shorthand for 'raise ValueError()'

如果您需要确定是否发生了异常但不打算处理它，则更简单的[raise](https://docs.python.org/3.6/reference/simple_stmts.html#raise)语句形式允许您重新抛出异常：

    >>> try:
    ...     raise NameError('HiThere')
    ... except NameError:
    ...     print('An exception flew by!')
    ...     raise
    ...
    An exception flew by!
    Traceback (most recent call last):
      File "<stdin>", line 2, in <module>
    NameError: HiThere

<h4 id='8.5.'>8.5. 用户自定义异常User-defined Exceptions</h4>

程序可以通过创建新的异常类来定义它们自己的异常（有关Python类的更多信息，请参阅[class](https://docs.python.org/3.6/tutorial/classes.html#tut-classes)）。通常应直接或间接地从[Exception](https://docs.python.org/3.6/library/exceptions.html#Exception)类中派生异常。

可以定义异常类，异常类可以执行任何其他类可以执行的任何操作，但通常要保持简单，通常只提供一些允许异常处理程序提取有关错误的信息的属性。在创建可能抛出多个不同错误的模块时，通常的做法是为该模块定义的异常创建基类，并为不同错误条件创建特定异常子类：

    class Error(Exception):
        """Base class for exceptions in this module."""
        pass
    
    class InputError(Error):
        """Exception raised for errors in the input.
    
        Attributes:
            expression -- input expression in which the error occurred
            message -- explanation of the error
        """
    
        def __init__(self, expression, message):
            self.expression = expression
            self.message = message
    
    class TransitionError(Error):
        """Raised when an operation attempts a state transition that's not
        allowed.
    
        Attributes:
            previous -- state at beginning of transition
            next -- attempted new state
            message -- explanation of why the specific transition is not allowed
        """
    
        def __init__(self, previous, next, message):
            self.previous = previous
            self.next = next
            self.message = message

大多数用户自定义异常都以“Error”为命名结尾，类似于标准异常的命名。

<h4 id='8.6.'>8.6. 定义清理操作Defining Clean-up Actions</h4>

[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句有另一个可选子句[finally](https://docs.python.org/3.6/reference/compound_stmts.html#finally)，该子句用于定义在所有情况下都必须执行的清理操作。例如：

    >>> try:
    ...     raise KeyboardInterrupt
    ... finally:
    ...     print('Goodbye, world!')
    ...
    Goodbye, world!
    KeyboardInterrupt
    Traceback (most recent call last):
      File "<stdin>", line 2, in <module>

无论是否发生异常，在结束[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句的时候永远会执行*finally*子句。当异常发生在[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)子句中且未被[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)子句处理 （或者异常发生在[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)或[else](https://docs.python.org/3.6/reference/compound_stmts.html#else)子句中）时，异常会在[finally](https://docs.python.org/3.6/reference/compound_stmts.html#finally)子句执行后被重新抛出，而不是先抛出异常而不执行*finally*子句。当[try](https://docs.python.org/3.6/reference/compound_stmts.html#try)语句的任意其他子句使用[break](https://docs.python.org/3.6/reference/simple_stmts.html#break), [continue](https://docs.python.org/3.6/reference/simple_stmts.html#continue)或者[return](https://docs.python.org/3.6/reference/simple_stmts.html#return)语句退出时，*finally*子句也会被执行。一个更复杂的例子：

    >>> def divide(x, y):
    ...     try:
    ...         result = x / y
    ...     except ZeroDivisionError:
    ...         print("division by zero!")
    ...     else:
    ...         print("result is", result)
    ...     finally:
    ...         print("executing finally clause")
    ...
    >>> divide(2, 1)
    result is 2.0
    executing finally clause
    >>> divide(2, 0)
    division by zero!
    executing finally clause
    >>> divide("2", "1")
    executing finally clause
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 3, in divide
    TypeError: unsupported operand type(s) for /: 'str' and 'str'

如您所见，[finally](https://docs.python.org/3.6/reference/compound_stmts.html#finally)子句在任何情况下都会执行。由于两个字符串相除抛出了[TypeError](https://docs.python.org/3.6/library/exceptions.html#TypeError)异常，该异常没有被后续的[except](https://docs.python.org/3.6/reference/compound_stmts.html#except)子句捕获，所以在执行完[finally](https://docs.python.org/3.6/reference/compound_stmts.html#finally)子句后会重新抛出[TypeError](https://docs.python.org/3.6/library/exceptions.html#TypeError)异常。

在实际应用程序中，[finally](https://docs.python.org/3.6/reference/compound_stmts.html#finally)子句对于释放外部资源（例如文件或网络连接）非常有用，无论资源的使用是否成功。

<h4 id='8.7.'>8.7. 预定义的清理操作Predefined Clean-up Actions</h4>

某些对象定义了当不再需要该对象时要执行的标准清理操作，无论使用该对象的操作是成功还是失败。请查看以下示例，该示例尝试打开文件并将其内容打印到屏幕上。

    for line in open("myfile.txt"):
        print(line, end="")

此代码的问题在于，在部分代码执行完毕后，它会使文件保持打开一段不确定的时间。这在简单的小脚本中不是问题，但对于较大的应用程序可能是一个问题。[with](https://docs.python.org/3.6/reference/compound_stmts.html#with)语句允许以一种始终确保及时正确地清理对象的方式来使用文件等对象。

    with open("myfile.txt") as f:
        for line in f:
            print(line, end="")

After the statement is executed, the file f is always closed, even if a problem was encountered while processing the lines. Objects which, like files, provide predefined clean-up actions will indicate this in their documentation.

## 个人笔记