# Python 教程（六） -- 数据结构

## 原文链接

[5. Data Structures](https://docs.python.org/3.6/tutorial/datastructures.html#tut-loopidioms)

## 原文翻译

<h3 id='5.'>5. 数据结构</h3>

本章将更详细地介绍您已经了解的一些内容，并添加了一些新内容。

<h4 id='5.1.'>5.1. 再述列表</h4>

列表数据类型有更多方法。以下是列表对象的所有方法：

`list.append(x)`
列表末尾添加一个元素。等价于`a[len(a):] = x`；（切片索引超出范围不会报错）

`list.clear()`
从列表中删除所有元素。等价于`del a[:]`；

`list.copy()`
返回列表的浅拷贝。等价于`a[:]`；

`list.count(x)`
返回元素x在列表中出现的次数；

`list.extend(iterable)`
列表末尾添加可迭代对象`iterable`中的所有元素。等价于`a[len(a):] = iterable`；

`list.index（x [，start [，end ] ] ）`
返回列表中第一个值为x的元素的索引。如果没有找到对应元素x，则抛出异常[ValueError](https://docs.python.org/3.6/library/exceptions.html#ValueError)。

可选参数*start*和*end*被解释为切片表示法，并用于将搜索限制为列表的特定子序列。返回的索引是相对于完整序列的开头而不是*start*参数为开头计算的。

`list.insert（i，x ）`
在给定位置i处插入一个元素x，原位置为i上的元素后移。第一个参数是要插入的元素的索引，因此`a.insert(0, x)`插入列表的前面，并且`a.insert(len(a), x)`等效于`a.append(x)`。

`list.pop([i])`
删除并返回列表中给定位置i的元素。如果未指定索引，则`a.pop()`删除并返回列表中的最后一项。（方法签名中i周围的方括号表示该参数是可选的，而不是您应该在该位置键入方括号。您将在Python Library Reference中经常看到这种表示法。）

`list.remove(x)`
删除列表中第一个值为x的元素。如果没有这样的元素，则报错。

`list.reverse()`
反转列表中的元素。

`list.sort(key=None, reverse=False)`
排序列表中的元素（参数可用于自定义排序，请参阅[sorted()](https://docs.python.org/3.6/library/functions.html#sorted)其说明）

使用大多数列表方法的示例：

    >>> fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi', 'apple', 'banana']
    >>> fruits.count('apple')
    2
    >>> fruits.count('tangerine')
    0
    >>> fruits.index('banana')
    3
    >>> fruits.index('banana', 4)  # Find next banana starting a position 4
    6
    >>> fruits.reverse()
    >>> fruits
    ['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange']
    >>> fruits.append('grape')
    >>> fruits
    ['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange', 'grape']
    >>> fruits.sort()
    >>> fruits
    ['apple', 'apple', 'banana', 'banana', 'grape', 'kiwi', 'orange', 'pear']
    >>> fruits.pop()
    'pear'

您可能已经注意到，像`insert`，`remove`或者`sort`这样只修改列表没有打印返回值的方法，它们默认返回`None`。这是Python中所有可变数据结构的设计原则。

<h4 id='5.1.1.'>5.1.1. 将列表用作栈</h4>

列表的方法使得列表很容易用作栈，栈是一种后入先出的数据结构。请使用`append()`将元素添加到栈顶。请使用无索引参数的`pop()`从栈顶读取元素。

    >>> stack = [3, 4, 5]
    >>> stack.append(6)
    >>> stack.append(7)
    >>> stack
    [3, 4, 5, 6, 7]
    >>> stack.pop()
    7
    >>> stack
    [3, 4, 5, 6]
    >>> stack.pop()
    6
    >>> stack.pop()
    5
    >>> stack
    [3, 4]

<h4 id='5.1.2.'>5.1.2. 将列表用作队列</h4>

列表也可以用作队列，队列是一种先进先出的数据结构; 但是，当列表用作队列时，效率并不高。尽管在列表末尾的追加append和弹出pop很快，但是在列表开头进行插入insert或弹出pop是很慢的（因为所有其他元素都必须移动一位）。

要实现队列，请使用[collections.deque](https://docs.python.org/3.6/library/collections.html#collections.deque)，[collections.deque](https://docs.python.org/3.6/library/collections.html#collections.deque)设计为支持从列表两端都能够快速追加append和弹出pop。例如：

    >>> from collections import deque
    >>> queue = deque(["Eric", "John", "Michael"])
    >>> queue.append("Terry")           # Terry arrives
    >>> queue.append("Graham")          # Graham arrives
    >>> queue.popleft()                 # The first to arrive now leaves
    'Eric'
    >>> queue.popleft()                 # The second to arrive now leaves
    'John'
    >>> queue                           # Remaining queue in order of arrival
    deque(['Michael', 'Terry', 'Graham'])


<h4 id='5.1.3.'>5.1.3. 列表生成式</h4>

列表推导提供了创建列表的简明方法。列表生成式的常见应用是创建新的列表，新列表的每个元素是另一个序列对象或者可迭代对象的每个元素经过一些操作的结果，或者创建满足特定条件的元素的子序列。

例如，假设我们要创建一个正方形列表：

    >>> squares = []
    >>> for x in range(10):
    ...     squares.append(x**2)
    ...
    >>> squares
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    
请注意，这会创建（或覆盖）一个名为`x`在循环完成后仍然存在的变量。我们可以使用以下方法计算正方形列表而没有任何副作用：

    squares = list(map(lambda x: x**2, range(10)))
    
或者，等效地：

    squares = [x**2 for x in range(10)]

这更简洁，更易读。

列表推导由括号组成，括号中包含一个表达式，后跟一个[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)从句，然后是零个或多个[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)或[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)从句。结果将生成一个新的列表，该列表是通过在其后的[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)和[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)子句的上下文中评估表达式而得到的。(The result will be a new list resulting from evaluating the expression in the context of the for and if clauses which follow it.)例如，如果对应元素不相等，则此列表生成式将两个列表的元素组合在一起：

    >>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
    [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
    
它相当于：

    >>> combs = []
    >>> for x in [1,2,3]:
    ...     for y in [3,1,4]:
    ...         if x != y:
    ...             combs.append((x, y))
    ...
    >>> combs
    [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
    
请注意这两段代码中[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)和[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)语句的顺序是如何相同的。(外层for循环在前，内层for循环在后)

如果表达式是元组（例如前面示例中的元组`(x, y)`），则必须将其括起来。

    >>> vec = [-4, -2, 0, 2, 4]
    >>> # create a new list with the values doubled
    >>> [x*2 for x in vec]
    [-8, -4, 0, 4, 8]
    >>> # filter the list to exclude negative numbers
    >>> [x for x in vec if x >= 0]
    [0, 2, 4]
    >>> # apply a function to all the elements
    >>> [abs(x) for x in vec]
    [4, 2, 0, 2, 4]
    >>> # call a method on each element
    >>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
    >>> [weapon.strip() for weapon in freshfruit]
    ['banana', 'loganberry', 'passion fruit']
    >>> # create a list of 2-tuples like (number, square)
    >>> [(x, x**2) for x in range(6)]
    [(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
    >>> # the tuple must be parenthesized, otherwise an error is raised
    >>> [x, x**2 for x in range(6)]
      File "<stdin>", line 1, in <module>
        [x, x**2 for x in range(6)]
                   ^
    SyntaxError: invalid syntax
    >>> # flatten a list using a listcomp with two 'for'
    >>> vec = [[1,2,3], [4,5,6], [7,8,9]]
    >>> [num for elem in vec for num in elem]
    [1, 2, 3, 4, 5, 6, 7, 8, 9]

列表生成式可以包含复杂的表达式和嵌套函数：

    >>> from math import pi
    >>> [str(round(pi, i)) for i in range(1, 6)]
    ['3.1', '3.14', '3.142', '3.1416', '3.14159']


<h4 id='5.1.4.'>5.1.4. 嵌套的列表生成式</h4>

列表生成式中的初始表达式可以是任意表达式，甚至可以是另一个列表生成式。

考虑以下3x4矩阵示例，该矩阵由3个长度为4的列表实现：

    >>> matrix = [
    ...     [1, 2, 3, 4],
    ...     [5, 6, 7, 8],
    ...     [9, 10, 11, 12],
    ... ]
    
以下列表生成式将转置行和列：

    >>> [[row[i] for row in matrix] for i in range(4)]
    [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

正如我们在上一节中看到的那样，嵌套的列表生成式根据其后面[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)的上下文来进行计算，因此该示例等效于：

    >>> transposed = []
    >>> for i in range(4):
    ...     transposed.append([row[i] for row in matrix])
    ...
    >>> transposed
    [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
    
依次的，它也等价于：

    >>> transposed = []
    >>> for i in range(4):
    ...     # the following 3 lines implement the nested listcomp
    ...     transposed_row = []
    ...     for row in matrix:
    ...         transposed_row.append(row[i])
    ...     transposed.append(transposed_row)
    ...
    >>> transposed
    [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

在现实世界中，您应该更喜欢内置函数来处理复杂的流语句。该[zip()](https://docs.python.org/3.6/library/functions.html#zip)函数对于这个用例非常有用：

    >>> list(zip(*matrix))
    [(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]
    
有关此行中星号的详细信息，请参阅[解压缩参数列表](https://docs.python.org/3.6/tutorial/controlflow.html#tut-unpacking-arguments)。   

<h4 id='5.2.'>5.2. del 语句</h4>

有一种方法可以根据元素的索引而不是值来从列表中删除一个元素：[del](https://docs.python.org/3.6/reference/simple_stmts.html#del)语句。这与`pop()`方法不同的地方在于，`pop()`返回被删除元素的值。[del](https://docs.python.org/3.6/reference/simple_stmts.html#del)语句还可用于从列表中删除切片或清除整个列表（我们之前通过将空列表分配给切片来执行此操作）。例如：

    >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
    >>> del a[0]
    >>> a
    [1, 66.25, 333, 333, 1234.5]
    >>> del a[2:4]
    >>> a
    [1, 66.25, 1234.5]
    >>> del a[:]
    >>> a
    []
    
[del](https://docs.python.org/3.6/reference/simple_stmts.html#del)也可以用来删除整个变量：

    >>> del a

删除`a`之后引用`a`会报错（至少在为其分配了另一个值之前）。我们会在后续讲解[del](https://docs.python.org/3.6/reference/simple_stmts.html#del)的其他用途。

<h4 id='5.3.'>5.3. 元组和序列对象</h4>

我们看到列表和字符串有许多相同的属性，例如索引和切片操作。列表和字符串是序列数据类型的两个示例（请参阅[Sequence Types — list, tuple, range](https://docs.python.org/3.6/library/stdtypes.html#typesseq)）。由于Python是一种不断发展的语言，因此可能还会添加其他序列数据类型。还有另一种标准序列数据类型：*元组*。

元组由逗号分隔的多个值组成，例如：

    >>> t = 12345, 54321, 'hello!'
    >>> t[0]
    12345
    >>> t
    (12345, 54321, 'hello!')
    >>> # Tuples may be nested:
    ... u = t, (1, 2, 3, 4, 5)
    >>> u
    ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
    >>> # Tuples are immutable:
    ... t[0] = 88888
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
    >>> # but they can contain mutable objects:
    ... v = ([1, 2, 3], [3, 2, 1])
    >>> v
    ([1, 2, 3], [3, 2, 1])
    
如您所见，输出元组始终用括号括起来，以便正确解释嵌套元组; 创建元组时可以有或没有括号括起来，尽管通常括号是必要的（如果元组是较大表达式的一部分）。不能为元组的各个元素赋值，但是可以创建包含可变对象的元组，例如列表。

尽管元组看起来与列表类似，但它们通常用于不同的情况和不同的目的。元组是[不可变对象](https://docs.python.org/3.6/glossary.html#term-immutable)，并且通常是包含不同类型元素的序列，这些元素可以通过解包（参见本节后面部分）或通过索引（或者甚至是使用[namedtuples](https://docs.python.org/3.6/library/collections.html#collections.namedtuple)时通过属性）来访问。列表是[可变对象](https://docs.python.org/3.6/glossary.html#term-mutable)，列表的元素通常是同种类型的，可以通过遍历列表来访问。

一个特殊的问题是构造长度为0或1的元组：语法有一些额外的怪癖来适应这些（ the syntax has some extra quirks to accommodate these）。使用一对空括号来创建空元组; 使用单一值后跟随一个逗号来创建长度为1的元组（单个值用括号括起来是不够的，还需要逗号跟在值后面）。丑陋但有效。例如：

    >>> empty = ()
    >>> singleton = 'hello',    # <-- note trailing comma
    >>> len(empty)
    0
    >>> len(singleton)
    1
    >>> singleton
    ('hello',)

语句`t = 12345, 54321, 'hello!'`是元组打包的一个例子：值`12345`,`54321`,`'hello!'`被组装成一个元组。反向操作也是可以的：

    >>> x, y, z = t
    
这称为序列解包，等号右侧可以是任何序列类型。序列解包需要等号左侧变量数等于等号右侧的序列中的元素数。请注意，多元赋值实际上只是元组打包和序列解包的组合。

<h4 id='5.4.'>5.4. 集合</h4>

Python还包括一种数据类型--集合。集合是没有重复元素的无序集合。基本用途包括成员资格测试和消除重复元素。集合对象还支持数学运算，如并集，交集，差集和异或。

大括号或[set()](https://docs.python.org/3.6/library/stdtypes.html#set)函数可用于创建集合。注意：必须使用[set()](https://docs.python.org/3.6/library/stdtypes.html#set)来创建一个空集，而不是`{}`; 后者创建一个空字典，一个我们将在下一节讨论的数据结构。

这是一个简短的演示:

    >>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
    >>> print(basket)                      # show that duplicates have been removed
    {'orange', 'banana', 'pear', 'apple'}
    >>> 'orange' in basket                 # fast membership testing
    True
    >>> 'crabgrass' in basket
    False

    >>> # Demonstrate set operations on unique letters from two words
    ...
    >>> a = set('abracadabra')
    >>> b = set('alacazam')
    >>> a                                  # unique letters in a
    {'a', 'r', 'b', 'c', 'd'}
    >>> a - b                              # letters in a but not in b
    {'r', 'd', 'b'}
    >>> a | b                              # letters in a or b or both
    {'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
    >>> a & b                              # letters in both a and b
    {'a', 'c'}
    >>> a ^ b                              # letters in a or b but not both
    {'r', 'd', 'b', 'm', 'z', 'l'}
    
与[列表生成式](https://docs.python.org/3.6/tutorial/datastructures.html#tut-listcomps)类似，也支持集合生成式：

    >>> a = {x for x in 'abracadabra' if x not in 'abc'}
    >>> a
    {'r', 'd'}

<h4 id='5.5.'>5.5. 字典</h4>

Python内置的另一个有用的数据类型是字典（请参阅 [映射类型 - 字典](https://docs.python.org/3.6/library/stdtypes.html#typesmapping)）。字典有时在其他语言中被称为“关联记忆associative memories”或“关联阵列associative arrays”。与通过数字索引的序列对象不同，字典由键`key`索引，键`key`可以是任何不可变类型; 字符串和数字可以用作键`key`。如果元组仅包含字符串，数字或元组，则它们可用作键`key`; 如果元组直接或间接包含任何可变对象，则不能将其用作键`key`。你不能用列表作关键字（键），因为列表可以用索引赋值，切片赋值，或类似append()和 extend()的方法进行修改。

最好将字典视为要求键是唯一的（在一个字典中），一组无序的键：值对集合。一对花括号创建一个空字典：`{}`。在花括号内放置以逗号分隔的键：值对列表，将初始的键：值对添加到字典中; 这也是字典在输出上的写法。

字典上的主要操作是使用某个键存储值并提取给定键的值。也可以使用`del`删除键：值对。如果使用已在使用的键进行存储值，则会忘记与该键关联的旧值（覆盖旧值）。使用不存在的键提取值是错误的。

对字典执行`list(d.keys())`会以任意顺序返回字典中所有键的列表（如果您希望对其进行排序，则只需使用`sorted(d.keys())`）。[2] 要检查单个键是否在字典中，请使用[in](https://docs.python.org/3.6/reference/expressions.html#in)关键字。

这是一个使用字典的小例子：

    >>> tel = {'jack': 4098, 'sape': 4139}
    >>> tel['guido'] = 4127
    >>> tel
    {'sape': 4139, 'guido': 4127, 'jack': 4098}
    >>> tel['jack']
    4098
    >>> del tel['sape']
    >>> tel['irv'] = 4127
    >>> tel
    {'guido': 4127, 'irv': 4127, 'jack': 4098}
    >>> list(tel.keys())
    ['irv', 'guido', 'jack']
    >>> sorted(tel.keys())
    ['guido', 'irv', 'jack']
    >>> 'guido' in tel
    True
    >>> 'jack' not in tel
    False

[dict()](https://docs.python.org/3.6/library/stdtypes.html#dict)构造器直接从键-值对的序列构建字典：

    >>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
    {'sape': 4139, 'jack': 4098, 'guido': 4127}
    
此外，字典生成式dict comprehensions可用于从任意键和值表达式创建字典：

    >>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}

当键是简单字符串时，有时使用关键字参数指定键值对更容易：

    >>> dict(sape=4139, guido=4127, jack=4098)
    {'sape': 4139, 'jack': 4098, 'guido': 4127}
    

<h4 id='5.6.'>5.6. 循环技巧</h4>

循环遍历字典时，可以使用该`items()`方法同时检索键和相应的值。

    >>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
    >>> for k, v in knights.items():
    ...     print(k, v)
    ...
    gallahad the pure
    robin the brave
    
循环遍历序列时，可以使用该[enumerate()](https://docs.python.org/3.6/library/functions.html#enumerate)函数同时检索位置索引和相应的值。

    >>> for i, v in enumerate(['tic', 'tac', 'toe']):
    ...     print(i, v)
    ...
    0 tic
    1 tac
    2 toe

要同时循环两个或更多个序列，the entries can be paired with the [zip()](https://docs.python.org/3.6/library/functions.html#zip) function.

    >>> questions = ['name', 'quest', 'favorite color']
    >>> answers = ['lancelot', 'the holy grail', 'blue']
    >>> for q, a in zip(questions, answers):
    ...     print('What is your {0}?  It is {1}.'.format(q, a))
    ...
    What is your name?  It is lancelot.
    What is your quest?  It is the holy grail.
    What is your favorite color?  It is blue.

要反向循环序列，使用正向序列调用[reversed()](https://docs.python.org/3.6/library/functions.html#reversed)函数。

    >>> for i in reversed(range(1, 10, 2)):
    ...     print(i)
    ...
    9
    7
    5
    3
    1

To loop over a sequence in sorted order, use the [sorted()](https://docs.python.org/3.6/library/functions.html#sorted) function which returns a new sorted list while leaving the source unaltered.

    >>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
    >>> for f in sorted(set(basket)):
    ...     print(f)
    ...
    apple
    banana
    orange
    pear

It is sometimes tempting to change a list while you are looping over it; however, it is often simpler and safer to create a new list instead.

    >>> import math
    >>> raw_data = [56.2, float('NaN'), 51.7, 55.3, 52.5, float('NaN'), 47.8]
    >>> filtered_data = []
    >>> for value in raw_data:
    ...     if not math.isnan(value):
    ...         filtered_data.append(value)
    ...
    >>> filtered_data
    [56.2, 51.7, 55.3, 52.5, 47.8]

<h4 id='5.7.'>5.7. 再述条件</h4>

`while`和`if`语句中使用的条件可以包含任何运算符，而不仅仅是比较运算符。

比较运算符`in`和`not in`检查序列中是否存在（不存在）某值。运算符`is`和`is not`比较两个对象是否真的是同一个对象; 这只对像列表这样的可变对象很重要。所有比较运算符具有相同的优先级，低于所有数值运算符的优先级。

比较运算符可以连接。例如，`a < b == c`测试是否`a`小于`b`，而且`b`等于`c`。

比较运算符可以使用布尔运算符`and`和`or`进行组合，并且一次比较的结果（或任何其它的布尔表达式的结果）可以与否定`not`组合。布尔运算符优先级低于比较运算符; 它们之间，`not`具有最高优先级和`or`具有最低优先级，因此`A and not B or C`相当于`(A and (not B)) or C`
。与往常一样，括号可用于表达所需的组成。

布尔运算符`and`和`or`是所谓的**短路运算符**：它们的参数从左到右进行计算，一旦确定结果，计算就会停止。例如，如果`A`和`C`为true但`B`为false，则`A and B and C`不评估表达式`C`。当用作一般值而不是布尔值时，短路运算符的返回值是最后一个计算的参数。

可以将比较结果或其他布尔表达式赋值给变量。例如，

    >>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
    >>> non_null = string1 or string2 or string3
    >>> non_null
    'Trondheim'
    
Note that in Python, unlike C, assignment cannot occur inside expressions. C programmers may grumble about this, but it avoids a common class of problems encountered in C programs: typing = in an expression when == was intended.

<h4 id='5.8.'>5.8. 比较序列对象和其他类型</h4>

可以将序列对象与具有相同序列类型的其他对象进行比较。比较使用词典排序：首先比较前两个元素，如果它们不同，则确定比较的结果; 如果它们相等，则比较接下来的两个元素，依此类推，直到任一序列用完为止。如果要比较的两个元素本身就是是相同类型的序列，则递归地执行词典比较。如果两个序列的所有元素相等，则认为序列相等。如果一个序列是另一个序列的初始子序列，则较短的序列是较小的（较小的）序列。字符串的字典顺序使用Unicode代码点编号来排序单个字符。相同类型序列之间比较的一些示例：

    (1, 2, 3)              < (1, 2, 4)
    [1, 2, 3]              < [1, 2, 4]
    'ABC' < 'C' < 'Pascal' < 'Python'
    (1, 2, 3, 4)           < (1, 2, 4)
    (1, 2)                 < (1, 2, -1)
    (1, 2, 3)             == (1.0, 2.0, 3.0)
    (1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)

请注意，如果对象具有适当的比较方法，则将不同类型的对象与对象进行比较`<`或是`>`合法的。例如，混合数字类型根据它们的数值进行比较，因此0等于0.0等。否则，解释器将引发[TypeError](https://docs.python.org/3.6/library/exceptions.html#TypeError)异常，而不是提供任意排序。


## 个人笔记