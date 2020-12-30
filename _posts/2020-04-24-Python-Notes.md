---
title: Python学习笔记
author: Liang Chen
date: 2020-04-24 18:00:00 +0800
categories: [Notes]
tags: [Python]
pin: false
toc: true
comments: false
math: true
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

# 基于[廖雪峰的Python教程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017063826246112)笔记

## 基础

1. 直接运行`.py`文件

   在`hello.py`文件的首行加上一个特殊的注释：`#!/usr/bin/env python3`，然后在命令行执行`chmod a+x hello.py`，最后就可以在命令行直接运行：`./hello.py`；或者，只能在命令行执行`python hello.py`。

2. python是**大小写敏感**的，在编辑器里设置Tab键自动转换为4个空格，不能混用tab和space键。

3. 可以写负数，`-100`，`-0.1`。

4. 字符串用`\`转义，当然也可以`r''`不转义。

5. 布尔值`True`、`False`，运算`and`、`or`、`not`。

6. 空值`None`不是`0`，`0`有意义，`None`是一个特殊的空值。

7. Python是**动态语言**，不是静态语言(赋值需要与变量类型匹配)。

8. 通常用大写的变量名表示常量。

9. 两种除法，`/`结果是浮点数，`//`地板除是只取结果的整数部分，`%`是取余。

10. Python字符串用Unicode编码，在源代码的开头应该写上`# -*- coding: utf-8 -*-`。
    
    | 字符  | ASCII    | Unicode             | UTF-8 |
    | :----: | :---------: | :-------------------: | :---------: |
    | A    | 0100_0001 | 0000_0000 0100_0001 | 0100_0001 |
    | 中 | x | 0100_1110 0010_1101 | 11100100 10111000 10101101 |

11. Python支持多语言

    ```python
    >>> print('包含中文的str')
    包含中文的str
    ```

12. ```python
    >>> ord('A')
    65
    >>> ord('中')
    20013
    >>> chr(66)
    'B'
    >>> chr(25991)
    '文'
    ```

13. Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示。

    ```python
    x = b'ABC
    ```

14. 以Unicode表示的`str`通过`encode()`方法可以编码为指定的`bytes`

    ```python
    >>> 'ABC'.encode('ascii')
    b'ABC'
    >>> '中文'.encode('utf-8')
    b'\xe4\xb8\xad\xe6\x96\x87'
    >>> '中文'.encode('ascii')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
    ```

15. `bytes`解码为`str`

    ```python
    >>> b'ABC'.decode('ascii')
    'ABC'
    >>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
    '中文'
    ```

16. 如果`bytes`中只有一小部分无效的字节，可以传入`errors='ignore'`忽略错误的字节

    ```python
    >>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
    '中'
    ```

17. ```python
    # 字符数
    >>> len('ABC')
    3
    >>> len('中文')
    2
    # 字节数
    >> len(b'ABC')
    3
    >>> len(b'\xe4\xb8\xad\xe6\x96\x87')
    6
    >>> len('中文'.encode('utf-8'))
    6
    ```

18. 格式化

    ```python
    >>> 'Hello, %s' % 'world'
    'Hello, world'
    >>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
    'Hi, Michael, you have $1000000.'
    ```

19. `list`列表，`tuple`元祖

    > `list`有序可变集合，所含元素类型可以不同，`len()`，`list[0]`，`list[-1]`，`list.append()`，`list.insert()`，`list.pop()`，`list.pop(i)`
    >
    > `tuple`有序不可变集合，`tuple t = (1,)`

20. 条件判断`if...elif...elif...else`

21. 循环`for x in X`，`while`，`break`，`continue`，`range()`

22. 字典`dict`，集合`set`

    > 字典`{key:value}`，键-值一一对应，采用哈希(Hash)算法无序集合，key是**不可变对象**，比如字符串和整数，`dict[key]`，`dict.get(key, -1)`，`dict.pop(key)`，用空间换取时间，而`list`是用时间换取空间；
    >
    > 集合`s = set([1, 2, 3])`，`s.add(key)`，`s.remove()`，没有值，只有键。

## 函数

1. 内置函数查找表：[Built-in Functions](https://docs.python.org/3.8/library/functions.html)

   可以把内置函数名赋给一个变量：

   ```python
   >>> a = abs # 变量a指向abs函数
   >>> a(-1) # 所以也可以通过a调用abs函数
   1
   ```

2. 函数执行完毕也没有`return`语句时，自动`return None`

3. 函数可以同时返回多个值，但其实就是一个tuple。

4. 如果你已经把`my_abs()`的函数定义保存为`abstest.py`文件了，那么，可以在该文件的当前目录下启动Python解释器，用`from abstest import my_abs`来导入`my_abs()`函数，注意`abstest`是文件名（不含`.py`扩展名）。

5. 空函数`pass`

6. `isinstance(x, int)`数据类型检查

7. **函数的参数**

   - 位置参数
   - 默认参数：必须指向不变的对象，比如`None`
   - *可变参数*：`def calc(*numbers)`，对应tuple
   - *关键字参数*：`def person(name, age, **kw)`，对应dict
   - *命名关键字参数*：`def person(name, age, *, city, job)`，`def person(name, age, *args, city, job)`
   - 参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数
   - 对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的

8. 递归函数

   - 在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以递归调用次数过多，会导致**栈溢出**。

   - 解决递归调用栈溢出的方法是通过**尾递归**优化，***尾递归就是循环***。尾递归是指，**在函数返回的时候，调用自身本身，并且，return语句不能包含表达式**。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

   - 但是Python解释器没有针对尾递归做优化。

   - practice 1：计算`n!=1*2*3*...*(n-1)*n`

     ```python
     def fact(n):
         if n==1:
             return 1
         else:
             return n*fact(n-1)
     
     ########## 尾递归优化 ############
     
     def fact(n):
         return fact_iter(n, 1)
     
     def fact_iter(n, product):
         if n==1:
             return product
         else:
             return fact_iter(n-1, product*n)
     ```

   - practice 2：[汉诺塔](https://zh.wikipedia.org/wiki/%E6%B1%89%E8%AF%BA%E5%A1%94)

     ```python
     def hanoi(n, a, b, c):
         if n==1:
             print(a, '-->', c)
         else:
             hanoi(n-1, a, c, b)
             hanoi(1, a, b, c)
             hanoi(n-1, b, a, c)
     if __name__ == '__main__':
         hanoi(4, 'a', 'b', 'c')
     ```

## 高级特性

1. 切片(Slice)

   对列表、元祖、字符串切片，`list1[0:3]`，`list1[:3]`，`list1[-2:]`，`list1[-2:-1]`，`list1[-1]`，`list1[:]`，`list1[::2]`，`list1[::-1]`切片操作就是对字符串的截取函数。

2. 迭代(Iteration)

   `for ... in ...`

   C语言等很多语言是通过下标迭代的，但是Python对可迭代对象都可迭代，无论有没有索引下标，比如列表，元祖，字典，字符串，`dict.keys()`，`dict.values()`，`dict.items()`

   **如何判断一个对象是可迭代对象呢？**

   ```python
   >>> from collections import Iterable
   >>> isinstance('abc', Iterable) # str是否可迭代
   True
   >>> isinstance([1,2,3], Iterable) # list是否可迭代
   True
   >>> isinstance(123, Iterable) # 整数是否可迭代
   False
   ```
   
   下标循环
   
   ```python
   >>> for i, value in enumerate(['A', 'B', 'C']):
   ...     print(i, value)
   ...
   0 A
   1 B
   2 C
   ```
   
   同时引用两个变量也很常见
   
   ```python
   >>> for x, y in [(1, 1), (2, 4), (3, 9)]:
   ...     print(x, y)
   ...
   1 1
   2 4
   3 9
   ```
   
3. **列表生成式**

   快速生成列表，代码非常简洁，无需循环

   `[表达式 for ... in ... if...]`，`if`起筛选条件

   `[表达式 if ... else ... for ... in ...]`

   ```python
   >>> [x * x for x in range(1, 11)]
   [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
   ```

   ```python
   >>> [x * x for x in range(1, 11) if x % 2 == 0]
   [4, 16, 36, 64, 100]
   ```

   ```python
   >>> [m + n for m in 'ABC' for n in 'XYZ']
   ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
   ```

   ```python
   >>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
   >>> [k + '=' + v for k, v in d.items()]
   ['y=B', 'x=A', 'z=C']
   ```

   ```python
   #!/usr/bin/env python3
   # -*- coding: UTF-8 -*-
   
   L1 = ['Hello', 'World', 18, 'Apple', None]
   L2 = [s.lower() for s in L1 if isinstance(s, str)]
   print(L2)
   ```

4. **生成器(Generator)**

   把一个列表生成式的`[]`改成`()`

   ```python
   >>> L = [x * x for x in range(10)]
   >>> L
   [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
   >>> g = (x * x for x in range(10))
   >>> g
   <generator object <genexpr> at 0x1022ef630>
   ```

   **generator保存的是算法**，每次调用`next(g)`，就计算出`g`的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出`StopIteration`的错误。

   不用`next()`，用`for ... in`循环输出值，generator是可迭代对象

   ```python
   >>> g = (x * x for x in range(10))
   >>> for n in g:
   ...     print(n)
   ```

   ***函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。***

   practice 1：斐波拉契数列（Fibonacci）`1, 1, 2, 3, 5, 8, 13, 21, 34, ...`

   - 普通函数

     ```python
     #!/usr/bin/env python3
     # -*- coding: utf-8 -*-
     
     def fib(N):
         n, a, b = 0, 0, 1
         while n < N:
             print(b)
             a, b = b, a + b # 相当于tuple t=(b,a+b) a=t[0] b=t[1]
             n = n + 1
     ```

   - 生成器

     ```python
     #!/usr/bin/env python3
     # -*- coding: utf-8 -*-
     
     def fib_g(N):
         n, a, b = 0, 0, 1
         while n < N:
             yield b
             a, b = b, a + b
             n = n + 1
     
     if __name__ == "__main__":
         for i in fib_g(10):
             print(i)
     ```

   practice 2：[杨辉三角](https://zh.wikipedia.org/wiki/杨辉三角形)

   > ```python
   > #!/usr/bin/env python3
   > # -*- coding: utf-8 -*-
   > 
   > def yhtri():
   >     L1 = [1]
   >     L2 = []
   >     while True:
   >         yield L1
   >         L1 = [1] + L2 + [1]
   >         L2 = []
   >         for i in range(len(L1)-1):
   >             L2.append(L1[i]+L1[i+1])
   >             
   > if __name__ == "__main__":
   >     results = []
   >     n = 0
   >     for l in yhtri():
   >         results.append(l)
   >         n = n + 1
   >         if n == 10:
   >             break
   >     for i in results:
   >         print(i)
   > ```
   
5. 迭代器(Iterator)

   可以被`next()`函数调用并不断返回下一个值的对象称为**迭代器**

   ```python
   >>> from collections.abc import Iterator
   >>> isinstance((x for x in range(10)), Iterator)
   True
   >>> isinstance([], Iterator)
   False
   >>> isinstance({}, Iterator)
   False
   >>> isinstance('abc', Iterator)
   False
   ```

   ```python
   >>> isinstance(iter([]), Iterator)
   True
   >>> isinstance(iter('abc'), Iterator)
   True
   ```

   > 你可能会问，为什么`list`、`dict`、`str`等数据类型不是`Iterator`？
   >
   > 这是因为Python的`Iterator`对象表示的是一个**数据流**，Iterator对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却**不能提前知道序列的长度**，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的**计算是惰性**的，只有在需要返回下一个数据时它才会计算。
   >
   > `Iterator`甚至可以表示一个无限大的数据流，例如全体自然数。而使用`list`是永远不可能存储全体自然数的。

   ***Python的`for`循环本质上就是通过不断调用`next()`函数实现的***

   ```python
   for x in [1, 2, 3, 4, 5]:
       pass
   
   ########### <=> ##############
   
   # 首先获得Iterator对象:
   it = iter([1, 2, 3, 4, 5])
   # 循环:
   while True:
       try:
           # 获得下一个值:
           x = next(it)
       except StopIteration:
           # 遇到StopIteration就退出循环
           break
   ```

## 函数式编程

### 高阶函数

1. 函数名就是变量，函数名指向这个函数。

2. **把函数作为参数传入，这样的函数称为高阶函数**，函数式编程就是指这种高度抽象的编程范式。

3. `map()`，`reduce()`高阶函数：

   - `map(fun， Iterable)`：第一个参数是函数，第二个参数是可迭代对象，过程是函数作用在可迭代对象上，结果是返回一个迭代器（惰性序列，每`next()`一次，返回一个值）；

   - `reduce(fun, 序列)`：同样是函数作用在序列上，**要求函数必须包括2个参数**，过程是把函数作用在序列上的结果与序列的下一个元素作为函数的2个参数累积作用；

   - practice 1：实现`int()`函数功能

     ```python
     from functools import reduce
     
     DATA = {'0':0, '1':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9}
     
     def str2int(s):
         def fn(x, y):
             return x * 10 + y
         def char2nums(c):
             return DATA[c]
         return reduce(fn, map(char2nums, s))
     ```

   - practice 2：利用`map()`函数，把用户输入的不规范的英文名，变为首字母大写，其他小写的规范名

     ```python
     #!/usr/bin/env python3
     # -*- coding: utf-8 -*-
     
     def normalize(name):
         def low(name):
             return name.lower()
         def up(name):
             return name.capitalize()
         return map(up, map(low, name))
     
     if __name__ == "__main__":
         name = ['adam', 'LISA', 'barT']
         Name = normalize(name)
         print(list(Name))
     ```

   - `sum()`函数可以接受一个list并求和，编写一个`prod()`函数，可以接受一个list并利用`reduce()`求积

     ```python
     #!usr/bin/env python3
     # -*- coding: utf-8 -*-
     
     from functools import reduce
     
     def prod(lis):
         return reduce(lambda x, y: x * y, lis)
     
     if __name__ == "__main__":
         print('3 * 5 * 7 * 9 =', prod([3, 5, 7, 9]))
         if prod([3, 5, 7, 9]) == 945:
             print('测试成功!')
         else:
             print('测试失败!')
     ```

   - practice 3：利用`map`和`reduce`编写一个`str2float`函数，把字符串转换成浮点数

     ```python
     #!/usr/bin/env python3
     # -*- coding: utf-8 -*-
     
     from functools import reduce
     
     DIGITS = {'0':0, '1':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9}
     
     def str2float(s):
         def char2nums(c):
             return DIGITS[c]  
         def fn(x, y):
             return x * 10 + y
         S = s.split('.')
         return reduce(fn, map(char2nums, S[0]) + reduce(fn, map(char2nums, S[1])) / (10**(len(S[1]))) 
                       
     if __name__ == "__main__":
     	print('str2float(\'123.456789\') =', str2float('123.456789'))
     ```

4. `filter()`筛选函数：

   - 过滤序列，接收一个函数，一个序列，返回的是迭代器，惰性序列，`next()`调用，可以用`list()`转换

   - practice 1：筛选素数，[埃氏筛选法](https://zh.wikipedia.org/wiki/埃拉托斯特尼筛法)

     ```python
     #!/usr/bin/env python3
     # -*- coding: utf-8 -*-
     
     def main():
         for n in primes():
             if n < 1000:
                 print(n)
             else:
                 break
     
     def odd_iter():
         n = 1 
         while True:
             n = n + 2
             yield n
     
     def not_divisible(n):
         return lambda x: x % n > 0
     
     def primes():
         yield 2
         it = odd_iter()
         while True:
             n = next(it)
             yield n
             it = filter(not_divisible(n), it)
     
     if __name__ == "__main__":
         main()
     ```

   - practice 2：请利用`filter()`筛选出回数

     ```python
     def huishu(n):
         if str(n) == str(n)[::-1]:
             return n
     
     print(list(filter(huishu, range(1000))))
     ```

5. `sorted()`也是一个高阶函数

   - `sorted(L, key, reverse=True)`
   - ***`key`函数作用于`L`的每个元素，`key`的参数是`L[0], L[1]...`，不是`L`***

   - 反向排序：添加参数`reverse=True`

   - practice 1：对元祖tuple分别按照姓名和成绩排序

     ```python
     # -*- coding: utf-8 -*-
     
     L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
     
     def by_name(t):
         return t[0]
     
     def by_score(t):
         return t[1]
     
     L1 = sorted(L, key=by_name)
     L2 = sorted(L, key=by_score, reverse=True)
     print(L1)
     print(L2)
     ```

     也可以用`lambda`代替函数定义：

     ```python
     # -*- coding: utf-8 -*-
     
     L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
     
     L1 = sorted(L, key=lambda t: t[0])
     L2 = sorted(L, key=lambda t: t[1], reverse=True)
     
     print(L1)
     print(L2)
     ```


### 返回函数

1. 函数不仅可以接受函数作为参数，也可以把函数作为返回值返回

2.  返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量

3. 如果一定要引用循环变量，那就再创建一个函数

4. practice 1：利用闭包返回一个计数器函数，每次调用它返回递增整数

   ```python
   # 外函数临时变量为可变类型，内函数与此变量绑定，由于是可变类型，会在原对象上更改而不会创建新对象
   
   def createCounter():
       c=[0]
       def counter():
           c[0]+=1
           return c[0]
       return counter
   
   if __name__ == "__main__":
   	counterA = createCounter()
   	print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5
      
   ```

### 匿名函数

`lambda`

### 装饰器（decorator）

> 不太懂？？？？？？？？？以后用到再回来看看。

1. 接受一个函数作为参数，并返回一个函数

2. 在面向对象（OOP）的设计模式中，decorator被称为装饰模式

3. 学习参考代码：[decorator](https://github.com/michaelliao/learn-python3/blob/master/samples/functional/decorator.py)

4. practice：请设计一个decorator，它可作用于任何函数上，并打印该函数的执行时间

   ```python
   # 待完成
   ```

### 偏函数

1. `functools.partial`

2. 把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，使得调用这个新函数更简单

3. ```python
   >>> import functools
   >>> int2 = functools.partial(int, base=2) # base二进制转换
   >>> int2('1000000')
   64
   >>> int2('1010101')
   85
   ```

## 模块

1. 一个`.py`就是一个模块（module）

2. 包（package）：

   ```ascii
   mycompany
   ├─ __init__.py # 可以是空文件，必须存在，否则会被视为普通目录
   ├─ abc.py
   └─ xyz.py
   ```

3. 模块名不要和系统模块名冲突，最好先查看系统是否已存在该模块

4. 检查方法是在Python交互环境执行`import abc`，若成功则说明系统存在此模块

5. ```python
   #!/usr/bin/env python3
   # -*- coding: utf-8 -*-
   
   ' a test module ' # 任何模块代码的第一个字符串都被视为模块的文档注释，__doc__
   
   __author__ = 'Michael Liao'
   
   import sys
   
   def test():
       pass
   
   if __name__=='__main__':
       test()
   ```

   > 当我们在命令行运行模块文件时，Python解释器把一个特殊变量`__name__`置为`__main__`
   >
   > 而如果在其他地方导入该模块时，`if`判断将失败
   >
   > 这种`if`测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

6. 非公开的函数或变量（private）

   - `__xx__`：特殊魔法
   - `_x 或 __x`：私有，Python并没有一种方法可以完全限制访问private函数或变量，可以说是在遵守约定俗成习惯下的私有，可以用特殊方法在外部调用
   - 外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public

7. 安装第三方模块：https://pypi.org/

   - 包管理工具`pip`

   - [anaconda](https://www.anaconda.com/)

   - 模块搜索路径：

     默认情况下，Python解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在`sys`模块的`path`变量中：

     ```python
     >>> import sys
     >>> sys.path
     ```

   - 添加自己的搜索目录：

     1. 在运行时修改，运行结束后失效

        ```python
        >>> import sys
        >>> sys.path.append('/Users/michael/my_py_scripts')
        ```

     2. 设置环境变量`PYTHONPATH`

## OOP
...

