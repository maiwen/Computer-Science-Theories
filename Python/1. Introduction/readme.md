# 1. 引言

如果你在电脑上做了很多工作，最终你会发现有一些任务你想要自动化。例如，你可能希望对大量的文本文件执行搜索和替换，或者以复杂的方式重命名并排列一堆照片文件。也许你想写一个小的自定义数据库，或专门的GUI应用程序，或一个简单的游戏。

如果你是一个专业的软件开发人员，您可能必须使用几个 C / C + + /Java 库，但发现通常的编写/编译/测试/重新编译周期太慢。也许你要写这样的库中的测试套件，然后发现编写测试代码是很乏味的工作。或许您编写了一个程序，它可以使用一种扩展语言，但你不想为您的应用程序来设计与实现一个完整的新语言。

Python正是你所需要的语言。

你可以为其中一些任务写一个 Unix shell 脚本或 Windows 批处理文件，但是 shell 脚本最适合处理文件移动和文本编辑，而不适用于 GUI 应用程序和游戏。你可以用C / C + +/Java写一个 程序，但是可能会花费大量的开发时间去完成一份初稿。Python更易于使用，在Windows，Mac OS X和Unix操作系统上可用，并且将帮助您更快地完成工作。

Python使用起来很简单，但它是一种真正的编程语言，与shell脚本或批处理文件相比，它可以为大型程序提供更多的结构和支持。另一方面，Python还提供比C更多的错误检查，并且作为非常高级语言，它具有内置的高级数据类型，例如灵活的数组和字典。因为其丰富的更加通用的数据类型， Python 的适用领域比 Awk 甚至 Perl 要广泛得多，而且很多事情在 Python 中至少和那些语言一样容易。

Python 允许您将您的程序拆分成可以在其他 Python 程序中再次使用的模块。它有一大批的标准模块，你可以用它作为你的程序的基础 - 或者作为例子开始学习使用Python编程。这些模块提供诸如文件 I/O、 系统调用、 套接字，甚至还为像 Tk 这样的图形界面开发包提供了接口。

Python 是一门解释性的语言，因为没有编译和链接，它可以节省你程序开发过程中的大量时间。Python 解释器可以交互地使用，这使得试验Python语言的特性、编写用后即扔的程序或在自底向上的程序开发中测试功能非常容易。它也是一个方便的桌面计算器。

Python 使程序编写起来能够简洁易读。编写的 Python 程序通常比等价的 C、 C + + 或 Java 程序短很多，原因有几个：

* 高级数据类型允许您在单个语句中来表达复杂的操作；
* 语句分组是通过缩进，而不是开始和结束的括号 ；
* 不需要为变量或参数进行声明。
Python是可扩展的：如果你会C语言，可以很容易地向解释器添加一个新的内建函数或模块，以最快速度执行关键操作，或者链接Python程序到只能以二进制形式可用的库（例如供应商特定的图形库）。一旦你真的着迷，你可以把 Python 解释器链接到 C 编写的应用程序中，并把它当作那个程序的扩展或命令行语言。

顺便说一句，Python 语言的名字来自于BBC 的 “Monty Python’s Flying Circus” 节目，与爬行动物无关。我们不仅允许，甚至鼓励您在代码中引用Monty Python短剧！

# 2. 简介

* 如果变量未"定义"（即未赋值），使用的时候将会报错︰
``` python
>>> n  # try to access an undefined variable
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'n' is not defined
```

# 2.1  数字
* 除法(/)总是返回一个float类型数。要做 floor 除法 并且得到一个整数结果(返回商的整数部分) 可以使用 // 运算符.

分数：
``` python
>>> Fraction(16, -10)
Fraction(-8, 5)
```

复数：
用 j 或 J 后缀（例如指示的虚部3+5j）。

# 2.2 字符串

如果字符串中只有单引号而没有双引号，就用双引号引用，否则用单引号引用。

如果你不想让 \ 被解释为特殊字符开头的字符，您可以通过添加 r 使用 原始字符串︰
```python
>>> print('C:\some\name')  # here \n means newline!
C:\some
ame
>>> print(r'C:\some\name')  # note the r before the quote
C:\some\name
```

字符串可以用+操作符连接，也可以用*操作符重复多次.相邻的两个或多个字符串字面量（用引号引起来的）会自动连接。

Python没有单独的字符类型；一个字符就是一个简单的长度为1的字符串。

Python 字符串不能更改 — — 他们是 不可变的。因此，赋值给字符串索引的位置会导致错误.如果你需要一个不同的字符串，你应该创建一个新的.
```python
>>> word[0] = 'J'
  ...
TypeError: 'str' object does not support item assignment
>>> word[2:] = 'py'
  ...
TypeError: 'str' object does not support item assignment

>>> 'J' + word[1:]
'Jython'
>>> word[:2] + 'py'
'Pypy'
```

# 2.3 列表
与字符串的不可变特性不同，列表是可变的类型.


# 3. 流程控制
# 3.1 语句
一个if ... elif ... elif ...序列 是对其他语言的switch or case 语句的替代方案。

Python 的 for 语句可以按照元素出现的顺序迭代任何序列（列表或字符串）。
如果要在循环内修改正在迭代的序列（例如，复制所选的项目），建议首先创建原序列的拷贝。迭代序列不会隐式地创建副本。
使用切片就可以很容易地做到.
```python
>>> # Measure some strings:
... words = ['cat', 'window', 'defenestrate']
>>> for w in words:
...     print(w, len(w))
...
cat 3
window 6
defenestrate 12

>>> for w in words[:]:  # Loop over a slice copy of the entire list.
...     if len(w) > 6:
...         words.insert(0, w)
...
>>> words
['defenestrate', 'cat', 'window', 'defenestrate']
```

如果你确实需要遍历一个数字序列，内置函数 range() 会派上用场。它生成算术级数.
如果你只打印 range，会出现奇怪的结果：
```python
>>> print(range(10))
range(0, 10)
```
在很多方面由 range() 返回的对象的行为就像它是一个列表，但事实上它不是。它是一个对象。
当您遍历它时， 它会返回所需的序列连续项。但它并不真的生成了列表，从而节省了空间。
我们说这样一个对象是iterable，就是说，它适合作为一个函数和结构的目标，而函数期望从中获得连续的项目，直到耗尽。
我们已经看到 for 语句是这种 迭代器。list() 函数是另一个;它从可迭代量创建列表︰
```python
>>> list(range(5))
[0, 1, 2, 3, 4]
```

循环中的else子句与try语句的else子句有更多的共同点：try语句的else子句在未出现异常时运行，循环的else子句在未出现break时运行。

# 3.2 定义函数
关键字 def 引入函数的定义。其后必须跟有函数名和以括号标明的形式参数列表。组成函数体的语句从下一行开始，且必须缩进。

函数体的第一行可以是一个可选的字符串文本；此字符串是该函数的文档字符串，或称为docstring。
有工具使用 docstrings 自动生成在线的或可打印的文档，或者让用户在代码中交互浏览；在您编写的代码中包含 docstrings 是很好的做法，所以让它成为习惯吧。

执行 一个函数会引入一个用于函数的**局部变量的新符号表**。
更确切地说，函数中的**所有的赋值都是将值存储在局部符号表**；
而变量引用首先查找局部符号表，然后是上层函数的局部符号表，然后是全局符号表，最后是内置名字表。
因此，在函数内部无法给一个全局变量直接赋值（除非在一个 global 语句中命名），虽然可以引用它们。

当函数被调用时候，函数调用的实际参数被引入到被调用函数的局部（本地）符号表中；
因此，参数传递通过 传值调用 （这里的 值 始终是**对象的引用**，不是对象的值）。
一个函数调用另一个函数时，会为本次调用创建一个新的局部符号表。
```python
a = 1
def fun(a):
    print "func_in",id(a)   # func_in 41322472
    a = 2
    print "re-point",id(a), id(2)   # re-point 41322448 41322448
print "func_out",id(a), id(1)  # func_out 41322472 41322472
fun(a)
print a  # 1

a = []
def fun(a):
    print "func_in",id(a)  # func_in 53629256
    a.append(1)
print "func_out",id(a)     # func_out 53629256
fun(a)
print a  # [1]
```
这里记住的是类型是属于对象的，而不是变量。而对象有两种,“可更改”（mutable）与“不可更改”（immutable）对象。
在python中，strings, tuples, 和numbers是不可更改的对象，而 list, dict, set 等则是可以修改的对象。(这就是这个问题的重点)

## 3.2.1 函数参数的定义
可以定义具有可变数目的参数的函数。有三种函数形式，可以结合使用。

### 1. 默认参数值
```python
# 默认参数值，最有用的形式是指定一个或多个参数的默认值。这种方法创建的函数被调用时，可以带有比定义的要少的参数。
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

```
这个函数可以通过几种方式调用：
只提供必须的参数: ask_ok('Do you really want to quit?')
提供可选参数中的一个: ask_ok('OK to overwrite the file?', 2)
或者提供所有参数: ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')

重要的警告︰默认值只初始化一次。当默认值是一个可变对象（如列表，字典或大多数类的实例）时，默认值会不同。
```python
i = 5
def f(arg=i):
    print(arg)
i = 6
f()    # 会打印 5.

def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
# 这将会打印
[1]
[1, 2]
[1, 2, 3]
```

### 2. 关键字参数
函数还可以用kwarg=value形式的关键字参数调用。
*name形式， 这种形式接收一个元组；**name形式，它将接收一个字典。

def cheeseshop(kind, *arguments, **keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in arguments:
        print(arg)
    print("-" * 40)
    keys = sorted(keywords.keys())
    for kw in keys:
        print(kw, ":", keywords[kw])
它可以这样被调用：

cheeseshop("Limburger", "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")
并且当然它会打印：

 -- Do you have any Limburger ?
 -- I'm sorry, we're all out of Limburger
It's very runny, sir.
It's really very, VERY runny, sir.
 ----------------------------------------
client : John Cleese
shopkeeper : Michael Palin
sketch : Cheese Shop Sketch

### 3. 任意参数的列表
最后，最不常用的场景是指明某个函数可以被可变个数的参数调用。这些参数将被封装在一个元组中。
通常，这些可变的参数将位于形式参数列表的最后面，因为它们将剩下的传递给函数的所有输入参数都包含进去。
``` python
>>> def concat(*args, sep="/"):
...     return sep.join(args)
...
>>> concat("earth", "mars", "venus")
'earth/mars/venus'
>>> concat("earth", "mars", "venus", sep=".")
'earth.mars.venus'
```
## 3.2.2 Lambda 表达式
可以使用 lambda关键字创建小的匿名函数。此函数会返回两个参数的总和： lambda a, b: a+b.。

另一种用法是将一个小函数作为参数传递：
``` python
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

## 3.2.3 文档字符串
```
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
```

# 4. 代码风格
若要编写更长更复杂的 Python 代码，是时候谈一谈 编码风格了 。大部分语言都可以有多种（比如更简洁，更格式化）写法，有些写法可以更易读。让你的代码更具可读性，而良好的编码风格对此有很大的帮助。

对Python， PEP 8 已经成为多数项目遵循的代码风格指南；它推动了一种非常易于阅读且赏心悦目的编码风格。每个Python开发者都应该找个时间读一下； 以下是从中提取出来的最重要的一些点：

* 使用 4 个空格的缩进，不要使用制表符。

4 个空格是小缩进（允许更深的嵌套）和大缩进（易于阅读）之间很好的折衷。制表符会引起混乱，最好弃用。

* 折行以确保其不会超过 79 个字符。

* 这有助于小显示器用户阅读，也可以让大显示器能并排显示几个代码文件。

* 使用空行分隔函数和类，以及函数内的大块代码。

* 如果可能，注释单独成行。

* 使用文档字符串。

* 在操作符两边和逗号之后加空格， 但不要直接在左括号后和右括号前加： a = f(1, 2) + g(3, 4).

* 类和函数的命名风格要一致；传统上使用 CamelCase （驼峰风格）命名类 而用 lower_case_with_underscores（小写字母加下划线）命名函数和方法。方法的第一个参数名称应为 self (查看 初识类 以获得更多有关类和方法的规则)。

* 如果您的代码要在国际环境中使用，不要使用花哨的编码。Python 默认的 UTF-8 或者 ASCII 在任何时候都是最好的选择。

* 同样，只要存在哪怕一丁点可能性有使用另一种不同语言的人会来阅读或维护你的代码，就不要在标识符中使用非 ASCII 字符。