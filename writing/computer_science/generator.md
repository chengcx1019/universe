# python中的延迟

> python版本3.5.2

python提供了工具在需要时才产生结果，即一次产生一个结果项，而不是在内存中一次产生全部结果列表。

延迟的核心是迭代器。

后面的内容包括以下几个部分，可迭代对象和迭代器，生成器，map和zip，filter和reduce，序列range，enumerate，文件迭代器，字典视图迭代器。

## 可迭代对象和迭代器

一个可迭代对象需要支持`__iter__()`和`__next__()`这两个方法，这两个方法构成迭代协议。

`__iter__()`返回迭代器对象本身,它允许对象（容器containers和迭代器iterators）在for和in表达式中直接使用；

`__next__()`返回容器container中的下一个元素，如果没有更多元素，则会引起`StopIteration`异常。

生成器是实现迭代协议的一种便利方式，如果一个容器对象的`__iter__()`是以生成器实现的，它会自动返回一个生成器对象。

> 在之后的阅读中始终注意，哪些对象本身就是迭代器，哪些对象需要使用iter(self)来返回迭代器。

## 生成器

**一个生成器的迭代器是生成器自身**。

**一个生成器运行到完成，必须产生一个新的生成器以再次开始**。

有两种语言结构尽可能的延迟结果创建，无需一次构建所有对象：生成器函数和生成器表达式。

1. 生成器函数

   举例说明，有如下定义：

   ```python
   def gen():
       for i in range(5):
           x = yield i
           print("x:",x)
   ```

   理解`yield`：

   yield表达式只在定义生成器函数时使用，而且也只能在函数体中使用。使用了yield表达式的函数会被特别的解释为生成器。yield语句挂起该函数并向调用者发送回一个值，同时保留足够的状态使得函数从离开的地方继续。

   当通过调用生成器的某个方法继续时，函数在yield返回后立即继续执行，继续时yield表达式的值取决于调用的方法，如果调用的是`__next__()`即内置函数`next()`,yield表达式的值为`None`,如果是`send()`，则yield表达式的值为传递给send方法的值。

   理解`yield from`：

   将后面的表达式视为子迭代器，子迭代器的所有返回值都会直接返回给当前生成器的调用者；同理，外部调用`send()`和`throw()`也可直接传递到子迭代器(也是生成器的话)内部。下面的两条语句作用是一致的：

   ```python
   yield from iterable
   for item in iterable: yield item
   ```

   当一个生成器函数被调用时，返回一个生成器作为迭代器，在一个list内置调用中包含一个生成器表达式以迫使其生成包含所有结果的列表：

   ```python
   >>>g = gen()
   >>>list(g)
   [0, 1, 2, 3, 4]
   ```

   也可以通过next方法依次获取,直到StopIteration异常，生成器运行完成：

   ```python
   >>>g = gen()
   >>>next(g)
   0
   >>>next(g)
   x: None
   1
   ···
   >>>next(g)
   StopIteration
   ```

   生成器函数协议中增加了一个send方法，send方法的行为可以这么理解:

   1. 将值发送给生成器，即回到生成器函数内部的，yield表达式返回发送的值：

      ```python
      >>>g = gen()
      >>>next(g)
      0
      >>>g.send(100)
      x: 100
      1
      ```

   2. 如果没有调用send，正常调用next，那么在生成器函数内部yield表达式返回None：

      ```Python
      >>>g = gen()
      >>>next(g)
      0
      >>>next(g)
      x: None
      1
      ```

   通过一个实例更好的理解`yield from`和生成器的`send`方法，定义如下两个生成器：

   ```Python
   def accumulate():
       tally = 0
       while 1:
           next = yield
           if next is None:
               return tally
           tally += next

   def gather_tallies(tallies):
       while 1:
           tally = yield from accumulate()
           tallies.append(tally)
   ```
   按如下方式进行调用：
   ```python
   tallies = []
   acc = gather_tallies(tallies)
   next(acc)  # Ensure the accumulator is ready to accept values

   for i in range(4):
       acc.send(i)
   acc.send(None)  # Finish the first tally

   for i in range(5):
       acc.send(i)
   acc.send(None)  # Finish the second tally

   >>>tallies
   [6, 10]
   ```

2. 生成器表达式

   迭代器和列表解析的概念形成生成器表达式，与列表解析不同，生成器表达式包含在园括号(parentheses)而不是方括号(brackets or curly braces)中，如(x ** 2 for x in range(4))。

## map和zip

### map

用法：`map(function, iterable, ...)`，对iterable的每个元素应用function，并将结果以生成器返回；如果参数中有多个可迭代对象，function必须能接收多个参数，当最短的可迭代对象耗尽时，迭代停止。

```python
>>>m = map(abs,[-1, 0, 1])
>>>iter(m) is m
True
```

自定义一个map函数：

```python
def mymap(func,*seqs):
    res = []
    for args in zip(*seqs):
        res.append(func(*args))
    return res
```

```Python
# 或者是更简洁的列表解析式的表达,
def mymap(func,*seqs):
    return [func(*args) for args in zip(*seqs)]
>>>mymap(abs, [-2, -1, 0, 1, 2])
[2, 1, 0, 1, 2]
>>>mymap(pow, [1 ,2 ,3], [2, 3, 4, 5])
[1, 8, 81]
```
```Python
# 上面的实现和实际的map略有差异，更优的方案是通过生成器函数和生成器表达式来返回结果
def mymap(func,*seqs):
    for args in zip(*seqs):
        yield func(*args)
# 生成器表达式
def mymap(func,*seqs):
    return (func(*args) for args in zip(*seqs))
>>>mymap(abs, [-2, -1, 0, 1, 2])
<generator object mymap.<locals>.<genexpr> at 0x102583bf8>
```
### zip

用法：`zip(*iterables)`从每个可迭代对象iterable中取出一个元素组成元组，以迭代器的形式返回这些元组，当最短的那个可迭代对象耗尽时，迭代停止。

```python
>>>z = zip(['a', 'b', 'c'],[1, 2, 3])
>>>iter(z) is z
True
```

`zip`用来迭代多个序列，`zip(a,b)`会生成一个可返回元组`(x,y)`的**迭代器**，其中x来自a，y来自b；一旦其中某个序列遍历完，迭代结束，因此迭代长度和参数中最短序列长度一致；`zip`会创建一个迭代器来作为结果返回，如果需要将所有结果存储在列表中，要使用`list()`方法。



自定义zip前先了解两个内置函数all和any（之后也许会有一个关于内置函数的专题），`all(iterable)`,如果可迭代的对象(数组，字符串，列表等)中的元素都不为空的话返回 `True`；`any(iterable)`，如果可迭代的对象中任何一个元素不为空的话返回 `True` ，如果可迭代的对象都为空则返回 `False` ，自定义一个zip函数：

```Python
def myzip(*seqs):
    seqs = [list(S) for S in seqs]
    res = []
    while all(seqs):
        res.append(tuple(S.pop(0) for S in seqs))
        # 类似木桶原理，当最短的那个可迭代的对象遍历完成后，all(seqs)返回False
    return res
```

```python
# 使用生成器函数表达
def myzip(*seqs):
    seqs = [list(S) for S in seqs]
    while all(seqs):
        yield tuple(S.pop(0) for S in seqs)
```
```python
# 结合map实现zip
def myzip(*args):
    iters = list(map(iter, args))
    while iters:
        res = [next(i) for i in iters]
        yield tuple(res)
    """
    如果把iters = list(map(iter, args))替换为iters = map(iter, args)
    在python3中，myzip会陷入无限循环，map返回一个单次可迭代对象，只要在循环中运行一次列表解析，iters不为空（是'<map object at 0x107c242e8>'），但iters的迭代结果将会永远为空，并且res是[],需要使用list来创建一个支持多次迭代的对象。
    """
```
## filter和reduce

### filter

用法：`filter(function, iterable)`，从可迭代对象iterable中取出function返回`True`的元素构造的迭代器，等价表达：

```python
# if function is not None
(item for item in iterable if function(item))
# if function is None
(item for item in iterable if item)
```

```python
>>>filt = filter(lambda x: x > 0, [-1, 0, 1])
>>>iter(filt) is filt
True
```

### reduce

用法：`functools.reduce(function, iterable[, initializer])`,对可迭代对象iterable累积应用于有两个参数的function，直到将可迭代对象iterable归约为一个值，比如`reduce(lambda x, y: x+y, [1, 2, 3,4, 5])` 等效于`((((1+2)+3)+4)+5)`，`reduce`实现：

```python
def reduce(function, iterable, initializer=None):
    it = iter(iterable)
    if initializer is None:
        value = next(it)
    else:
        value = initializer
    for element in it:
        value = function(value, element)
    return value
```

## 序列range

用法：`range(stop)`和`range(start, stop[, step])`

支持多个活跃迭代器。

```python
>>>r = range(5)
>>>r
range(0, 5)
>>>iter(r)
<range_iterator at 0x107c46fc0>
>>>iter(r) is r
False
```

range是一个可迭代对象，根据需要产生元素

## enumerate

迭代一个序列的同时跟踪正在被处理元素的索引。

用法：`enumerate(iterable, start=0）`,

```python
>>>D = dict(a=1, b=2, c=3)
>>>eD = enumerate(D)
>>>iter(eD) is eD
True
```

`enumerate`的等价表达：

```python
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```

## 文件

文件对象的迭代器是文件本身

```python
>>>f = open('hello.py')
>>>iter(f) is f
True
```

文件读取可以使用`readlines`直接将文件全部加载到内存中，但往往这种方法的效果很差，甚至会出现内存不够的情况，下面这种逐行读取的迭代器版本效果会更好：

```python
for line in open('hello.py'):
    print(line, end='')
```

## 字典视图迭代器

`keys()`,`values()`,`items()`,分别返回字典相应的视图，即返回的是字典实体的动态视图，如果字典变化了,视图相应变化，迭代视图时如果修改了实体会引发`RuntimeError`。

```python
>>>D = dict(a=1, b=2, c=3)
>>>D.keys()
dict_keys(['b', 'a', 'c'])
>>>D.values()
dict_values([2, 1, 3])
>>>D.items()
dict_items([('b', 2), ('a', 1), ('c', 3)])
```

Hello

```Python
>>>D = dict(a=1, b=2, c=3)
>>>iter(D) is D # 字典有自己的迭代器，返回连续的键,可以直接迭代
False
>>>for key in D:print(key, end=' ')
b a c
```

Hello