# 数据结构

## 1.列表
### 1.1 方法
list.append(x)
添加一个元素到列表的末尾。相当于 a[len(a):] = [x].

list.extend(L)
将给定列表L中的所有元素附加到原列表a的末尾。相当于 a[len(a):] = L.

list.insert(i, x)
在给定位置插入一个元素。第一个参数为被插入元素的位置索引，因此 a.insert(0, x) 在列表头插入值， a.insert(len(a), x)相当于 a.append(x).

list.remove(x)
删除列表中第一个值为 x 的元素。如果没有这样的项目则会有一个错误。

list.pop([i])
删除列表中给定位置的元素并返回它。如果没有给定位置，a.pop()将会删除并返回列表中的最后一个元素。（i 两边的方括号表示这个参数是可选的，而不是要你输入方括号。你会在 Python 参考库中经常看到这种表示法)。

list.clear()
删除列表中所有的元素。相当于 del a[:].

list.index(x)
返回列表中第一个值为 x 的元素的索引。如果没有这样的元素将会报错。

list.count(x)
返回列表中 x 出现的次数。

list.sort(key=None, reverse=False)
排序列表中的项 (参数可被自定义, 参看 sorted() ).

list.reverse()
列表中的元素按位置反转。

list.copy()
返回列表的一个浅拷贝。相当于 a[:].

### 1.2 列表作为栈使用
列表方法使得将列表用作堆栈非常容易，其中添加的最后一个元素是可提取的第一个元素（“last-in，first-out”）。使用 append()添加项到栈顶。使用无参的 pop() 从栈顶检出项。

### 1.3 列表作为队列使用
列表也有可能被用来作队列——先添加的元素被最先取出 (“先进先出”)；然而列表用作这个目的相当**低效**。
因为在列表的末尾添加和弹出元素非常快，但是在列表的开头插入或弹出元素却很慢 (因为所有的其他元素必须向右或向左移一位)。

若要实现一个队列， **collections.deque** 被设计用于快速地从两端操作。例如：
``` python
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
```

### 1.4 列表推导式
列表推导式提供一个生成列表的简洁方法。应用程序通常会从一个序列的每个元素的操作结果生成新的列表，或者生成满足特定条件的元素的子序列。
列表推导式由一对方括号组成，方括号包含一个表达式，其后跟随一个for子句，然后是零个或多个for或if子句。
结果将是一个新的列表，其值来自将表达式在其后的for和if子句的上下文中求值得到的结果。

``` python
>>> squares = []
>>> for x in range(10):
...     squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

squares = list(map(lambda x: x**2, range(10)))
squares = [x**2 for x in range(10)]

>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

在实际中，与复杂的控制流比起来，你应该更喜欢内置的函数。zip()函数对这个使用场景做得非常好。
zip()函数返回一个由元组构成的迭代器，其中第i个元组包含来自每一组参数序列或可迭代量的第i元素。
``` python
>>> list(zip(*matrix))
[(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]
```