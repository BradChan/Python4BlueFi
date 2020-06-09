====================
yield和generator
====================

yield是Python编程语言的一个高效的指令性方法。

在Python程序中，使用yield的函数被称作生成器(generator)，调用generator时，脚本程序的执行过程与调用普通的函数是完全不同的。

本节课使用几个示例来说明yield的语法和用法。yield几乎是大多数RTOS的线程间切换常用的方法，我们首先了解RTOS的yield，然后在回来
看Python的yield语法。

---------------------------------



RTOS中的yield
-------------------------

多任务的RTOS(Read Time OS)支持多任务/线程编程模式。假设我们定义2个任务：task_A和task_B。并假设，在task_A任务中，检查是否有按钮A被
按下，如果按钮A被按下，我们在屏幕上显示“A”；task_B任务也是如此，只是检查按钮B是否被按下，如果按下在屏幕上显示“B”。

这样的两个任务，有RTOS编程经验的人会使用yield。在task_A任务中，当未检测到A按钮被按下，则执行yield，OS将立即暂停task_A任务执行，切换
到另一个任务执行。

RTOS多任务切换可以使用yield进行人为干预，提前调度任务切换，提高任务响应性能。

通俗地说，RTOS的yield是当前任务提前放弃计划的任务调度时间，暂停本任务中yield之后的程序执行，当本任务再次被切换回来时，首先执行yield之后
的程序语句。


Python中的yield
-------------------------

Python的yield与RTOS的yield很相似。

Python的函数定义中，如果使用yield，这种函数被称作generator。generator都具有next()属性，执行yield语句时，函数会被中断一次，并返回一个
迭代值，下一次再执行该函数时，根据next()属性确定yield的下一个脚本语句，从该语句继续执行函数中的程序语句。从程序执行的流程看，调用generator
时遇到yield就被中断，并返回一个迭代值，再次执行就从yield后的语句开始执行。

Python的generator被yield中断一次并返回一个迭代值，这样的机制带来很多益处，尤其对于需要那些返回列表值且列表非常长的函数，消耗的内存与返回值
列表的长度有关，当系统资源较少时，调用这样的函数会出现内存不足的异常。我们使用yield中断函数执行并返回一个迭代值，下次继续执行yield后的程序，
这样的机制将列表中的值逐个返回，几乎不消耗内存。

下面来对比几个示例，我们可以更好地理解Python的yield。


返回斐波那契数列的函数
-------------------------

斐波那契数列是一个典型的递推器：

   x[n] = x[n-1] + x[n-2],  n>1, x[0]=1, and x[1]=1

除了前两项，斐波那契数列的第n项等于前两项之和。我们定义一个Python函数Fibonacci生成存储有斐波那契数列前n项的列表，并返回这个列表。代码如下：

.. code-block::  python
  :linenos:

      def Fibonacci(iterm): 
         n, a, b = 0, 0, 1 
         list_fib = [] 
         while n < iterm: 
            list_fib.append(b) 
            a, b = b, a + b 
            n = n + 1 
         return list_fib

      lf = Fibonacci(20)
      print(lf)

将该程序代码保存到BlueFi的/CIRCUITPY/code.py文件，你将会在BlueFi的LCD屏幕和串口控制台看到以下输出

.. code-block::  

      code.py output:
      [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765]

这是斐波那契数列前20项的列表。这个程序看起来没有任何毛病，但是我们可以预测，当调用函数Fibonacci的输入参数很大时，
该函数将返回一个很大的列表，占用内存量随着该参数增大而增加。在有限内存资源的计算机系统中调用这个函数，由于函数的
iterm变化而带来不可预制的内存错误。

有经验的Python程序员会建议使用可迭代对象来迭代输出该列表。Python的range()和xrange()都是可迭代器。在Python2.x版本中，这两个迭代器是
不同的，range(n)将生成一个n项整数列表，随着n的增加内存消耗很大；xrange(n)仅仅是一个迭代器，执行一次仅给出一个迭代值，几乎不消耗内存。
在Python3开始，range(n)和xrange(n)完全一样。

BLueFi完全兼容Python3，支持range(n)迭代器，去掉xrange()函数。用USB数据线将BlueFi和电脑连接后，打开MU编辑器，
点击“串口”按钮，按下“Ctrl+c”键并按确认，进入REPL模式，你可以输入验证range()迭代器：

.. code-block::  

      >>> l=range(10)
      >>> l
      range(0, 10)
      >>> list(l)
      [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
      >>> 

当你将迭代器变量l打印到控制台时，他只是显示“range(0, 10)”，而不是“[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]”，只有使用list(l)函数将
迭代器变量l明确地转换为列表时我们才看到完整列表。


改进的斐波那契数列生成器
-------------------------

模仿Python的range()函数，我们定义一个叫Fibonacci的迭代对象生成器，并使用“for _ in _ ”模版逐一获取斐波那契数列项，代码如下：

.. code-block::  python
  :linenos:

      def Fibonacci(iterm): 
         n, a, b = 0, 0, 1 
         while n < iterm: 
            yield b 
            a, b = b, a + b 
            n = n + 1 

      for i in Fibonacci(10):
         print(i)

将该程序代码保存到BlueFi的/CIRCUITPY/code.py文件，你将会在BlueFi的LCD屏幕和串口控制台看到以下输出:

.. code-block:: 

      code.py output:
      1
      1
      2
      3
      5
      8
      13
      21
      34
      55

你可以改变调用Fibonacci生成器时的输入参数，无论你给任意大的数，除了输出数列的打印时间很长之外，内存消耗几乎保持不变。
这个示例程序的关键是第4行——“yield b”，程序执行到这里的时候会中断一次并返回b的当前值，然后再继续执行下一句——继续迭代，
直到while调节不成立。

通过本示例，我们掌握一种新的定义迭代对象的方法，该迭代器依然像“range()”函数一样地使用。

改进的read_file
-------------------------

对文件的读写操作也是Python程序中常用的操作，如果写文件可以用逐“字”增加的方法，那么读文件是否也可以逐“字”读取并处理？
这样的方法跟改进的斐波那契数列生成器一样节约内存，避免将整个文件读入内存再处理。对于有限内存资源的计算机系统来说，
这样地优化读文件操作非常有意义。

.. code-block::  python
  :linenos:

      def read_file(file): 
         BLOCK_SIZE = 256 
         with open(file, 'rb') as f: 
             while True: 
                 block = f.read(BLOCK_SIZE) 
                 if block: 
                     yield block 
                 else: 
                     return

这仅仅是一个改进的逐块读取文件的程序模型。关键的第7行——yield block，当程序执行到这里的时候，函数会中断一次并抛出
文件的一个块给函数调用者，然后继续执行下一句，继续读取下一个数据块，如果已经到文件末尾，则直接返回。

采用这个程序模型来读任意大的文件，实际消耗的内存几乎不变，仅与变量BLOCK_SIZE的值有关。

至此，你是否已经掌握Python的yield用法？事实上，yield还有更多种用法可以去探索。
