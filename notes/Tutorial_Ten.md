# Python 教程(十) -- 类

## 原文链接

[9. Classes](https://docs.python.org/3.6/tutorial/classes.html)

## 原文翻译

<h3 id='9.'>9. 类</h3>

类提供了将数据和处理数据的函数捆绑在一起的方法。创建新类会创建一种新的类型对象（Python中一切都是对象，类型也是对象），从而允许创建该类型的新实例。每个类实例都可以具有用于维护其状态的附加属性。类实例还可以具有用于修改其状态的方法（由其类定义）。

与其他编程语言相比，Python的类机制为类添加了最少量的新语法和语义。它是C++和Modula-3中的类机制的混合体。Python类提供面向对象编程的所有标准功能：类继承机制允许多个基类（多重继承），派生类可以重构其基类的任何方法，并且方法可以调用具有相同名称的基类的方法。对象可以包含任意数量和种类的数据。与模块一样，类依赖于Python的动态特性：它们是在运行时创建的，并且可以在创建后进一步修改。

在C++术语中，通常类成员（包括数据成员）是*公有的*（除了下述的[私有变量](https://docs.python.org/3.6/tutorial/classes.html#tut-private)），并且所有成员函数都是*虚函数*。与Modula-3中一样，Python没有用于从其方法中引用对象成员的简写：类方法声明时，其第一个参数必定表示对象本身，该参数由调用隐式提供。与Smalltalk一样，Python中类本身也是对象（类型对象）。这为导入和重命名提供了语义。与C++和Modula-3不同，Python内置类型可以用作用户扩展的基类。此外，与C++一样，Python中大多数具有特殊语法（算术运算符，下标等）的内置运算符都可以在类实例中重载（重载运算符）。

（由于缺乏普遍接受的、用于讨论类的术语，我偶尔会使用Smalltalk和C++术语。我会使用Modula-3术语，因为它的面向对象语义比C++更接近Python，但我估计读者很少听说过它。）

<h4 id='9.1.'>9.1. 关于名称Names和对象Object</h4>

对象具有独立性，（在不同作用域的）多个名称Names可以绑定到同一个对象。刚学习Python时通常无法了解这一点，在处理不可变的基本类型（数字，字符串，元组）时可以安全地忽略它。但是，别名对涉及可变对象（如列表，字典和大多数其他类型）的Python代码的语义可能会产生惊人的影响。这通常有利于程序，因为别名在某些方面表现的像指针。例如，传递一个对象很简便，因为只需要传递一个指针就可以实现。如果函数修改了作为参数传递进函数的对象，调用者将看到对象的更改 - 这消除了对Pascal中两个不同的参数传递机制的需要（值传递和地址传递）。

<h4 id='9.2.'>9.2. Python作用域和命名空间</h4>

在介绍类之前，我首先要告诉你一些Python的作用域规则。类定义对命名空间做了一些手脚，你需要知道作用域和命名空间如何工作才能完全理解正在发生的事情。顺便说一下，关于这个主题的知识对任何高级Python程序员都很有用。

让我们从一些定义开始。

*命名空间*是从名称Names到对象Objects的映射(表示名称和对象的对应关系)。现在大多数命名空间都是作为Python字典实现的，but that’s normally not noticeable in any way (except for performance)，并且它可能在将来发生变化。命名空间的例子有：内置名称集（包含内置函数，例如[abs()](https://docs.python.org/3.6/library/functions.html#abs)和内置异常名称）; 模块中的全局名称global names; 函数调用中的局部（本地）名称local names。在某种意义上，对象的属性集也形成了命名空间。关于命名空间的重要一点是，不同命名空间中的名称之间绝对没有关系（互不影响）; 例如，两个不同的模块都可以定义函数`maximize`而不会产生混淆 - 模块的用户在使用`maximize`时必须在其前面加上模块名称。

顺便说一句，我对点操作符后跟着的任意名称name使用attribute属性这个词来描述--例如，在表达式`z.real`中，`real`是对象`z`的属性。严格来讲，对模块中名称的引用实质上是对模块属性的引用：在表达式`modname.funcname`中，`modname`是一个模块对象并且`funcname`是它的一个属性（Python中模块也是对象）。在这种情况下，模块属性和模块中定义的全局名称之间恰好一一对应：它们使用相同的命名空间！(Except for one thing. Module objects have a secret read-only attribute called [__dict__](https://docs.python.org/3.6/library/stdtypes.html#object.__dict__) which returns the dictionary used to implement the module’s namespace; the name [__dict__](https://docs.python.org/3.6/library/stdtypes.html#object.__dict__) is an attribute but not a global name. Obviously, using this violates the abstraction of namespace implementation, and should be restricted to things like post-mortem debuggers.)

属性可以是只读的，也可以是可写的。对于可写属性，可以给属性赋值。模块属性是可写的：你可以写`modname.the_answer = 42`。可写属性也可以使用[del](https://docs.python.org/3.6/reference/simple_stmts.html#del)语句删除。例如，`del modname.the_answer`将从名为`modname`的对象中删除属性`the_answer`。

命名空间在不同时刻创建，且具有不同的生命周期。包含内置名称的命名空间是在Python解释器启动时创建的，并且永远不会被删除。模块的全局命名空间是在读入模块定义时创建的; 通常，模块命名空间也会持续到解释器退出。由解释器的顶级调用执行的语句（从脚本文件读取或交互式解释器加载）被视为`__main__`模块的一部分，因此它们具有自己的全局命名空间。（内置名称实际上也存在于模块中;该模块为[builtins](https://docs.python.org/3.6/library/builtins.html#module-builtins)。）

函数的局部(本地)命名空间在调用函数时创建，并在函数返回时或抛出函数内未处理的异常时删除。（实际上，遗忘是描述实际情况的更好方法。）当然，每个递归调用都有自己的局部(本地)命名空间。

作用域是可以直接访问一个命名空间的Python程序的文本区域。这里的“可直接访问”意味着对名称的无限制引用会尝试在命名空间中查找名称。

尽管作用域是静态确定的，但它们是动态使用的。在执行期间的任何时候，至少有三个嵌套的作用域，可以直接访问其命名空间：

+ 最内层作用域（最先被搜索），包含函数局部（本地）名称。
+ 任意外层函数的作用域（从最内层函数的作用域开始，一层一层向外层函数作用域搜索），包含非局部（本地）非全局名称。
+ 倒数第二层作用域（模块作用域），包含当前模块的所有全局名称。
+ 最外层作用域（最后被搜索），包含所有内置名称。

如果名称被声明为全局名称，则对全局名称的所有引用和赋值都会在包含模块全局名称的中间（倒数第二层，不是最外层也不是最内层）作用域。在内层作用域内可以访问外层作用域的名称的值，但是在内层作用域内要想对外层作用域的名称进行赋值，必须使用[nonlocal](https://docs.python.org/3.6/reference/simple_stmts.html#nonlocal)语句（否则，将会在内层作用域中创建一个与外层作用域名称同名的新的局部（本地）名称，而外层作用域中同名的名称保持不变）。

通常，局部（本地）作用域引用当前函数的局部（本地）名称。在函数外部，局部（本地）作用域引用与全局作用域相同的命名空间：模块命名空间。类定义在局部（本地）作用域中放置了另一个命名空间。

重要的是要意识到作用域是以文本方式确定的：模块中定义的函数的全局作用域是模块的命名空间，无论函数是从何处调用或函数是通过调什么别名调用。另一方面，名称的实际搜索是在运行时动态完成的 - 但是，语言定义朝着在“编译”时静态名称解析发展，所以不要依赖于动态名称解析！（事实上​​，局部变量已经静态确定。）

Python的一个特殊之处在于--如果没有使用[global](https://docs.python.org/3.6/reference/simple_stmts.html#global)语句，对名称的赋值将在最内层作用域中进行。对名称赋值并不复制数据值，而是将名称和对象绑定到一起（Python中将对象的引用赋值给名称，而不是对象的数据值，也就是将名称指向对象）。删除名称也是如此：`del x`语句在本地作用域引用的命名空间中删除了名称`x`与对像的绑定。实际上，引入新名称的所有操作都使用本地作用域：特别是，[import](https://docs.python.org/3.6/reference/simple_stmts.html#import)语句和函数定义绑定本地作用域中的模块或函数名称。

The [global](https://docs.python.org/3.6/reference/simple_stmts.html#global) statement can be used to indicate that particular variables live in the global scope and should be rebound there; the [nonlocal](https://docs.python.org/3.6/reference/simple_stmts.html#nonlocal) statement indicates that particular variables live in an enclosing scope and should be rebound there.

<h4 id='9.2.1'>9.2.1. 作用域与命名空间例子</h4>

这是一个例子展示了如何引用不同的作用域和命名空间，以及[global](https://docs.python.org/3.6/reference/simple_stmts.html#global)和[nonlocal](https://docs.python.org/3.6/reference/simple_stmts.html#nonlocal)如何影响变量绑定：

    def scope_test():
        def do_local():
            spam = "local spam"
    
        def do_nonlocal():
            nonlocal spam
            spam = "nonlocal spam"
    
        def do_global():
            global spam
            spam = "global spam"
    
        spam = "test spam"
        do_local()
        print("After local assignment:", spam)
        do_nonlocal()
        print("After nonlocal assignment:", spam)
        do_global()
        print("After global assignment:", spam)
    
    scope_test()
    print("In global scope:", spam)

示例代码的输出是：

    After local assignment: test spam
    After nonlocal assignment: nonlocal spam
    After global assignment: nonlocal spam
    In global scope: global spam

请注意本地（局部）赋值（默认情况下）是如何没有改变scope_test函数的spam名称（变量）的绑定的。[nonlocal](https://docs.python.org/3.6/reference/simple_stmts.html#nonlocal)赋值是如何改变scope_test函数的spam名称（变量）的绑定的，以及[global](https://docs.python.org/3.6/reference/simple_stmts.html#global)赋值是如何改变模块层次的名称（变量）的绑定的。

你也可以发现，在使用[global](https://docs.python.org/3.6/reference/simple_stmts.html#global)赋值之前，并没有对象绑定到spam名称（没用定义spam名称）。

<h4 id='9.3.'>9.3. 初次学习类</h4>

类引入了一些新语法，三种新对象类型和一些新语义。

<h4 id='9.3.1.'>9.3.1. 类定义语法</h4>

类定义的最简单形式如下所示：

    class ClassName:
        <statement-1>
        .
        .
        .
        <statement-N>

类定义，类似函数定义（[def](https://docs.python.org/3.6/reference/compound_stmts.html#def)语句）必须在类生效之前执行。（您可以想象将类定义放在[if](https://docs.python.org/3.6/reference/compound_stmts.html#if)语句的分支中，或者放在函数内部。）

实际上，类定义中的语句通常是函数定义，但也允许有其他语句，有时也很有用 - 我们稍后会再回过头来讨论它。类中的函数定义通常具有一种特殊形式的参数列表，该参数列表由方法的调用约定决定 - 再次，这将在后面解释。

进入类定义时，会创建一个新的命名空间，并将其用作本地作用域 - 因此，对局部（本地）变量（名称）的所有赋值操作都将进入此新命名空间。特别是，在类内命名空间，函数定义绑定到新函数的名称。

当正常离开（结束）类定义时，一个类对象被创建。这本质上是类定义创建的命名空间的内容的包装器; 我们将在下一节中了解有关类对象的更多信息。当离开类定义时，恢复原始本地作用域（在进入类定义之前生效的作用域），并且类对象在作用域中绑定到类定义头部（ClassName在示例中）中给出的类名。

<h4 id='9.3.2.'>9.3.2. 类对象</h4>

类对象支持两种操作：属性引用和实例化。

*属性引用*使用Python中所有*属性引用*使用的标准语法：`obj.name`。有效的属性名称是创建类对象时类的命名空间中的所有名称。所以，如果类定义看起来像这样：

    class MyClass:
        """A simple example class"""
        i = 12345
    
        def f(self):
            return 'hello world'

那么`MyClass.i`和`MyClass.f`是有效的属性引用，分别返回一个整数和一个函数对象。也可以为类属性赋值，因此您可以通过赋值更改`MyClass.i`属性的值。`__doc__`也是一个有效的属性，返回属于该类的文档字符串:`"A simple example class"`。

类实例化使用函数表示法。只是假装类对象是一个无参数函数，该函数返回一个新的类实例。例如（假设上述类）：

    x = MyClass()

创建类的新实例，并将此类实例对象分配给局部变量`x`。

实例化操作（“调用”类对象）创建一个空对象。许多类喜欢创建具有特定初始状态的类实例对象。因此，类可以定义一个名为`__init__()`的特殊方法，如下所示：

    def __init__(self):
        self.data = []

当一个类定义了`__init__()`方法，在类实例化时，新创建的类实例会自动调用`__init__()`。因此，在此示例中，可以通过以下方式获取新的初始化实例：

    x = MyClass()

当然，[__init__()](https://docs.python.org/3.6/reference/datamodel.html#object.__init__)方法可以携带参数，以便更灵活地初始化类实例。在这种情况下，传递给类实例操作符的参数将会被传递给[__init__()](https://docs.python.org/3.6/reference/datamodel.html#object.__init__)方法。例如：

    >>> class Complex:
    ...     def __init__(self, realpart, imagpart):
    ...         self.r = realpart
    ...         self.i = imagpart
    ...
    >>> x = Complex(3.0, -4.5)
    >>> x.r, x.i
    (3.0, -4.5)

<h4 id='9.3.3.'>9.3.3. 类实例对象</h4>

现在我们可以用实例对象做什么？实例对象理解的唯一操作是属性引用。有两种有效的属性名称，数据属性和方法。

*数据属性*对应于Smalltalk中的“实例变量”，以及C ++中的“数据成员”。不需要声明数据属性; 就像局部变量一样，它们在第一次被赋值时就会存在。例如，如果 `x`是`MyClass`的实例，则以下代码段将打印值16，而不留下踪迹：

    x.counter = 1
    while x.counter < 10:
        x.counter = x.counter * 2
    print(x.counter)
    del x.counter

另一种实例属性是方法。方法是“属于”对象的函数。（在Python中，方法不是类实例独有的：其他对象类型也可以有方法。例如，列表对象具有名为append，insert，remove，sort等的方法。但是，在下面的讨论中，除非另有明确说明，否则我们将专门使用术语方法来表示类实例对象的方法。）

实例对象的有效方法名称取决于其类。根据定义，的类的所有函数对象属性（属性本身是函数对象）定义了其实例的相应方法。所以在我们的例子中，`x.f`是一个有效的方法引用，因为`MyClass.f`是一个函数对象，但`x.i`不是有效的方法引用，因为`MyClass.i`不是函数对象。但`x.f`和`MyClass.f`不是同一个东西 - 它是一个方法对象，而不是一个函数对象。

<h4 id='9.3.4.'>9.3.4. 方法对象</h4>

通常，绑定后立即调用方法：

    x.f()

在MyClass示例中，这将返回字符串`'hello world'`。但是，没有必要立即调用方法：`x.f`是一个方法对象，可以存储起来并在以后调用。例如：

    xf = x.f
    while True:
        print(xf())

会一直打印`hello world`。

调用方法时到底发生了什么？您可能已经注意到，即使`f()`的函数定义指定了一个参数，但是上面调用`x.f()`时没有携带参数。这个参数怎么了？当一个需要参数的函数在调用时没有传递任何参数，Python肯定会抛出异常 - 即使参数实际上没有被使用......

实际上，您可能已经猜到了答案：方法的特殊之处在于实例对象作为第一个参数传递给函数。在我们的示例中，调用`x.f()`完全等同于`MyClass.f(x)`。

如果您仍然不了解方法的工作原理，那么查看实现可能会让你稍微理解。当引用实例的非数据属性时，将搜索实例的类。如果名称表示的有效类属性是一个函数对象，则通过打包（指向）实例对象和刚在类对象中找到的函数对象来创建方法对象：这是方法对象。当使用参数列表调用方法对象时，将从实例对象和参数列表构造新的参数列表，并使用此新参数列表调用函数对象。

<h4 id='9.3.5.'>9.3.5. 类变量和实例变量</h4>

一般来说，实例变量是针对每个实例唯一的数据，而类变量是针对类的所有实例共享的属性和方法：

    class Dog:
    
        kind = 'canine'         # class variable shared by all instances
    
        def __init__(self, name):
            self.name = name    # instance variable unique to each instance
    
    >>> d = Dog('Fido')
    >>> e = Dog('Buddy')
    >>> d.kind                  # shared by all dogs
    'canine'
    >>> e.kind                  # shared by all dogs
    'canine'
    >>> d.name                  # unique to d
    'Fido'
    >>> e.name                  # unique to e
    'Buddy'

正如[关于名称和对象](#9.1.)中讨论的，当共享数据是可变对象时，例如列表和字典，共享数据可能产生令人惊讶的效果。例如，以下代码中的trick列表不应该用作类变量，因为所有Dog实例都共享这个列表：

    class Dog:
    
        tricks = []             # mistaken use of a class variable
    
        def __init__(self, name):
            self.name = name
    
        def add_trick(self, trick):
            self.tricks.append(trick)
    
    >>> d = Dog('Fido')
    >>> e = Dog('Buddy')
    >>> d.add_trick('roll over')
    >>> e.add_trick('play dead')
    >>> d.tricks                # unexpectedly shared by all dogs
    ['roll over', 'play dead']

应该将trick设计成实例变量，这样才能正确设计类：

    class Dog:
    
        def __init__(self, name):
            self.name = name
            self.tricks = []    # creates a new empty list for each dog
    
        def add_trick(self, trick):
            self.tricks.append(trick)
    
    >>> d = Dog('Fido')
    >>> e = Dog('Buddy')
    >>> d.add_trick('roll over')
    >>> e.add_trick('play dead')
    >>> d.tricks
    ['roll over']
    >>> e.tricks
    ['play dead']
    
<h4 id='9.4.'>9.4. Random Remarks</h4>
    
当数据属性和方法属性同名时，数据属性会覆盖方法属性。为了避免名称冲突，名称冲突可能会导致大型程序中难以发现的BUGs，最好使用某种命名约定来最小化名称冲突的可能性。可能的命名约定包括方法名称首字母大写、数据属性使用小而简单的字符串作为前缀（可能只是一个下划线符号），或者方法名称使用动词，数据属性名称使用名词。

数据属性可能被方法所引用，也可能被对象的普通用户（“客户端”）所引用。换句话说，类不可用于实现纯抽象数据类型。事实上，Python中没有任何东西可以强制执行数据隐藏（类不存在完全的私有成员，仍然可以通过某种方式访问私有成员） - 它完全基于约定。（另一方面，用C语言编写的Python实现可以完全隐藏实现细节并在必要时控制对对象的访问;这可以由用C编写的Python的扩展来使用。）

客户端应谨慎使用数据属性 - 客户端可能会通过标记其数据属性来破坏方法维护的不变量。请注意，客户端可以将自己的数据属性添加到实例对象，而不会影响方法的有效性，只要避免名称冲突 - 同样，命名约定可以在这里节省很多麻烦。

从方法中引用数据属性（或其他方法！）没有简写。我发现这实际上增加了方法的可读性：当浏览方法时，没有可能混淆局部变量和实例变量。

通常，方法的第一个参数是`self`。这只不过是一个惯例：这个`self`名称对Python来说绝对没有特殊意义。但请注意，如果不遵循惯例，您的代码可能对其他Python程序员来说可读性较低， 并且可以想象得到一个类浏览程序（类检查程序）可能是遵循这样一个约定编写的。（and it is also conceivable that a class browser program might be written that relies upon such a convention.）

作为类属性的任何函数对象都为该类的实例定义了一个方法。函数定义不必包含在类定义中：将在类定义外部定义的函数对象赋值给类中的局部变量也是可以为类实例定义方法的。例如：

    # Function defined outside the class
    def f1(self, x, y):
        return min(x, x+y)
    
    class C:
        f = f1
    
        def g(self):
            return 'hello world'
    
        h = g

现在`f`, `g`和`h`都是类C的函数对象属性，因此他们也是类C的实例的方法属性---`h`等价于（就是）`g`。请注意，这种做法通常只会使程序的读者感到困惑。

方法可以通过使用`self`参数的方法属性来调用其他方法：

    class Bag:
        def __init__(self):
            self.data = []
    
        def add(self, x):
            self.data.append(x)
    
        def addtwice(self, x):
            self.add(x)
            self.add(x)

方法可以使用与普通函数相同的方式引用全局名称。与方法对应的全局作用域是包含方法定义的模块。（一个类永远不会被用作全局作用域。）虽然很少遇到在方法中使用全局数据的充分理由，但是全局作用域有许多合法用途：一方面，导入全局作用域的函数和模块可以由在全局作用域中定义的方法、函数和类使用。通常，包含该方法的类本身就在此全局作用域内定义，在下一节中，我们将找到一些方法想要引用其自己的类的一些很好的理由。

Python中每个值都是对象，因此都有一个类（也称为其类型）。可以通过`object.__class__`来访问其类型。

<h4 id='9.5.'>9.5. 继承</h4>

当然，如果不支持继承，则这个语言特性就称不上是"类"。定义派生类的语法如下所示：

    class DerivedClassName(BaseClassName):
        <statement-1>
        .
        .
        .
        <statement-N>

名称`BaseClassName`必须在派生类定义所在的作用域中事先定义好。除了使用基类名称，也允许使用其他任意表达式来代替。这可能很有用，例如，如果基类定义在另一个模块中时，可以如下定义派生类：

    class DerivedClassName(modname.BaseClassName):

派生类定义的执行与基类定义的执行相同。构造类对象时，会记住基类。这用于解析属性引用：如果在派生类中找不到请求的属性，则继续在基类中搜索属性。如果基类本身也是从其他类派生的，则递归应用此规则。

关于派生类的实例化没有什么特别之处： `DerivedClassName()`创建一个新的类实例。方法引用按如下方式解析：先在当前类中，搜索相应的类属性，如果在当前类中没有找到，则沿着继承关系链向基类移动，如果返回函数对象，则方法引用有效。

派生类会覆盖其基类的方法。因为方法在调用同一对象的其他方法时没有特殊权限，所以在派生类中，使用某一基类方法`a`（派生类没有重载这个方法）来调用同一基类中定义的另一个方法`b`，如果派生类重载了`b`,则最终可能会调用覆盖它的派生类的方法`b`。（对于C ++程序员：Python中的所有方法都是`virtual`的。）

派生类中的重写方法实际上可能希望扩展同名的基类方法而不是简单地替换同名的基类方法。有一种直接调用基类方法的简单方法：只需调用`BaseClassName.methodname(self, arguments`即可。这对客户来说偶尔也很有用。（请注意，这仅在基类`BaseClassName`可以在全局作用域内访问时才有效。）)

Python有两个内置函数可以用于类的继承：

+ 使用[isinstance()](https://docs.python.org/3.6/library/functions.html#isinstance)检查实例的类型：如果`obj.__class__`是整型[int](https://docs.python.org/3.6/library/functions.html#int)或者整型[int](https://docs.python.org/3.6/library/functions.html#int)的派生类，则`isinstance(obj, int)`返回真值True；
+ 使用[issubclass()](https://docs.python.org/3.6/library/functions.html#issubclass)检查类的继承关系：因为[bool](https://docs.python.org/3.6/library/functions.html#bool)型是整型[int](https://docs.python.org/3.6/library/functions.html#int)的子类（派生类），所以`issubclass(bool, int)`返回真值`True`。

<h4 id='9.5.1'>9.5.1. 多重继承</h4>

Python也支持多重继承的形式。继承多个基类的类定义如下所示：

    class DerivedClassName(Base1, Base2, Base3):
        <statement-1>
        .
        .
        .
        <statement-N>

在大多数情况下，在最简单的情况下，您可以认为对从父类继承的属性的搜索是深度优先搜索，同一深度时从左到右，在继承关系中如果一个类出现多次则只搜索一次（不会搜索两次）。因此，如果在派生类`DerivedClassName`中没有找到属性，则搜索其第一个基类`Base1`，然后一层一层往上（递归地）搜索`Base1`的基类，如果仍找不到，则搜索它的第二个基类`Base2`，然后一层一层往上搜索`Base2`的基类，依此类推。

事实上，方法解析顺序稍微复杂一些; 方法解析顺序动态变化以支持协作调用[super()](https://docs.python.org/3.6/library/functions.html#super)。这种方法在一些其他支持多重继承语言中称为call-next-method，并且比单继承语言中的超级super调用更强大。

动态解析顺序是必要的，因为多重继承的继承关系树大多存在着一个或多个菱形关系（也就是说，其中至少有一个父类可以通过最底层类的多个路径访问）。For example, all classes inherit from [object](https://docs.python.org/3.6/library/functions.html#object), so any case of multiple inheritance provides more than one path to reach [object](https://docs.python.org/3.6/library/functions.html#object). To keep the base classes from being accessed more than once, the dynamic algorithm linearizes the search order in a way that preserves the left-to-right ordering specified in each class, that calls each parent only once, and that is monotonic (meaning that a class can be subclassed without affecting the precedence order of its parents). Taken together, these properties make it possible to design reliable and extensible classes with multiple inheritance. For more detail, see <https://www.python.org/download/releases/2.3/mro/>.

<h4 id='9.6.'>9.6. 私有变量</h4>

Python中不存在除对象内部之外无法访问的“私有”实例变量(Python中的类私有变量也是有方法可以访问的)。但是，大多数Python代码都遵循一个约定：以下划线（例如`_spam`）为前缀的名称应被视为API的非公共部分（无论是函数，方法还是数据成员）。它应被视为实现细节，如有更改，无需通知用户。

由于类私有成员有一个特别有用的用处（即能够避免名称与子类定义的名称冲突），Python为一种名为name mangling的机制提供有限的支持，类似`__spam`形式的任何标识符 （至少两个前缀下划线，最多一个后缀下划线）在文本上替换为`_classname__spam`，其中`classname`是当前类名。只要它出现在类的定义中，就可以在不考虑标识符的句法位置的情况下完成名称修改。

名称修改name mangling有助于在子类中重构方法而不破坏类内方法调用。例如：

    class Mapping:
        def __init__(self, iterable):
            self.items_list = []
            self.__update(iterable)
    
        def update(self, iterable):
            for item in iterable:
                self.items_list.append(item)
    
        __update = update   # private copy of original update() method
    
    class MappingSubclass(Mapping):
    
        def update(self, keys, values):
            # provides new signature for update()
            # but does not break __init__()
            for item in zip(keys, values):
                self.items_list.append(item)

请注意，修剪规则主要是为了避免事故; 它仍然可以访问或修改被视为私有的变量。这在特殊情况下甚至可能很有用，例如在调试器中。

Notice that code passed to `exec()` or `eval()` does not consider the classname of the invoking class to be the current class; this is similar to the effect of the `global` statement, the effect of which is likewise restricted to code that is byte-compiled together. The same restriction applies to `getattr()`, `setattr()` and `delattr()`, as well as when referencing `__dict__` directly.

<h4 id='9.7.'>9.7. 杂七杂八</h4>

有时，使用类似于Pascal“record”或C“struct”的数据类型是有用的，这些数据类型将一些命名数据项捆绑在一起。一个空的类定义可以很好地达到上述效果：

    class Employee:
        pass
    
    john = Employee()  # Create an empty employee record
    
    # Fill the fields of the record
    john.name = 'John Doe'
    john.dept = 'computer lab'
    john.salary = 1000

A piece of Python code that expects a particular abstract data type can often be passed a class that emulates the methods of that data type instead. For instance, if you have a function that formats some data from a file object, you can define a class with methods read() and readline() that get the data from a string buffer instead, and pass it as an argument.

Instance method objects have attributes, too: `m.__self__` is the instance object with the method m(), and `m.__func__` is the function object corresponding to the method.

<h4 id='9.8.'>9.8. 迭代器</h4>

到目前为止，您可能已经注意到大多数容器对象都可以使用[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)语句循环：

    for element in [1, 2, 3]:
        print(element)
    for element in (1, 2, 3):
        print(element)
    for key in {'one':1, 'two':2}:
        print(key)
    for char in "123":
        print(char)
    for line in open("myfile.txt"):
        print(line, end='')

这种访问方式清晰，简洁，方便。迭代器的使用遍及并统一了Python。在背后，[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)语句对容器对象调用[iter()](https://docs.python.org/3.6/library/functions.html#iter)。[iter()](https://docs.python.org/3.6/library/functions.html#iter)函数返回一个迭代器对象，该对象定义了一个一次访问容器对象中一个元素的方法[__next__()](https://docs.python.org/3.6/library/stdtypes.html#iterator.__next__)。当没有更多元素时， [__next__()](https://docs.python.org/3.6/library/stdtypes.html#iterator.__next__)抛出一个[StopIteration](https://docs.python.org/3.6/library/exceptions.html#StopIteration)异常，告诉[for](https://docs.python.org/3.6/reference/compound_stmts.html#for)循环终止。您可以使用[next()](https://docs.python.org/3.6/library/functions.html#next)内置函数调用[__next__()](https://docs.python.org/3.6/library/stdtypes.html#iterator.__next__)方法; 这个例子说明了它是如何工作的：

    >>> s = 'abc'
    >>> it = iter(s)
    >>> it
    <iterator object at 0x00A1DB50>
    >>> next(it)
    'a'
    >>> next(it)
    'b'
    >>> next(it)
    'c'
    >>> next(it)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
        next(it)
    StopIteration

看过迭代器协议背后的机制，很容易为类添加迭代器行为。定义一个返回具有[__next__()](https://docs.python.org/3.6/library/stdtypes.html#iterator.__next__)方法的对象的方法[__iter__()](https://docs.python.org/3.6/reference/datamodel.html#object.__iter__)。如果类定义了[__next__()](https://docs.python.org/3.6/library/stdtypes.html#iterator.__next__)，那么[__iter__()](https://docs.python.org/3.6/reference/datamodel.html#object.__iter__)可以返回`self`：

    class Reverse:
        """Iterator for looping over a sequence backwards."""
        def __init__(self, data):
            self.data = data
            self.index = len(data)
    
        def __iter__(self):
            return self
    
        def __next__(self):
            if self.index == 0:
                raise StopIteration
            self.index = self.index - 1



    >>> rev = Reverse('spam')
    >>> iter(rev)
    <__main__.Reverse object at 0x00A1DB50>
    >>> for char in rev:
    ...     print(char)
    ...
    m
    a
    p
    s

<h4 id='9.9.'>9.9. 生成器</h4>

[Generator](https://docs.python.org/3.6/glossary.html#term-generator)是一个用于创建迭代器的简单而强大的工具。它们像常规函数一样编写，但只要他们想要返回数据就使用[yield](https://docs.python.org/3.6/reference/simple_stmts.html#yield)语句。每次[next()](https://docs.python.org/3.6/library/functions.html#next)调用它时，生成器从它停止的地方恢复（它记住所有数据值和上次执行的语句）。一个例子表明，生成器很容易创建：

    def reverse(data):
        for index in range(len(data)-1, -1, -1):
            yield data[index]
    >>>
    >>> for char in reverse('golf'):
    ...     print(char)
    ...
    f
    l
    o
    g

任何可以使用生成器完成的操作也可以使用基于类的迭代器完成，如上一节中所述。使生成器如此紧凑简短的原因是`__iter__()`和`__next__()`方法是自动创建的。

除了自动创建方法和保存程序状态外，当生成器终止时，它们会自动引发StopIteration。结合使用这些功能，可以轻松创建迭代器，而不需要编写常规函数。

<h4 id='9.10.'>9.10. 生成器表达式</h4>

Some simple generators can be coded succinctly as expressions using a syntax similar to list comprehensions but with parentheses instead of square brackets. These expressions are designed for situations where the generator is used right away by an enclosing function. Generator expressions are more compact but less versatile than full generator definitions and tend to be more memory friendly than equivalent list comprehensions.

Examples:

    >>>
    >>> sum(i*i for i in range(10))                 # sum of squares
    285
    
    >>> xvec = [10, 20, 30]
    >>> yvec = [7, 5, 3]
    >>> sum(x*y for x,y in zip(xvec, yvec))         # dot product
    260
    
    >>> from math import pi, sin
    >>> sine_table = {x: sin(x*pi/180) for x in range(0, 91)}
    
    >>> unique_words = set(word  for line in page  for word in line.split())
    
    >>> valedictorian = max((student.gpa, student.name) for student in graduates)
    
    >>> data = 'golf'
    >>> list(data[i] for i in range(len(data)-1, -1, -1))
    ['f', 'l', 'o', 'g']




## 个人笔记

<h4 id='1.'>1. 块</h4>

1. 块(block)：模块、函数体、类定义、脚本、`command`(Python -c `command`)；
2. 代码块(code block)：脚本、`command`(Python -c `command`)；

<h4 id='2.'>2. 名称Names与对象Objects</h4>

1. 名称：Python中的名称（变量）就是对象的引用；Python是将对象的引用而不是值赋值给名称（变量）；
2. 当一个名称在块中定义，则它是该块的本地（局部）名称（变量）；当一个名称是模块级名称，则它既是模块的本地（局部）名称（变量）又是全局名称（变量）；当一个名称在代码块中使用而没有在代码块中定义时，则它是自由名称（变量）；

<h4 id='3.'>3. 命名空间Namespace与作用域Scope </h4>

1. 命名空间：名称与对象的映射，就像是个字典
    1. 分类：
        + 内置名称空间
        + 全局名称空间
        + 局部名称空间
    2. 生存期：
        + 内置名称空间：Python解释器启动时创建，退出时删除；
        + 全局名称空间：读入模块定义时创建，退出时删除；
        + 局部名称空间：调用函数时创建，调用结束时删除；
    3. 类的定义也会引入命名空间，函数中的再定义函数，类中的成员函数定义会在局部名称空间中再次引入局部名称空间。 

2. 作用域：命名空间的可见性

    Python程序中的几个作用域：

        1. 最内层的局部作用域；
        2. 外层函数的局部作用域；
        3. 模块的全局作用域；
        4. 包含Python内置对象的最外层作用域；


3. `global`与`nonlocal`语句；


