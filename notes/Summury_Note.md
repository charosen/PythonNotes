# Python 归纳

<h3 id='1.'>1. 对象、类型和值</h3>

<h4 id='1.1.'>1.1. Python 对象</h4>

Python中的一切数据都是对象。对象有3个特性：身份(Identity)、类型(Type)、值(Value)。

+ 身份：对象的身份可视为对象的内存地址。'[is][0]'运算符比较两个对象的身份；[id()][1]内置函数返回对象的内存地址（身份）；
+ 类型：对象的类型决定对象支持的操作、可能值以及是否可变；[type()][2]内置函数返回对象的类型（Python中一切都是对象，类型也是对象）；
+ 值：根据值是否可变，对象可以分为可变对象和不可变对象；当讨论容器对象的可改变性时，是通过容器对象中包含的对象的身份是否可变来判断的。

[0]: https://docs.python.org/3.6/reference/expressions.html#is
[1]: https://docs.python.org/3.6/library/functions.html#id
[2]: https://docs.python.org/3.6/library/functions.html#type

<h4 id='1.2.'>1.2. 标准类型机制The standard type hierarchy</h4>

1. None
2. NotImplemented：当算术方法和比较方法无法对给定的操作数进行操作时，则返回`NotImplemented`；
3. Ellipsis(省略类型)：可以通过`...`和内置名称`Ellipsis`访问；
4. [numbers.Number][3]（数字类型)：不可变Immutable
    1. [numbers.Integral][4]-subtype->[int][5]-subtype->[bool][6]；
    2. [numbers.Real][7]([float][])：双精度浮点数；
    3. [numbers.Complex][8]([complex][])：一对双精度浮点数；
5. Sequences(序列类型)：使用非负整数索引的、有序的有限集合；
    1. Immutable sequences(不可变序列类型)：
        1. Strings(字符串)；
        2. Tuples(元组)；
        3. Bytes(字节)。
    2. Mutable sequences(可变序列类型)：
        1. Lists(列表)；
        2. Byte arrays(字节数组类型)。
6. Set types(集合类型)：元素各不相同且不可变对象的、无序的有限集合；
    1. Sets(集合)：可变mutable；
    2. Frozen sets：不可变immutable；
7. Mappings(映射类型)：
    1. Dictionaries(字典)：可变mutable;
8. Callable types(可调用类型)：
    1. User-defined functions(用户自定义函数);
    2. Instance methods(实例方法);
    3. Generator functions(生成器函数)：使用[yield][9]语句定义的函数或方法；
    4. Coroutine functions(协同函数)：使用[async def][10]定义的函数或方法；
    5. Asynchronous generator functions(异步生成器函数)：使用[async def][10]和[yield][9]语句定义的函数或方法；
    6. Built-in functions(内置函数)；
    7. Built-in methods(内置方法)；
    8. Classes(类)；
    9. Class Instances(类实例)：当类定义了[__call__()][11]方法，则类实例为可调用类型；
9. Modules(模块类型)：
    1. Custom classes(定制类)：由类定义创建的类，同时也是可调用类型；
    2. Class Instances(类实例)；
    3. I/O objects(文件对象类型)；
    4. Internal types(内部类型)：暴露给用户的、解释器内部使用的类型；
        1. Code objects(代码对象类型)
        2. Frame objects(框架对象类型)
        3. Traceback objects(回溯对象类型)
        4. Slice objects(切片对象类型)
        5. Static method objects(静态方法对象)
        6. Class method objects(类方法对象)

[3]: https://docs.python.org/3.6/library/numbers.html#numbers.Number
[4]: https://docs.python.org/3.6/library/numbers.html#numbers.Integral
[5]: https://docs.python.org/3.6/library/functions.html#int
[6]: https://docs.python.org/3.6/library/functions.html#bool
[7]: https://docs.python.org/3.6/library/numbers.html#numbers.Real
[float]: https://docs.python.org/3.6/library/functions.html#float
[8]: https://docs.python.org/3.6/library/numbers.html#numbers.Complex
[complex]: https://docs.python.org/3.6/library/functions.html#complex
[9]: https://docs.python.org/3.6/reference/simple_stmts.html#yield
[10]: https://docs.python.org/3.6/reference/compound_stmts.html#async-def
[11]: https://docs.python.org/3.6/reference/datamodel.html#object.__call__

<h3 id='2.'>2. 运算符</h3>

大部分类型都支持布尔运算和比较运算。

<h4 id='2.1.'>2.1. 布尔运算</h4>

1. 布尔运算符：非[not][], 与[and][], 或[or][]；与、或运算符是短路运算符，从左到右计算，一旦确定结果则不计算后面的表达式。（当与运算符的左侧表达式返回值为`False`，则结果必为`False`，不再计算右侧表达式；当或运算符的左侧表达式返回值为`True`，则结果必为`True`，不再计算右侧表达式）。
2. 优先级：[not][] > [and][] > [or][]

[not]: https://docs.python.org/3.6/reference/expressions.html#not
[and]: https://docs.python.org/3.6/reference/expressions.html#and
[or]: https://docs.python.org/3.6/reference/expressions.html#or

<h4 id='2.2.'>2.2. 比较运算</h4>

1. 比较运算符：不同身份[is not][], 相同身份[is][], 不相等`!=`, 相等`==`, 大于等于`>=`, 大于`>`, 小于等于`<=`, 小于`<`；比较运算符可以连接起来，等价于各比较运算用与运算符`and`连接起来，从左到右计算；
2. 优先级：比较运算符具有相同优先级;

[is not]: https://docs.python.org/3.6/reference/expressions.html#is-not
[is]: https://docs.python.org/3.6/reference/expressions.html#is

<h3 id='3.'>3. 数字类型Number(不可变类型)</h3>

<h4 id='3.1.'>3.1. 内置数字类型</h4>

1. 整型`int`：表示整数
    1. 进制表示：
    
            二进制：以`0b`或`0B`开头；
                八进制：以`0o`或`0O`开头；
                十六进制：以`0x`或`0X`开头；
                十进制：默认十进制，以非0数字开头，其他进制可以以0开头；
            
    2. 内置方法：`bit_length()`, `to_bytes()`, `from_bytes()`;
    
2. 布尔型`bool`表示逻辑真假
     1. 布尔测试：值为`False`的对象如下：
            + 定义为`False`的常量：`None`和`False`
            + 任意数字类型的0：`0`、`0.0`、`0j`、`Decimal(0)`、`Fraction(0, 1)`;
            + 空（长度为0）序列和空集合：`''`、`()`、`[]`、`{}`、`set()`、`range(0)`；
            + 定义了以`False`为返回值的[__bool__()][]方法或者定义了以`0`为返回值的[__len__()][]方法的类的对象；
    
1. 浮点型`float`表示带小数部分的数
    1. 小数：小数点表示；
    2. 指数：`e`或`E`表示；
    3. 内置方法：`as_integer_ratio()`、`hex()`、`from_hex()`；
    
2. 复数`complex`表示复数（后缀`j`或`J`表示虚部）；
    1. 属性：实部`z.real`、虚部`z.imag`；
    2. 内置方法：取共轭`conjugate()`；

PS: 下划线`_`可以在整型、浮点型、复数中使用，用于分组数字，看清位数

<h4 id='3.2.'>3.2. 拓展数字类型</h4>

拓展数字类型：十进制浮点型`Decimal`和有理数`Fraction`；

<h4 id='3.3.'>3.3. 算术运算</h4>

+ 算术运算符：`+`,`-`,`*`,`/`,`//`,`%`,`**`;除法(`/`)返回浮点数（精确结果），地板除(`//`)返回整型（精确结果向下取整）；
+ 算术运算符优先级：`**` > `pow(x, y)` > `divmod(x, y)` > `c.conjugate()` > `complex(re, im)` > `float()` > `int()` > `abs()` > `+`(正) > `-`(负) > `%` > `//` > `/` > `*` > `-` (减) > `+`(加)；幂运算符的优先级比左操作数的单目运算符高，比右操作数的单目运算符低。

<h4 id='3.4.'>3.4. 整数的位运算</h4>

1. 位运算符：按位取反`~`、右移位`>>`、左移位`<<`、按位与`&`、按位异或`^`、按位或`|`；
2. 优先级：`~` > `>>` > `<<` > `&` > `^` > `|`;

<h4 id='3.5.'>3.5. Odds and ends</h4>

在交互解释器中，最后打印的表达式的结果赋值给变量`_`，该变量应被用户视作只读，不可为其显示赋值。

[__bool__()]: https://docs.python.org/3.6/reference/datamodel.html#object.__bool__
[__len__()]: https://docs.python.org/3.6/reference/datamodel.html#object.__len__

<h3 id='4.'>4. 序列类型Sequences</h3>

<h4 id='4.1.'>4.1. 通用序列运算(操作)</h4>

1. 正负索引，普通索引超过范围会报错；
2. 切片
        
        a. 始终包含开头，不包含结束，即s[i:] + s[:i] = s；
        b. 第一个索引默认为0，第二个索引默认为序列长度；
        c. 切片索引超出范围不会报错
        d. 所有索引操作返回都是包含所请求元素的原序列的浅拷贝。
    
3. 重载`+`, `*`;
4. 内置方法：`len(sequence)`、`min(sequence)`、`max(sequence)`、`sequence.inde(x[, i[, j]])`、`sequence.count(x)`；
5. 优先级：`s.count(x)` > `s.index(x[, i[, j]])` > `max(s)` > `min(s)` > `len(s)` > 切片 > 正负索引 > `*` > `+` > `not in ` > `in`；

<h4 id='4.2.'>4.2. 不可变序列通用操作</h4>

内置函数[hash()][]

[hash()]: https://docs.python.org/3.6/library/functions.html#hash

<h4 id='4.3.'>4.3. 可变序列通用操作</h4>



<h4 id='4.4.'>4.4. 字符串类型Strings(不可变类型、序列类型)</h4>

1. `print()`调用`str()`输出结果，交互解释器调用`repr()`输出结果；
2. `str()`和`repr()`比较：
    + `repr()`:面向程序开发者，除了输出数据内容，还会输出数据类型信息，不转义；
    + `str()`:面向用户，以简洁格式输出数据内容，转义；
    + 输出：
    
        | 同 | Numbers, Lists, Tuples, Dicts |
        | --- | --- |
        | 异 | Strings |


3. Python中有4种字符串文字String literal:
    + 普通字符串：单引号`'...'`或双引号`"..."`括起；
    + 原始字符串：以`r`开头的字符串，例如`r'...'`，不转义。 
    + 多行字符串：三个单引号或双引号括起来。换行符自动包含在字符串每行中，但可以通过在行尾加反斜线`\`来去除换行符；
    + 格式化字符串([formatted string literals](https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings))：以`f`开头的字符串；
    + 两个或多个相邻字符串文字会自动连接，过长的字符串文字可以如下跨行：
    
            ('...'
            '....')

4. 字符串特性：正负索引、切片、运算符重载、内置方法
   
