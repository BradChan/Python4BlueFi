list及其操作
======================

list(列表)是Python语言的一种基本数据类型，形式简洁但功能强大。本节教程我们了解Python的列表型对象及其操作。


---------------------------------


定义list的方法
---------------------------------

最简单的定义list型对象的方法就是“l = []”，这个语句的目的是定义一个空的列表，其中“[]”是Python中列表的界定符号，
任意对象的列表都必须使用“[]”进行界定，如“[0,1,2,3,4]”是一个最简单的列表，包含有5个数据。

使用“l = [0,1,2,3,19,7]”定义一个包含6个数据的列表。这种方法常用于定义一个常量列表。譬如，我们可以定义一组RGB颜色列表，
“colors = [ (255,0,0), (255,127,0), (127,127,0), (0,255,0), (0,255,127), (0,0,255), (0,127,127) ]”，
列表中每一项是一个三基色元组数据。

列表的各项不必是相同的类型，如“l = [1, 'hiibot', 'a', 9, [0,1,2], 10]”不仅包含有数值型、字符串型列表项，
还包含有子列表项。可见，列表是一种数据容器，可以包含多种不同类型的

使用迭代器定义列表，如“l = [x**2 for x in range(10)]”定义0~9共10个整数的平方的列表，即
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]。“range()”是Python内置的基本迭代器。类似地，我们用下面的代码可以定义一个随机数列表：

.. code-block::  python
  :linenos:

  import random
  r = [random.randint(10 , 100)  for _ in range(5)]
  print(r)

将程序保存到BlueFi的/CIRCUITPY/code.py文件中，执行结果为：

.. code-block::  python
  :linenos:

  code.py output:
  [80, 63, 29, 63, 46]

我们使用“random.randint(10 , 100)”和迭代器“range(5)”生成一个包含有5项10~100之间的随机整数，所以你每次执行程序时，输出结果都不同。

更多种定义列表的方法请查阅Python语言的相关参考资料。

访问list某一项的方法
---------------------------------

列表是一种有序的数据结构，他的每一项都有一个固定的索引值(index)，通过索引值可以访问列表的每一项。

如果定义“l = [1, 'hiibot', 'a', 9, [0,1,2], 10]”，则“l[0]”代表列表中的首项(即数值1)，“l[1]”代表'hiibot'字符串，
“l[4]”代表子列表“[0,1,2]”。很显然，列表的每一项都作为一个整体来访问，无论该项的是什么类型。如果你需要访问列表中的子列表
的某一项，仍继续使用“[index]”形式，如“l[4][1]”代表子列表“[0,1,2]”的第1项(即数值1)。对于的字符串型列表项，如“l[1][3]”
代表'hiibot'的第3个字符(即字符‘b’)。

上面这些访问方法都是从第0项开始向列表尾部逐项索引，Python的列表访问也可以逆序进行，譬如“l[-1]”代表列表最后一项(即数值10)，
“l[-4]”代表字符‘a’。

总之，作为一种有序的数据结构，列表的每一项都可以使用“listName[index]”来访问，如果index是正数，默认从列表的第0项到
第“len(l)-1”项(即列表的最后一项)；如果index为负数，默认从列表的最后一项到第0项。

特别值得注意的是，使用“listName[index]”访问列表中的第index项，index的有效值：-len(listName) ~ 0 ~ len(listName)-1。
如果定义“l = [1, 'hiibot', 'a', 9, [0,1,2], 10]”，index的有效值是 -6 ~ 0 ~ 5。如果index超出有效值范围，将会导致脚本
程序执行错误，系统将给出错误提示“IndexError: list index out of range”。

list的切片操作
--------------------------------

list的切片，即截取list的某些连续子项组成一个子列表。list切片操作的操作符为“:”.

将BlueFi连接到电脑，打开MU编辑器并按“串口”按钮和“Ctrl+c”键，进入REPL模式，执行以下的程序可以更好地理解列表的切片操作：

.. code-block::  python
  :linenos:

  >>> l = [1, 'hiibot', 'a', 9, [0,1,2], 10]
  >>> l[1:3]
  ['hiibot', 'a']
  >>> l[3:5]
  [9, [0, 1, 2]]
  >>> l[3:]
  [9, [0, 1, 2], 10]
  >>> l[:3]
  [1, 'hiibot', 'a']
  >>> 

列表切片操作符“:”前的数值为切片起始项index，“:”后的数值为切片终止项index，其中终止项不包含在切片中。如果没有指定起始项时，默认从
列表的首项开始；如果没有指定终止项时，默认为列表的最后一项。

数值list的计算操作
----------------------------

数值型列表中的各项都是数值，Python的计算类函数和list的内部函数可以对列表实施数值计算，譬如确定最大值、最小值、排序、逆序、列表的项数等计算。

将BlueFi连接到电脑，打开MU编辑器并按“串口”按钮和“Ctrl+c”键，进入REPL模式，执行以下的程序可以更好地理解数值型列表的计算：

.. code-block::  python
  :linenos:

  >>> import random
  >>> r = [random.randint(10 , 100)  for _ in range(5)]
  >>> r
  [42, 39, 16, 19, 96]
  >>> min(r)
  16
  >>> max(r)
  96
  >>> r.sort()
  >>> r
  [16, 19, 39, 42, 96]
  >>> r.reverse()
  >>> r
  [96, 42, 39, 19, 16]
  >>> len(r)
  5
  >>>

这个示例中，5个随机数组成一个数值型列表，使用Python内部函数“min()”和“max()”分别确定数值型列表中最小值和最大值，并使用
list的内部函数“sort()”对数值型列表各项实施升序排列，以及list的内部函数“reverse()”对列表实施逆序排列。

list没有内部函数直接实现数值型列表的降序排列，组合“sort()”和“reverse()”函数可以实现数值型列表的降序排列。


list的内部函数
--------------------------------

使用list的内部函数我们可以对列表实施追加、移除、插入、搜索、统计、扩展等操作。在BlueFi的REPL模式，我们使用“dir()”或“help()”
函数可以确定BlueFi所支持的列表内部函数：

.. code-block::  python
  :linenos:

  >>> l = [x for x in range(10)]
  >>> dir(l)
  ['__class__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
  >>> 

list的内部函数简要说明：

  - append()，在列表末尾追加一个新的列表项，如“l.append( obj )”，其中“obj”可以是任意类型的Python对象
  - clear()，清除列表(结果为一个空列表)，如“l.clear()”，执行后l是一个空列表，相当于“l=[]”
  - copy()，列表的副本，如“lc = l.copy()”
  - count()，统计指定的对象出现在列表中的总次数，如“count_obj = l.count( obj )”
  - extend()，使用新的对象扩展列表，如“l.extend( [11, 12, 13] )”，相当于将列表“[11,12,13]”追加到原列表的尾部
  - index()，确定指定对象在列表中的索引，如“p5=l.index(5)”。注意，如果指定在对象在列表中有多个，将返回第一个出现指定对象的位置
  - insert()，将指定的对象插入到列表中的指定位置，如“l.insert( 3, obj )”，将对象插入到第3项的后面
  - pop()，移除列表的末项或指定项，如“l.pop()”移除列表的末项，如“l.pop(3)”移除列表中的第3项
  - remove()，从列表移除指定的对象，如“l.remove( obj )”
  - reverse()，将列表逆序排列，即首项变为末项，按逆序重新排列列表
  - sort()，将列表按升序排列，这个函数仅对数值型列表有效，如果列表中有非数值项将会发生错误

下面的示例中，我们使用列表的内部函数对list实施一些操作，帮助你理解列表的一些操作：

.. code-block::  python
  :linenos:

  import random
  r = [random.randint(10 , 100)  for _ in range(5)]
  print( "list:{}, length:{}".format(r, len(r)) )
  r.append(88), r.append(99)
  print( "list:{}, length:{}".format(r, len(r)) )
  print( r.count(88) )
  print( r.index(88) )
  r.remove(88)
  print( "list:{}, length:{}".format(r, len(r)) )
  r.pop()
  print( "list:{}, length:{}".format(r, len(r)) )
  r.insert(2, 88)
  print( "list:{}, length:{}".format(r, len(r)) )
  print( r.index(88) )
  r.sort()
  print( "list:{}, length:{}".format(r, len(r)) )
  r.reverse()
  print( "list:{}, length:{}".format(r, len(r)) )

将上述示例程序保存到BlueFi的/CIRCUITPY/code.py文件，执行结果如下：

.. code-block::  python
  :linenos:

  code.py output:
  list:[17, 68, 39, 89, 23], length:5
  list:[17, 68, 39, 89, 23, 88, 99], length:7
  1
  5
  list:[17, 68, 39, 89, 23, 99], length:6
  list:[17, 68, 39, 89, 23], length:5
  list:[17, 68, 88, 39, 89, 23], length:6
  2
  list:[17, 23, 39, 68, 88, 89], length:6
  list:[89, 88, 68, 39, 23, 17], length:6

遍历list
--------------------------

使用“for”程序结构我们可以遍历list的各项：

  - for  term  in  list

这种遍历列表的程序结构相当于一种生成器，每个循环会返回列表list中的一个对象，遍历顺序从首项到末项，循环次数等于len(list)。

下面的示例中，我们使用list及其内部函数定义一个呼吸灯亮度的数据列表，然后使用这个列表控制BlueFi白光灯的亮度，你将会看到
白光灯的“呼吸”效果。程序代码如下：

.. code-block::  python
  :linenos:

  import time
  from hiibot_bluefi.basedio import PWMLED
  led = PWMLED()
  l = [x for x in range(0, 65535, 5535)]
  lc = l.copy()
  lc.reverse()
  l.extend(lc)

  while True:
      for b in l:
          led.white = b//5
          time.sleep(0.1)

注意，第11行代码的“b//5”目的是确保亮度设定值为整数，并将白光灯的亮度衰减到20%，避免过亮刺眼。与前面教程中的LED呼吸灯效果相比，
这个示例程序的效果几乎完全相似，但是该示例中我们使用list定义一组“呼吸”规律变化的数据列表，避免计算和逻辑判断并实现同样的效果。
程序执行效果如下图：

.. image:: /../../_static/images/bluefi_advanced/list_fadeWhiteLed.gif
  :scale: 50%
  :align: center

本示例程序的第4～7行的4个语句中，首先生成一个列表l，即[0, 5535, 11070, .., 60885]；然后复制另一个列表lc；将列表lc倒序；
并用lc扩展原列表l，扩展后的列表l=[0, 5535, 11070, .., 60885, 60885, .., 11070, 5535, 0]，列表中共有22项数据，22项数值
的变化规律：从0逐渐变大到60885，然后再从60885逐渐变小到0。将这一规律的护具列表当作白色LED的亮度，我们就看到“呼吸灯”效果。

对上面的示例稍作修改，我们即可实现BlueFi的5颗RGB彩灯呈现“呼吸”效果：所有灯珠从灭逐渐变为红，然后从红渐变灭。效果如下图：

.. image:: /../../_static/images/bluefi_advanced/list_fadeRGB_red.gif
  :scale: 50%
  :align: center

对应的程序代码如下：

.. code-block::  python
  :linenos:

  import time
  from hiibot_bluefi.basedio import NeoPixel
  pixels = NeoPixel()
  r = [x for x in range(0, 255, 25)]
  rc = r.copy()
  rc.reverse()
  r.extend(rc)

  while True:
      for b in r:
          pixels.fillPixels((b, 0, 0))
          pixels.pixels.show()
          time.sleep(0.1)

这个示例中原始列表r，即[0, 25, 50, .., 250]为RGB颜色的单分量的值，取值范围0～255。我们使用列表的复制、逆序和扩展等操作生成
一个红色分量的渐变最大再渐变为0的列表，使用“for  term  in  list”遍历列表，以及“pixels.fillPixels((b, 0, 0))”和
“pixels.pixels.show()”让BlueFi的5颗RGB彩灯实现渐变红再渐变灭的“呼吸”效果。


-----------------------------

.. admonition:: 
  总结：

    - list
    - 列表的定义
    - 访问列表中的某一项
    - 访问列表中的某些项：列表切片
    - 列表的操作：排序和倒序/逆序、插入和移除、追加和弹出、统计、清除、扩展等
    - 列表的遍历
    - 列表的应用

------------------------------------

