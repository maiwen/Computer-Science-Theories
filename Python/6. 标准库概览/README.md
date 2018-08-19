# 标准库概览

## 1. 操作系统接口
os 模块提供很多函数与操作系统进行交互
``` python
>>> import os
>>> os.getcwd()      # Return the current working directory
'C:\\Python35'
>>> os.chdir('/server/accesslogs')   # Change current working directory
>>> os.system('mkdir today')   # Run the command mkdir in the system shell
0
```
确保使用import os而不是from os import *。这样可以防止函数os.open()覆盖内建函数open()，两者之间的操作是很不同的。


## 2.

glob模块提供一个对目录中的文件进行通配符搜索的函数.
``` python
>>> import glob
>>> glob.glob('*.py')
['primes.py', 'random.py', 'quote.py']

# 命令行参数,常见的实用程序脚本通常需要处理命令行参数。那些参数以列表的形式存储在sys 模块的 argv 属性中
>>> import sys
>>> print(sys.argv)
['demo.py', 'one', 'two', 'three']

# math模块提供基于c库函数的浮点运算.

>>> import math
>>> math.cos(math.pi / 4)
0.70710678118654757
>>> math.log(1024, 2)
10.0

# random 的模块提供了进行随机选择的工具︰

>>> import random
>>> random.choice(['apple', 'pear', 'banana'])
'apple'
>>> random.sample(range(100), 10)   # sampling without replacement
[30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
>>> random.random()    # random float
0.17970987693706186
>>> random.randrange(6)    # random integer chosen from range(6)
4

# statistics 模块计算数值数据的基本统计特性 （均值、 中位数、 方差，等）。:


>>> import statistics
>>> data = [2.75, 1.75, 1.25, 0.25, 0.5, 1.25, 3.5]
>>> statistics.mean(data)
1.6071428571428572
>>> statistics.median(data)
1.25
>>> statistics.variance(data)
1.3720238095238095

# 互联网访问，有很多的模块用于访问互联网和处理的互联网协议。最简单的两个是用于网络访问的 urllib.request 和用于发送邮件的 smtplib
>>> from urllib.request import urlopen
>>> with urlopen('http://tycho.usno.navy.mil/cgi-bin/timer.pl') as response:
...     for line in response:
...         line = line.decode('utf-8')  # Decoding the binary data to text.
...         if 'EST' in line or 'EDT' in line:  # look for Eastern Time
...             print(line)

<BR>Nov. 25, 09:43:32 PM EST

>>> import smtplib
>>> server = smtplib.SMTP('localhost')
>>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
... """To: jcaesar@example.org
... From: soothsayer@example.org
...
... Beware the Ides of March.
... """)
>>> server.quit()

# 日期和时间，datetime模块提供了处理日期和时间的简单和复杂的方法。支持日期和时间算法的同时，实现的重点放在更有效的处理和格式化输出。该模块还支持处理时区。

>>> # dates are easily constructed and formatted
>>> from datetime import date
>>> now = date.today()
>>> now
datetime.date(2003, 12, 2)
>>> now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
'12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'

>>> # dates support calendar arithmetic
>>> birthday = date(1964, 7, 31)
>>> age = now - birthday
>>> age.days
14368

# 数据压缩
# 通常的数据归档和压缩格式由以下模块直接支持，包括：zlib，gzip，bz2，lzma zipfile和tarfile。
>>> import zlib
>>> s = b'witch which has which witches wrist watch'
>>> len(s)
41
>>> t = zlib.compress(s)
>>> len(t)
37
>>> zlib.decompress(t)
b'witch which has which witches wrist watch'

#一些 Python 用户对同一问题的不同解决方法之间的性能差异深有兴趣。Python 提供了的一个度量工具可以立即解决这些问题。timeit模块能够快速演示一个适度的性能优势：

>>>
>>> from timeit import Timer
>>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
0.57535828626024577
>>> Timer('a,b = b,a', 'a=1; b=2').timeit()
0.54962537085770791
#与timeit的精细粒度相对比，profile and pstats 模块提供了识别时间跨度较大的代码的工具。

# decimal模块提供具有所需精度的算术操作：
>>> getcontext().prec = 36
>>> Decimal(1) / Decimal(7)
Decimal('0.142857142857142857142857142857142857')
```

## 3. 质量控制
开发高质量软件的方法之一是为每一个函数编写测试代码，并且在开发过程中经常性的运行这些测试代码。

doctest模块为 扫描模块 和 验证测试 提供了一个嵌入程序文档中的工具。测试的构造像一个把结果剪切并粘贴到文档字符串的典型调用一样简单。通过用户提供的例子，它发展了文档，允许 doctest 模块确认代码的结果是否与文档一致：
``` python
def average(values):
    """Computes the arithmetic mean of a list of numbers.

    >>> print(average([20, 30, 70]))
    40.0
    """
    return sum(values) / len(values)

import doctest
doctest.testmod()   # automatically validate the embedded tests
```

unittest模块并不像doctest模块那么轻松，但它允许在单独的文件中维护一组更全面的测试：
``` python
import unittest

class TestStatisticalFunctions(unittest.TestCase):

    def test_average(self):
        self.assertEqual(average([20, 30, 70]), 40.0)
        self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
        with self.assertRaises(ZeroDivisionError):
            average([])
        with self.assertRaises(TypeError):
            average(20, 30, 70)

unittest.main()  # Calling from the command line invokes all tests
```