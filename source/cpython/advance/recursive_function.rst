递归函数及其应用
======================

递归函数(recursive function)并不是Python编程语言专有的，几乎所有的计算机编程语言都支持递归。如果一个函数能够直接地或间接地
调用函数自身，他就属于递归函数。

数学上，一个映射“f: x -> y”，即y=f(x)，该映射的定义域X中的某个取值x0，该映射的值域中有f(x0)与之对应，如果f(x0)由f(f(x0))
决定，那么这个映射属于递归映射。

简单地说，一个计算过程，如果他的每一步计算结果都由他的前1～n步的计算结果所确定，该过程就属于递归过程。斐波那契数列的计算过程就是一种
典型的递归计算过程，除了第1步和第2步之外，每一步计算都是前两步计算结果之和。譬如，连加、连乘和阶乘的计算过程都可以使用递归过程，当
我们需要这些计算过程中的每一个中间结果时，递归计算过程就尤为重要。

本节教程中，我们一起使用Python作为编程语言掌握递归函数或递归计算过程的设计方法。

---------------------------------


如何定义一个递归函数(递归深度)
---------------------------------

递归过程必须是有限次的计算过程。定义递归函数时，必须首先确定终止递归的条件。

虽然斐波那契数列是可以无穷地递归下去(递归次数越多，相邻两个数的比值越接近黄金分割比)，我国古代数学名著——九章算术中的杨辉三角，
以及易经中的“无极生太极，太极生两仪，两仪生四象，四象生八卦”等递归过程都是可以无穷递归下去，对于特定场景适合我们使用的都是有限的。

我们只需要前N个斐波那契数，我们只需要展示N行的杨辉三角。这些都是终止递归的条件。

下面我们给一个示例，目标是在BlueFi的LCD屏幕上或串口控制台上输出N行/个斐波那契数列。
我们先观察示例程序的执行效果，再总结递归函数的架构。示例代码如下：

.. code-block::  python
  :linenos:

  N=12
  def recursiveFib(n, i=0, j=1):
      if n<1:
          return
      i, j = j, i+j
      print( "the {}-th: {}".format(N-n+1, i) )
      recursiveFib(n-1, i, j)

  recursiveFib(N)

将示例代码保存到BlueFi的/CIRCUITPY/code.py文件，执行结果如下：

.. code-block:: 
  :linenos:

    code.py output:
    the 1-th: 1
    the 2-th: 1
    the 3-th: 2
    the 4-th: 3
    the 5-th: 5
    the 6-th: 8
    the 7-th: 13
    the 8-th: 21
    the 9-th: 34
    the 10-th: 55
    the 11-th: 89
    the 12-th: 144

这个结果将显示在BlueFi的LCD屏幕上。如果你需要输出更多行斐波那契数列，只需要修改第1行程序的变量N的值即可。

.. Attention::  递归深度：

    - RuntimeError: maximum recursion depth exceeded。当你随便修改前面示例中N的值时，会遇到这样的错误提示
    - 为防止递归调用陷入死循环，所有支持递归函数的计算机编程环境都会限制递归调用的次数，或称递归深度

你可以使用前面的示例确定我们的BlueFi所限定的递归深度到底是多少。这样示例说明，软件测试非常重要，不断地改变N的值，
测试程序的执行结果，当出现错误提示的时候，务必仔细追查错误原因。

我们现在再回来看这个示例程序，递归函数——recursiveFib有三个输入参数：1) 第1个参数n指定输出的数列个数；
2) 第2和第3个参数分别是数列的前两项，他们的默认值分别为0和1.

我们在主程序中调用递归函数recursiveFib时，只给定第1个参数，第2和第3个参数使用默认值(分别为0和1)。首次调用
执行递归函数recursiveFib时，第一个参数只要不小于1，必定会执行一次递归调用，就是该函数的最后一个语句：
recursiveFib(n-1, i, j)，每递归调用1次，第一个参数会减少1，执行该语句就是递归调用。递归调用为什么不会陷入
死循环呢？递归函数的第1个语句就是终止递归调用的条件：n<1，每执行一次递归调用，n会减少1，有限次递归调用后，必定
会满足“n<1”这个终止条件。

我们可以将if条件的语句换一种写法，同样效果的示例程序代码如下：

.. code-block::  python
  :linenos:

  N=16
  def recursiveFib(n, i=0, j=1):
      if n>=1:
          i, j = j, i+j
          print( "the {}-th: {}".format(N-n+1, i) )
          recursiveFib(n-1, i, j)

  recursiveFib(N)

如此修改后的递归函数recursiveFib的逻辑是“满足条件(n>=1)则继续递归”，与该示例的原始逻辑是“满足条件(n<1)则终止递归”。
两种逻辑显然是等价的。

那么我们就有两种递归函数的基本架构，一种是“满足条件则继续递归”的架构：

.. code-block::  python
  :linenos:

  def recursiveFun(var):
      ..
      if ( cond ):
          ..
          recursiveFun( v )

另一种是“满足条件则终止递归”的架构：

.. code-block::  python
  :linenos:

  def recursiveFun(var):
      ..
      if ( cond ):
          return 
      ..
      recursiveFun( v )

两种架构是等价的。再次提醒，虽然递归都是有限次的递归调用，但要注意递归深度。每一种计算机系统环境的递归深度未必一致，
使用递归函数实现的某些功能请务必仔细测试，尤其边界测试十分地重要。


递归函数的应用1: 绘制“迷宫图案”
---------------------------------

妙用递归函数，我们将会达到事半功倍效果。短小的代码，实现强大的功能。我们先来看下一个示例程序的执行效果。如下图：

.. image:: /../../_static/images/cpython_advanced/recursive_maze.gif
  :scale: 30%
  :align: center

这是由数十根长度不同的红色直线顺序连接而成的迷宫图案，我们定一个递归函数绘制所有直线，关键的程序代码仅不到10行。
具体示例代码如下：

.. code-block::  python
  :linenos:

  from adafruit_turtle import Color,turtle
  from hiibot_bluefi.screen import Screen
  screen = Screen()
  drawPen = turtle(screen)

  drawPen.speed(6)
  drawPen.pensize(1)
  drawPen.pencolor(Color.RED)
  drawPen.setposition(-119,-119)
  drawPen.pendown()

  def drawLineRecursion(length):
      if length > 4:
          drawPen.forward(length)
          drawPen.right(90)
          length -= 4
          drawLineRecursion(length)

  drawLineRecursion(239)
  drawPen.ht()

  while True:
      pass

前2行是导入本示例所用的Python模块，然后在后续的两行代码实例化BlueFi的LCD屏，并将turtle绘图模块示例化为drawPen，这里的turtle(screen)目的
是指定turtle画笔在BlueFi屏幕上绘图。第6～10行程序是画图前准备工作，包括：设置绘图速度(0:最快，6:中等速度)，画笔粗细(pensize)，画笔颜色(
pencolor)，首次落笔的位置坐标(setposition)，落笔(pendown)。

接着我们定一个递归函数——drawLineRecursion，输入参数length指定待绘制的直线长度。这个递归函数采用“满足条件(length>4)则递归”的架构。如果满足
递归条件，首先绘制一条长为length的直线(forward(length))，然后画笔右转(right)90度，并将线长度减少4，递归调用drawLineRecursion。

在主程序中调用递归函数drawLineRecursion时传入的初始直线长度为239(BlueFi的屏幕宽度和高度都是240)，递归调用函数drawLineRecursion绘制第一根
红色直线，然后length减少4，即length=235，然后递归调用函数drawLineRecursion绘制第二根直线，如此重复直到条件“length>4”不成立，则终止递归
调用，此时迷宫图案已经绘制完毕。然后隐藏turtle图标(ht)。

这个程序仅用6行语句定义的递归函数drawLineRecursion就可以绘制完整的迷宫图案，程序的高效率完全归功于递归函数的益处。


递归函数的应用2: 绘制“二叉树”
---------------------------------

这个案例的执行效果如下图，绘制三颗不同大小的“二叉树”。所谓二叉树就是数的每一个节点分支仅有2个分叉。这样的图案看起来规律很明显，但是凭直觉
又觉得绘制该图案比较难。程序效果如下图所示：

.. image:: /../../_static/images/cpython_advanced/recursive_binarytree.gif
  :scale: 30%
  :align: center

这个“二叉树”图案的绘制方法：先绘制最右边的树叉，每绘制一根树叉时，树叉粗细(画笔粗细)缩小20%，树叉长度减少给定的值(branch_diffence)，
最右边的树叉绘制结束的条件为：branchLength<5，然后逐步回退一步绘制左边的树叉。示例程序代码如下：

.. code-block::  python
  :linenos:

  from adafruit_turtle import Color, turtle
  from hiibot_bluefi.screen import Screen
  screen = Screen()
  t = turtle(screen)
  t.speed(0)
  t.hideturtle()

  #  define a recursive function to draw a binary tree
  def recursiveDrawBranch(branchLength):
      global t, branch_diffence
      if branchLength >= 5:
          if branchLength - branch_diffence <= 5:
              t.pencolor(Color.GREEN)
          else:
              t.pencolor(Color.BROWN)
          t.pensize((branchLength * 0.2))
          t.pendown()
          t.backward(1)
          t.forward(branchLength)
          t.right(20)
          recursiveDrawBranch(branchLength - branch_diffence)
          t.left(40)
          recursiveDrawBranch(branchLength - branch_diffence)
          t.penup()
          t.right(20)
          t.backward(branchLength)

  t.setposition(-50, -110)
  branch_diffence = 10
  recursiveDrawBranch(45)

  t.setposition(60, -100)
  branch_diffence = 8
  recursiveDrawBranch(40)

  t.setposition(0, -120)
  branch_diffence = 15
  recursiveDrawBranch(80)

  while True:
      pass

为了便于理解，请你将该示例代码保存到BlueFi的/CIRCUITPY/code.py文件中，观察执行过程，再对照程序代码，很容易理解该示例程序。

这个示例程序的关键代码是递归函数recursiveDrawBranch，我们这个递归函数内部使用连两次递归调用，第21行的递归调用是为了绘制
“二叉树”右边的分支，执行这行递归调用时将持续到“递归条件branchLength>=5”不成立，如此递归将右边分叉绘制完毕，再继续绘制左边
分叉。

递归函数的应用3: 绘制“写意的水墨画”
-----------------------------------

前一个示例绘制的图案非常规则，修改上述示例并增加画笔颜色的变化、右转和左转的角度使用随机数产生(自然界的树枝分叉角度大多数都是随机的，
分叉角度与各种外界条件有关)，可以达成另一种非常写意的图案效果——水墨画。示例代码如下：

.. code-block::  python
  :linenos:

  # draw a Sakura tree (it is a very enjoyable works)
  from hiibot_bluefi.screen import Screen
  from adafruit_turtle import Color, turtle
  import random
  screen = Screen()
  t = turtle(screen)

  #  draw Sakura tree with a recursive method
  def draw_Sakura_Tree(branchLen, t):
      if branchLen >3:
          if 8<=branchLen and branchLen<=12:
              if random.randint(0, 2) == 0:
                  t.pencolor(Color.WHITE)
              else:
                  t.pencolor(Color.ORANGE)
              t.pensize(branchLen / 3)
          elif branchLen<8:
              if random.randint(0,1) == 0:
                  t.pencolor(Color.WHITE)
              else:
                  t.pencolor(Color.ORANGE)
              t.pensize(branchLen / 2)
          else:
              t.pencolor(Color.BROWN)
              t.pensize(branchLen/10)
          t.forward(branchLen)
          a = 1.5*random.random()
          t.right(20*a)
          b = 1.5*random.random()
          # ready! recursive
          draw_Sakura_Tree(branchLen-10*b, t)
          t.left(40*a)
          draw_Sakura_Tree(branchLen-10*b, t)
          t.right(20*a)
          t.up()
          t.backward(branchLen)
          t.down()

  # 0: the fastest speed
  t.speed(0)
  t.hideturtle()
  t.up()
  t.backward(120)
  t.down()
  t.pencolor(Color.BROWN)

  # call a recursive function to draw Sakura tree
  draw_Sakura_Tree(50, t)

  while True:
      pass

建议你将本示例程序代码保存到BlueFi的/CIRCUITPY/code.py文件中，观察执行过程，再对照程序代码，很容易理解该示例程序。
由于增加随机数发生器来选择树枝分叉的角度，以及每节树枝的长度，你会发现每次重新执行程序后就输出一张完全不同的“写意山水画”！
这也是随机的集聚效应。

----------------------

.. admonition::  总结：

  - 递归过程必须是有限次的，定义递归函数必须先确定递归终止条件
  - 递归深度跟计算系统的环境有关，必须仔细测试程序能够达到的最大递归深度
  - 递归函数的逻辑框架1: 满足条件则执行递归
  - 递归函数的逻辑框架2: 满足条件则终止递归
