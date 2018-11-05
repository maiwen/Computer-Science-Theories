# 面试题


## 1. 静态方法(staticmethod),类方法(classmethod)和实例方法

```python
def foo(x):
    print "executing foo(%s)"%(x)

class A(object):
    def foo(self,x):
        print "executing foo(%s,%s)"%(self,x)

    @classmethod
    def class_foo(cls,x):
        print "executing class_foo(%s,%s)"%(cls,x)

    @staticmethod
    def static_foo(x):
        print "executing static_foo(%s)"%x

a=A()
```
| \\      | 实例方法     | 类方法            | 静态方法            |
| :------ | :------- | :------------- | :-------------- |
| a = A() | a.foo(x) | a.class_foo(x) | a.static_foo(x) |
| A       | 不可用      | A.class_foo(x) | A.static_foo(x) |

## 2. 单下划线和双下划线
`__foo__`:一种约定,Python内部的名字,用来区别其他用户自定义的命名,以防冲突，就是例如`__init__()`,`__del__()`,`__call__()`这些特殊方法

`_foo`:一种约定,用来指定变量私有.程序员用来指定私有变量的一种方式.不能用from module import * 导入，其他方面和公有一样访问；

`__foo`:这个有真正的意义:解析器用`_classname__foo`来代替这个名字,以区别和其他类相同的命名,它无法直接像公有成员一样随便访问,
通过对象名._类名__xxx这样的方式可以访问.

## 3. 迭代器和生成器

迭代器的用法在 Python 中普遍而且统一。在后台，for语句调用容器对象的iter()方法。该函数返回一个定义了__next__()方法的迭代器对象，
它一次访问容器中的一个元素。迭代器只能前进不能后退,使用迭代器可以不用事先准备好带跌过程中的所有元素,
仅仅是在迭代到该元素的时候才计算该元素,而在这之前的元素则是被销毁,因此迭代器适合遍历一些数据量巨大的无限的序列.
没有后续的元素时，__next__() 会引发StopIteration 异常，告诉 for循环停止迭代。你可以使用内建的 next() 来调用 __next__().

生成器是一种可以简单有效的创建迭代器的工具。它们像常规函数一样撰写，但是在需要返回数据时使用yield语句。
每当对它调用next()，生成器从它上次停止的地方重新开始（它会记住所有的数据值和上次执行的语句）。
``` python
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
  File "<stdin>", line 1, in ?
    next(it)
StopIteration

def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]

>>> for char in reverse('golf'):
...     print(char)
...
f
l
o
g
```

 将列表生成式中[]改成() 之后数据结构是否改变？ 答案：是，从列表变为生成器
``` python
>>> L = [x*x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x*x for x in range(10))
>>> g
<generator object <genexpr> at 0x0000028F8B774200>
```
通过列表生成式，可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含百万元素的列表，不仅是占用很大的内存空间，
如：我们只需要访问前面的几个元素，后面大部分元素所占的空间都是浪费的。
因此，没有必要创建完整的列表（节省大量内存空间）。在Python中，我们可以采用生成器：边循环，边计算的机制—>generator

## 4.装饰器，闭包
闭包的定义：简单来说，闭包的概念就是当我们在函数内定义一个函数时，这个内部函数使用了外部函数的临时变量，
且外部函数的返回值是内部函数的引用时，我们称之为闭包。有点绕，上代码：
``` python
# 一个简单的实现计算平均值的代码

def get_avg():
    scores = []  # 外部临时变量

    def inner_count_avg(val):  # 内部函数，用于计算平均值
        scores.append(val)  # 使用外部函数的临时变量
        return sum(scores) / len(scores)  # 返回计算出的平均值

    return inner_count_avg  # 外部函数返回内部函数引用

avg = get_avg()
print(avg(10))  # 10
print(avg(11))  # 10.5
```
装饰器:装饰器的本质是一个闭包函数,他的作用就是让其他函数在不需要做任何代码修改的前提下增加额外功能,装饰器的返回值也是一个函数对象.
我们通常在一些有切面需求的场景,比如:插入日志,性能测试,事务处理,缓存,权限校验等场景,有了装饰器我们就可以少写很多重复代码,提高工作效率.



装饰器是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有插入日志、性能测试、事务处理等。
装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。
概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。
``` python
from functools import wraps

def memo(func):
    cache={}
    @wraps(func)
    def wrap(*args):
        if args not in cache:
            cache[args]=func(*args)
        return cache[args]
    return wrap

@memo
def fib(i):
    if i<2: return 1
    return fib(i-1)+fib(i-2)

print(fib(100))
```

## 5. 函数重载
函数重载主要是为了解决两个问题。

1. 可变参数类型。
2. 可变参数个数。

另外，一个基本的设计原则是，仅仅当两个函数除了参数类型和参数个数不同以外，其功能是完全相同的，此时才使用函数重载，如果两个函数的功能其实不同，那么不应当使用重载，而应当使用一个名字不同的函数。

好吧，那么对于情况 1 ，函数功能相同，但是参数类型不同，python 如何处理？答案是根本不需要处理，因为 python 可以接受任何类型的参数，如果函数的功能相同，那么不同的参数类型在 python 中很可能是相同的代码，没有必要做成两个不同函数。

那么对于情况 2 ，函数功能相同，但参数个数不同，python 如何处理？大家知道，答案就是缺省参数。对那些缺少的参数设定为缺省参数即可解决问题。因为你假设函数功能相同，那么那些缺少的参数终归是需要用的。

好了，鉴于情况 1 跟 情况 2 都有了解决方案，python 自然就不需要函数重载了。

## 6.新式类和旧式类
Python3里的类全部都是新式类.这里有一个MRO问题可以了解下(新式类是广度优先,旧式类是深度优先)

> 一个旧式类的深度优先的例子

```python
class A():
    def foo1(self):
        print "A"
class B(A):
    def foo2(self):
        pass
class C(A):
    def foo1(self):
        print "C"
class D(B, C):
    pass

d = D()
d.foo1()

# A
```

**按照经典类的查找顺序`从左到右深度优先`的规则，在访问`d.foo1()`的时候,D这个类是没有的..那么往上查找,先找到B,里面没有,深度优先,访问A,找到了foo1(),所以这时候调用的是A的foo1()，从而导致C重写的foo1()被绕过**

## 7. __new__和__init__
__new__是一个静态方法,而__init__是一个实例方法.

__metaclass__是创建类时起作用.所以我们可以分别使用__metaclass__,__new__和__init__来分别在类创建,实例创建和实例初始化的时候做一些小手脚.

## 8.单例模式

> ​	单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。
>
> `__new__()`在`__init__()`之前被调用，用于生成实例对象。利用这个方法和类的属性的特点可以实现设计模式的单例模式。单例模式是指创建唯一对象，单例模式设计的类只能实例


### 1 使用`__new__`方法

```python
class Singleton(object):
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            orig = super(Singleton, cls)
            cls._instance = orig.__new__(cls, *args, **kw)
        return cls._instance

class MyClass(Singleton):
    a = 1
```

### 2 共享属性

创建实例时把所有实例的`__dict__`指向同一个字典,这样它们具有相同的属性和方法.

```python

class Borg(object):
    _state = {}
    def __new__(cls, *args, **kw):
        ob = super(Borg, cls).__new__(cls, *args, **kw)
        ob.__dict__ = cls._state
        return ob

class MyClass2(Borg):
    a = 1
```

### 3 装饰器版本

```python
def singleton(cls):
    instances = {}
    def getinstance(*args, **kw):
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return getinstance

@singleton
class MyClass:
  ...
```

### 4 import方法

作为python的模块是天然的单例模式

```python
# mysingleton.py
class My_Singleton(object):
    def foo(self):
        pass

my_singleton = My_Singleton()

# to use
from mysingleton import my_singleton

my_singleton.foo()

```

## 9.GIL线程全局锁
线程全局锁(Global Interpreter Lock),即Python为了保证线程安全而采取的独立线程运行的限制,说白了就是一个核只能在同一时间运行一个线程.
**对于io密集型任务，python的多线程起到作用，但对于cpu密集型任务，python的多线程几乎占不到任何优势，还有可能因为争夺资源而变慢。**
解决办法就是多进程和下面的协程(协程也只是单CPU,但是能减小切换代价提升性能).

## 10.协程
协程，又称微线程，纤程。英文名Coroutine。

协程的概念很早就提出来了，但直到最近几年才在某些语言（如Lua）中得到广泛应用。

子程序，或者称为函数，在所有语言中都是层级调用，比如A调用B，B在执行过程中又调用了C，C执行完毕返回，B执行完毕返回，最后是A执行完毕。

所以子程序调用是通过栈实现的，一个线程就是执行一个子程序。

子程序调用总是一个入口，一次返回，调用顺序是明确的。而协程的调用和子程序不同。

协程看上去也是子程序，但执行过程中，在子程序内部可中断，然后转而执行别的子程序，在适当的时候再返回来接着执行。

注意，在一个子程序中中断，去执行其他子程序，不是函数调用，有点类似CPU的中断。比如子程序A、B：
``` python
def A():
    print('1')
    print('2')
    print('3')

def B():
    print('x')
    print('y')
    print('z')
# 假设由协程执行，在执行A的过程中，可以随时中断，去执行B，B也可能在执行过程中中断再去执行A，结果可能是：
1
2
x
y
3
z
```
但是在A中是没有调用B的，所以协程的调用比函数调用理解起来要难一些。

看起来A、B的执行有点像多线程，但协程的特点在于是一个线程执行，那和多线程比，协程有何优势？

最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。

第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。

因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

Python对协程的支持是通过generator实现的。

在generator中，我们不但可以通过for循环来迭代，还可以不断调用next()函数获取由yield语句返回的下一个值。

**但是Python的yield不但可以返回一个值，它还可以接收调用者发出的参数。**

来看例子：

传统的生产者-消费者模型是一个线程写消息，一个线程取消息，通过锁机制控制队列和等待，但一不小心就可能死锁。

如果改用协程，生产者生产消息后，直接通过yield跳转到消费者开始执行，待消费者执行完毕后，切换回生产者继续生产，效率极高：

``` python
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)


# 执行结果：
[PRODUCER] Producing 1...
[CONSUMER] Consuming 1...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 2...
[CONSUMER] Consuming 2...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 3...
[CONSUMER] Consuming 3...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 4...
[CONSUMER] Consuming 4...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 5...
[CONSUMER] Consuming 5...
[PRODUCER] Consumer return: 200 OK
```
注意到consumer函数是一个generator，把一个consumer传入produce后：

首先调用c.send(None)启动生成器；

然后，一旦生产了东西，通过c.send(n)切换到consumer执行；

consumer通过yield拿到消息，处理，又通过yield把结果传回；

produce拿到consumer处理的结果，继续生产下一条消息；

produce决定不生产了，通过c.close()关闭consumer，整个过程结束。

整个流程无锁，由一个线程执行，produce和consumer协作完成任务，所以称为“协程”，而非线程的抢占式多任务。

最后套用Donald Knuth的一句话总结协程的特点：

“子程序就是协程的一种特例。”

## 11.函数式编程

filter 函数的功能相当于过滤器。调用一个布尔函数`bool_func`来迭代遍历每个seq中的元素；返回一个使`bool_seq`返回值为true的元素的序列。

```python
>>>a = [1,2,3,4,5,6,7]
>>>b = filter(lambda x: x > 5, a)
>>>print b
>>>[6,7]
```

map函数是对一个序列的每个项依次执行函数，下面是对一个序列每个项都乘以2：

```python
>>> a = map(lambda x:x*2,[1,2,3])
>>> list(a)
[2, 4, 6]
```

reduce函数是对一个序列的每个项迭代调用函数，下面是求3的阶乘：

```python
>>> reduce(lambda x,y:x*y,range(1,4))
6
```

## 12.浅拷贝和深拷贝
浅拷贝是在另一块地址中创建一个新的变量或容器，但是容器内的元素的地址均是源对象的元素的地址的拷贝。
也就是说新的容器中指向了旧的元素（ 新瓶装旧酒 ）。

深拷贝是在另一块地址中创建一个新的变量或容器，同时容器内的元素的地址也是新开辟的，仅仅是值相同而已，是完全的副本。也就是说（ 新瓶装新酒 ）。

```python
import copy
a = [1, 2, 3, 4, ['a', 'b']]  #原始对象

b = a  #赋值，传对象的引用
c = copy.copy(a)  #对象拷贝，浅拷贝
d = copy.deepcopy(a)  #对象拷贝，深拷贝

a.append(5)  #修改对象a
a[4].append('c')  #修改对象a中的['a', 'b']数组对象

print 'a = ', a
print 'b = ', b
print 'c = ', c
print 'd = ', d

输出结果：
a =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
b =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
c =  [1, 2, 3, 4, ['a', 'b', 'c']]    # 和a无关，复制了a子元素的地址，没有复制a的地址
d =  [1, 2, 3, 4, ['a', 'b']]
```
## 13. Python垃圾回收机制
GC作为现代编程语言的自动内存管理机制，专注于两件事：
1. 找到内存中无用的垃圾资源

2. 清除这些垃圾并把内存让出来给其他对象使用。
GC彻底把程序员从资源管理的重担中解放出来，让他们有更多的时间放在业务逻辑上。
但这并不意味着码农就可以不去了解GC，毕竟多了解GC知识还是有利于我们写出更健壮的代码。

### 1. 引用计数
Python语言默认采用的垃圾收集机制是『引用计数法 Reference Counting』，该算法最早George E. Collins在1960的时候首次提出，
50年后的今天，该算法依然被很多编程语言使用，『引用计数法』的原理是：每个对象维护一个ob_ref字段，用来记录该对象当前被引用的次数，
每当新的引用指向该对象时，它的引用计数ob_ref加1，每当该对象的引用失效时计数ob_ref减1，一旦对象的引用计数为0，该对象立即被回收，
对象占用的内存空间将被释放。它的缺点是需要额外的空间维护引用计数，这个问题是其次的，不过最主要的问题是它不能解决对象的“循环引用”，
因此，也有很多语言比如Java并没有采用该算法做来垃圾的收集机制。

什么是循环引用？A和B相互引用而再没有外部引用A与B中的任何一个，它们的引用计数虽然都为1，但显然应该被回收，例子：
``` python
a = { } #对象A的引用计数为 1
b = { } #对象B的引用计数为 1
a['b'] = b  #B的引用计数增1
b['a'] = a  #A的引用计数增1
del a #A的引用减 1，最后A对象的引用为 1
del b #B的引用减 1, 最后B对象的引用为 1
```

在这个例子中程序执行完del语句后，A、B对象已经没有任何引用指向这两个对象，
但是这两个对象各包含一个对方对象的引用，虽然最后两个对象都无法通过其它变量来引用这两个对象了，
这对GC来说就是两个非活动对象或者说是垃圾对象，但是他们的引用计数并没有减少到零。
因此如果是使用引用计数法来管理这两对象的话，他们并不会被回收，它会一直驻留在内存中，
就会造成了内存泄漏（内存空间在使用完毕后未释放）。
为了解决对象的循环引用问题，Python引入了标记-清除和分代回收两种GC机制。

### 2. 标记清除
『标记清除（Mark—Sweep）』算法是一种基于追踪回收（tracing GC）技术实现的垃圾回收算法。
它分为两个阶段：第一阶段是标记阶段，GC会把所有的『活动对象』打上标记，第二阶段是把那些没有标记的对象『非活动对象』进行回收。
那么GC又是如何判断哪些是活动对象哪些是非活动对象的呢？

对象之间通过引用（指针）连在一起，构成一个有向图，对象构成这个有向图的节点，而引用关系构成这个有向图的边。
从根对象（root object）出发，沿着有向边遍历对象，可达的（reachable）对象标记为活动对象，不可达的对象就是要被清除的非活动对象。
根对象就是全局变量、调用栈、寄存器。

![](https://foofish.net/images/mark-sweep.svg)

在上图中，我们把小黑圈视为全局变量，也就是把它作为root object，从小黑圈出发，对象1可直达，那么它将被标记，对象2、3可间接到达也会被标记，而4和5不可达，那么1、2、3就是活动对象，4和5是非活动对象会被GC回收。

标记清除算法作为Python的辅助垃圾收集技术主要处理的是一些容器对象，比如list、dict、tuple，instance等，
因为对于字符串、数值对象是不可能造成循环引用问题。Python使用一个双向链表将这些容器对象组织起来。不过，这种简单粗暴的标记清除算法也有明显的缺点：
清除非活动的对象前它必须顺序扫描整个堆内存，哪怕只剩下小部分活动对象也要扫描所有对象。
### 3.分代回收

分代回收是一种以空间换时间的操作方式，Python将内存根据对象的存活时间划分为不同的集合，每个集合称为一个代，Python将内存分为了3“代”，
分别为年轻代（第0代）、中年代（第1代）、老年代（第2代），他们对应的是3个链表，它们的垃圾收集频率与对象的存活时间的增大而减小。
新创建的对象都会分配在年轻代，年轻代链表的总数达到上限时，Python垃圾收集机制就会被触发，把那些可以被回收的对象回收掉，
而那些不会回收的对象就会被移到中年代去，依此类推，老年代中的对象是存活时间最久的对象，甚至是存活于整个系统的生命周期内。
同时，分代回收是建立在标记清除技术基础之上。分代回收同样作为Python的辅助垃圾收集技术处理那些容器对象

## 14. List实现

### 1. List对象的C结构
``` c
typedef struct {
    PyObject_VAR_HEAD
    PyObject **ob_item;
    Py_ssize_t allocated;
} PyListObject;
```

### 2.List的初始化
list申请内存空间的大小（后文用allocated代替）的大小和list实际存储元素所占空间的大小(ob_size)之间的关系，ob_size的大小和len(L)是一样的，
而allocated的大小是在内存中已经申请空间大小。通常你会看到allocated的值要比ob_size的值要大。
这是为了避免每次有新元素加入list时都要调用realloc进行内存分配。

### 3.Append
我们在list中追加一个整数:L.append(1)。发生了什么？
来让我们看下list_resize(),list_resize()会申请多余的空间以避免调用多次list_resize()函数.
开辟了四个内存空间来存放list中的元素，存放的第一个元素是1。我们继续加入一个元素：L.append(2)。调用list_resize,同时n+1=2。
但是因为allocated（译者注：已经申请的空间大小）是4。所以没有必要去申请新的内存空间。

### 4.Insert
现在我们在列表的第一个位置插入一个整数5:L.insert(1, 5),看看内部发生了什么。
![](https://raw.github.com/acmerfight/insight_python/master/images/list_insert.png)

虚线框表示已经申请但是没有使用的内存。申请了8个内存空间但是list实际用来存储元素只使用了其中5个内存空间
insert的时间复杂度是O(n)
### 5.Pop
当你弹出list的最后一个元素：L.pop()。调用listpop()，list_resize在函数listpop()内部被调用，
如果这时ob_size（译者注：弹出元素后）小于allocated（译者注：已经申请的内存空间）的一半。这时申请的内存空间将会缩小。

你可以发现4号内存空间指向还指向那个数值（译者注：弹出去的那个数值），但是很重要的是ob_size现在却成了4.
让我们再弹出一个元素。在list_resize内部，size – 1 = 4 – 1 = 3 比allocated（已经申请的空间）的一半还要小。所以list的申请空间缩小到
6个，list的实际使用空间现在是3个.
![](https://raw.github.com/acmerfight/insight_python/master/images/list_pop_2.png)


### 6.Remove
切开list和删除元素，调用了list_ass_slice().Remove的时间复杂度为O(n)
### 7.总结
我们能看到 Python 设计者的苦心。在需要的时候扩容,但又不允许过度的浪费,适当的内存回收是非常必要的。
这个确定调整后的空间大小算法很有意思。
调整后大小 (new_allocated) = 新元素数量 (newsize) + 预留空间 (new_allocated)
调整后的空间肯定能存储 newsize 个元素。要关注的是预留空间的增长状况。
将预留算法改成 Python 版就更清楚了:

(newsize // 8) + (newsize < 9 and 3 or 6)。
当 newsize >= allocated,自然按照这个新的长度 "扩容" 内存。
而如果 newsize < allocated,且利用率低于一半呢?

allocated    newsize       new_size + new_allocated
10           4             4 + 3
20           9             9 + 7
很显然,这个新长度小于原来的已分配空间长度,自然会导致 realloc 收缩内存。
## 15.
is是对比地址,==是对比值

read 读取整个文件
readline 读取下一行,使用生成器方法
readlines 读取整个文件到一个迭代器以供我们遍历
## 16. super
http://funhacks.net/explore-python/Class/super.html

## 17. [more](https://www.aliyun.com/jiaocheng/452209.html)