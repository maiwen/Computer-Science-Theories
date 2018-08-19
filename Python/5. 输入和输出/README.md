# 输入和输出

展现程序的输出有多种方法；可以打印成人类可读的形式，也可以写入到文件以便后面使用.

## 1. 格式化输出

通常你会希望更好地控制输出的格式而不是简单地打印用空格分隔的值。有两种方式格式化你的输出：

第一种方式是自己做所有字符串处理；使用字符串切片和串联操作，你可以创建任何你可以想象的布局。字符串类型有一些方法，用于执行将字符串填充到指定列宽度的有用操作；这些稍后将讨论。
字符串对象的str.rjust()方法，它通过在左侧填充空格使字符串在给定宽度的列右对齐。

第二种方法是使用 str.format() 方法。

str.format()方法的基本用法如下所示：
``` python
>>>
>>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))
We are the knights who say "Ni!"
# 字符串内部的花括号和字符（叫做格式字段）将被传递给str.format()方法的对象所替换。花括号中的数字可以用来引用传递给str.format()方法的对象的位置。

>>>
>>> print('{0} and {1}'.format('spam', 'eggs'))
spam and eggs
>>> print('{1} and {0}'.format('spam', 'eggs'))
eggs and spam
# 如果str.format()方法中用到关键字参数，那么它们的值通过参数的名称引用。

>>>
>>> print('This {food} is {adjective}.'.format(
...       food='spam', adjective='absolutely horrible'))
This spam is absolutely horrible.
# 位置参数和关键字参数可以随意组合：

>>>
>>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
                                                       other='Georg'))
The story of Bill, Manfred, and Georg.

# 字段名后允许可选的':'和格式指令。这允许更好地控制值是何种格式。下面的例子将 Pi 转为三位精度。
>>> import math
>>> print('The value of PI is approximately {0:.3f}.'.format(math.pi))
The value of PI is approximately 3.142.
```

## 2. 读写文件
open()返回一个文件对象，最常见的用法带有两个参数：open(filename, mode)。

>>> f = open('workfile', 'w')
参数1是含有文件名的字符串。参数2也是字符串，以几个字符说明该文件的使用方式。mode为'r'表示文件只读；w表示文件只进行写入（已存在的同名文件将被删掉）；'a'表示打开文件进行追加；写入到文件中的任何数据将自动添加到末尾。'r+'表示打开文件进行读取和写入.

在mode后面附加'b'将以二进制模式打开文件：现在数据以字节对象的形式读取和写入。这个模式应该用于所有不包含文本的文件。

### 2.1. 文件对象的方法

要读取文件内容，可以调用f.read(size) ，该方法读取若干数量的数据并以字符串（在文本模式中）或字节对象（在二进制模式中）形式返回它。size 是可选的数值参数。当 size被省略或为负数时，将会读取并返回整个文件

f.readline()从文件读取一行数据；字符串结尾会带有一个换行符 (\n) ，只有当文件最后一行没有以换行符结尾时才会省略。

你可以循环遍历文件对象来读取文件中的每一行。这让内存高效，快速，并简化代码：
``` python
>>>
>>> for line in f:
...     print(line, end='')
...
This is the first line of the file.
Second line of the file
```
如果你想要读取文件列表中所有行的数据，你也可以使用 list(f) 或 f.readlines()。

f.write(string) 将 字符串 的内容写入到该文件，返回写入的字符数。其他类型的对象,在写入之前则需要转换成 字符串 （在文本模式下） 或 字节对象 （以二进制模式）.

使用完一个文件后，调用f.close()可以关闭它并释放其占用的所有系统资源。

处理文件对象时使用with关键字是很好的做法。这样做的好处在于文件用完后会自动关闭，即使过程中发生异常也没关系。它还比编写一个等同的try-finally语句要短很多：
``` python
>>>
>>> with open('workfile', 'r') as f:
...     read_data = f.read()
>>> f.closed
True
```

### 2.2 使用json存储结构化数据

Python 允许你使用常用的数据交换格式JSON（JavaScript Object Notation）。标准模块json可以接受 Python 数据结构，并将它们转换为字符串表示形式；此过程称为序列化。从字符串表示形式重新构建数据结构称为反序列化。
在序列化和反序列化之间，表示对象的字符串可能已经存储在文件或数据中，或者通过网络连接发送到一些远程机器。

如果f是为写入而打开的一个文件对象，我们可以这样做：
```
json.dump(x, f)
```
为了重新解码对象，如果f是为读取而打开的文本文件对象 ：
```
x = json.load(f)
```