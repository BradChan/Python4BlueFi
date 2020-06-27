可视化的冒泡排序算法
======================

排序算法是最常用的数据处理算法，在商业数据处理和科学计算中占据重要地位。计算机的早期时代，30%的计算周期都花在排序上！
虽然今天的排序任务量占用计算周期的比例很低，但并不是排序不重要了，而是排序算法的发展非常成熟，计算效率已大大地提升。

支付宝的交易流水，我们习惯上按日期先后次序排序，如果你关心自己的钱主要花在什么地方时，按单次消费额的降序排列是你需要的排序。
今天的计算机几乎在帮助我们处理每天产生的巨量数据中，排序几乎是其首要工作。

排序算法几乎是所有计算机标准库中必备的，我们称之为排序函数。譬如Python语言的“list”类型数据的“sort”函数就是对列表中的
全部项目进行升序排列。虽然排序函数是现成可用的，我们仍有必要掌握排序算法：1)我们的排序耗时多久？已经是最优的算法？2)相似的
算法是否可以用来解决其他问题？

本节教程中，我们将掌握冒泡排序算法，并使用BlueFi的LCD屏幕将这一排序算法可视化。让算法可视化有助于我们理解算法的执行过程，
非常可视化冒泡排序的效果如下图：


.. image:: /../../_static/images/bluefi_advanced/bubbling_sort.gif
  :scale: 30%
  :align: center


---------------------------------


冒泡排序的数学基础
---------------------------------

对于给定的N个数值保存在一个有序的数据结构中，数组(array)、列表(list)等都是有序的数据结构，记为Data(N)。冒泡排序的步骤如下：

  - 第1步，确定Data(N)的最小值，即D0_min = min( Data(N) )，并将D0_min移到存储空间的首位
  - 第2步，确定剩余的Data(N-1)的最小值，即D1_min = min( Data(N-1) )，并将D1_min移到存储空间的第2个位置
  - 第3步，确定剩余的Data(N-2)的最小值，即D2_min = min( Data(N-2) )，并将D2_min移到存储空间的第3个位置
  - ..
  - 第N-1步，确定剩余的两个数值集，即Data(2)的最小值，即DN-1_min = min( Data(2) )，并将DN-1_min移到存储空间的第N-1个位置

这种逐步从数值集合中确定最小值并将最小值移到首位的排序过程，每一个数值集合的最小值像气泡一样地冒出水面，所以这种排序方法被形象地称作
冒泡排序。

数学上，确定数值集合Data(m)的最小值的过程就是m-1次数值比较，为了较少对数据单元的访问次数，两个数据比较之后，根据比较结果立即进行位置调整。
我们由此确定单步的确定给定数值集合的最小值，并将最小值移到存储空间首位的子程序如下：

.. code-block::  python
  :linenos:

  def bubbling_sort_step(dl):
      if len(dl)>1:
          for i in range( 1, len(dl) ):
              if dl[0] > dl[i]:
                  t = dl[0]
                  dl[0] = dl[i]
                  dl[i] = t
      return dl

  import random
  r = [random.randint(10 , 100)  for _ in range(5)]
  print(r)
  r0 = bubbling_sort_step(r)
  print(r0)
  r1 = bubbling_sort_step(r0[1:])
  print(r1)
  r2 = bubbling_sort_step(r1[1:])
  print(r2)
  r3 = bubbling_sort_step(r2[1:])
  print(r3)
  r4 = bubbling_sort_step(r3[1:])
  print(r4)

为了测试这个子程序，我们使用随机整数发生器产生5个10~100之间的随机数并存放在列表r中，然后使用列表的切片操作闭关调用这个子程序
逐步对5、4、3、2、1个数值集合进行处理：确定最小值，并将最小值移动到列表的首位。将这个示例程序保存到BlueFi的/CIRCUITPY/code.py
文件中，我们将在控制台看到以下输出：

.. code-block::  
  :linenos:

  code.py output:
  [70, 47, 17, 46, 62]
  [17, 70, 47, 46, 62]
  [46, 70, 47, 62]
  [47, 70, 62]
  [62, 70]
  [70]

输出到控制台的第二行的列表是随机整数发生器给出的5个原始随机数列表。第3行输出中，17这个最小值被捡出来并移到列表r0的首位。
第4行输出是对列表r0切片(r0[1:]代表第1项至末项的切片)调用子程序的处理结果。

这个子程序的的关键步骤是“for”循环，循环次数等于列表长度减去1，用首项逐个与其后的各项比较大小，遇到比首项更小的数据，我们将其移到
首项，并将原来的首项移到该位置。

通过这个示例，我们可以得到如下结论：

  - N个数据的冒泡排序需要N-1步
  - 冒泡排序的每一步需要执行m-1次数值大小比较计算，如果这一步有m个数值
  - N个数据的冒泡排序总计需要 ((N-1) + (N-2) + .. +  (N-(N-1))) 次数值比较计算，即 N*(N-1)/2次


冒泡排序算法
---------------------

根据前面的基础，我们可以给出“任意N个数值的冒泡排序算法”。基本思路是，假设原始的N个数值存放在一个列表中，使用两重循环对列表的各项
进行排序，内循环次数按(N-1)、(N-2)、..、1逐步递减。冒泡排序算法的代码如下：

.. code-block::  python
  :linenos:

  import random
  r = [random.randint(10 , 100)  for _ in range(7)]
  print("original: {}".format(r))
  for i in range(len(r)):
      for j in range(i+1, len(r)):
          if r[i]>r[j]:
              t = r[i]
              r[i] = r[j]
              r[j] = t
  print("sorted: {}".format(r))

这个算法消耗的内存非常少，除了存放原始数据的列表之外，仅仅多2个循环变量和1个临时变量。本示例使用随机整数发生器产生7个10~100范围内的
随机整数，然后进行排序，我们将排序前后的列表分别输出到控制台。将示例程序保存到BlueFi的/CIRCUITPY/code.py文件中，BlueFi执行排序
程序后在其LCD屏幕上将输出以下结果：

.. code-block::  python
  :linenos:

  code.py output:
  original: [63, 28, 44, 95, 14, 47, 18]
  sorted: [14, 18, 28, 44, 47, 63, 95]

或许你觉得这些算法太抽象，是否有更好的理解算法的方法？下面我们将一起设计一个可视化的冒泡排序过程，帮助你更好地理解排序算法。
我们首先从如何让屏幕上的精灵动起来，然后再设计更多个精灵代替数值列表，然后编程控制精灵随着排序过程而运动，把整个排序的过程用动画
效果呈现出来。


如何让屏幕上的精灵动起来
--------------------------

我们下面使用一个小示例，让一个方块再BlueFi的LCD屏幕上移动。

.. code-block::  python
  :linenos:

  import time
  import displayio
  from adafruit_display_shapes.rect import Rect
  from hiibot_bluefi.screen import Screen
  screen = Screen()
  group = displayio.Group(max_size=1)
  sprite = Rect(60, 160, 20, 20, outline=(255,0,0), fill=(255,0,0))
  group.append(sprite)
  screen.show(group)

  while True:
      time.sleep(0.3)
      sprite.y -= 100
      time.sleep(0.3)
      sprite.x += 100
      time.sleep(0.3)
      sprite.y += 100
      time.sleep(0.3)
      sprite.x -= 100

将该示例保存到BlueFi的/CIRCUITPY/code.py文件中，你将看到BlueFi执行这个示例的效果：一个红色小方块在屏幕上移动。小红色方块
的移动效果由无穷循环程序块的第12～19行程序来定义，根据其x和y坐标的增量确定。

这个程序的前4行语句是导入Python模块，第5行是实例化BlueFi的LCD屏幕，第6行定义一个图层(或称作图形元素组)，并指定最多一个元素。
这些是准备工作。然后，第7行定义一个红色方块并命名为“sprite”，第8行将这个红色方块/sprite添加到图层中。最后，在第9行程序中，
我们将图层显示到BlueFi的LCD屏幕上。一切就绪，我定义一个无穷循环，在循环程序块内不断地改变sprite的x和y坐标，为了能看到动画效果，
每次坐标的改变必须增加一些空操作，即使用time.sleep()函数让sprite暂停移动。如果我们把空操作的时间改为很短，譬如0.03秒，动画效果
会是怎么样？你可以试着修改程序并重新保存到BlueFi的/CIRCUITPY/code.py文件中来观察程序的运行效果。

如如何让两个sprite都能动起来呢？我们只需要对上面的示例代码稍作修改即可达到目的。

.. code-block::  python
  :linenos:

  import time
  import displayio
  from adafruit_display_shapes.rect import Rect
  from hiibot_bluefi.screen import Screen
  screen = Screen()
  group = displayio.Group(max_size=2)
  sprite1 = Rect(60, 160, 20, 20, outline=(255,0,0), fill=(255,0,0))
  group.append(sprite1)
  sprite2 = Rect(60, 60, 20, 20, outline=(255,255,0), fill=(255,255,0))
  group.append(sprite2)
  screen.show(group)

  while True:
      time.sleep(0.3)
      sprite1.y -= 100
      sprite2.x += 100
      time.sleep(0.3)
      sprite1.x += 100
      sprite2.y += 100
      time.sleep(0.3)
      sprite1.y += 100
      sprite2.x -= 100
      time.sleep(0.3)
      sprite1.x -= 100
      sprite2.y -= 100

代码修改思路是，在第7行和第9行分别定义两个不同颜色的精灵(sprite1和sprite2)，注意他们的初始坐标位置不同！并分别添加到图层(图层
中包含的最大集合元素数目也修改为2)，并在无穷循环程序块中依次改变两个精灵的坐标，实现两个精灵的动画效果，如下图所示。

.. image:: /../../_static/images/bluefi_advanced/two_sprites.gif
  :scale: 30%
  :align: center


至此，你已经知道如何定义图层和多个精灵，并将精灵添加到图层中，然后改变精灵的x和y坐标实现动画效果的编程思路和方法。

-----------------------------------------

让冒泡排序过程可见
-----------------------------------------

当我们已经掌握上述的基本知识和编程思路之后，接下来我们设计冒泡排序过程的动画效果，让冒泡排序算法的执行过程显示在屏幕上，帮助
编程新手理解该算法。

为简化问题，我们首先仅对3个随机整数进行冒泡排序，并设计他们动画效果。程序设计思路：1) 随机生成10~100范围的3个随机数，存放在一个列表中；
2) 定义图层，可容纳5个精灵；3) 定义3个不同颜色的方块(即3个精灵)，高度分别为列表中的随机数；4)定义2个不同颜色的圆(即2个精灵)，
用于指示排序期间正在比较的两个数据；5) 定义排序期间两个数据(高度)需要调换位置时两个精灵的移动动画；4) 对3个数据实施冒泡排序，
排序期间调用定义的动画实现排序算法的可视化。

具体的示例代码如下：

.. code-block::  python
  :linenos:

  import time
  import random
  import displayio
  from adafruit_display_shapes.rect import Rect
  from adafruit_display_shapes.circle import Circle
  from hiibot_bluefi.screen import Screen
  screen = Screen()
  speed = 0.3 # seconds for changing animation 
  height = [random.randint(10 , 100)  for _ in range(3)] # generate n random (10~100)
  gol = [0, 1, 2]    # list of the index of group elements
  x = [80, 120, 160] # list of x-coordinate for each sprite
  #  draw each sprite (3x rects)
  group = displayio.Group(max_size=6)
  sprite0 = Rect(x[0], 150-height[0], 20, height[0], outline = (0, 52, 255), fill = (0, 52, 255))
  group.append(sprite0)
  sprite1 = Rect(x[1], 150-height[1], 20, height[1], outline = (255, 0, 0), fill = (255, 0, 0))
  group.append(sprite1)
  sprite2 = Rect(x[2], 150-height[2], 20, height[2], outline = (212, 255, 0) , fill = (212, 255, 0))
  group.append(sprite2)
  #  draw the red dot and white dot to mark the current comparing pairs
  red_dot = Circle(85, 170, 5, outline=(255,0,0), fill=(255,0,0))
  group.append(red_dot)
  white_dot = Circle( 66, 170, 5, outline=(127,127,127), fill=(127,127,127) )
  group.append(white_dot)
  #  show thoese sprites onto BlueFi LCD screen
  screen.show(group)

  #  changing animation
  def animation_chg(l, r, steps):
      global group
      for _ in range( 8 ):
          group[l].x += 5*steps
          group[r].x -= 5*steps
          time.sleep(speed)

  #  no-change animation
  def animation_nochg(l, r):
      global group
      tf = group[l].fill
      for _ in range(2):
          time.sleep(speed/4)
          group[l].y -= 40
          time.sleep(speed/2)
          group[l].y += 40
          time.sleep(speed/4)
      group[l].fill = tf

  # sort and its animation
  for i in range(3): 
      time.sleep(0.1)
      red_dot.x = x[i]+5
      time.sleep(0.1)
      for j in range(i+1, 3):
          time.sleep(0.1)
          white_dot.x = x[j]+5
          time.sleep(0.1)
          if height[i] > height[j]:
              c1, c2 = height[j], gol[j]
              height[j], gol[j] = height[i], gol[i]
              height[i], gol[i] = c1, c2
              animation_chg(gol[j], gol[i], j-i)
          else:
              animation_nochg(gol[j], gol[i])

  while True:
      pass

代码看起来很长！但是很容易理解和实现，除了前6行是导入必要的Python模块外，定义5个精灵并分别添加到图层中，以及冒泡排序的程序都很容易理解，
的确只有3个整数的排序，只需要3*2/2(=3)次比较和移位就可以把三个整数排序完毕，这个示例的关键是动画部分。

我们定义了两个函数，分别叫animation_chg和animation_nochg。前者是为了实现两个精灵需要交换位置时的动画效果，输入参数是两个精灵对象：
l(代表左边的精灵)、r(代表右边的精灵)，另一个参数steps两个精灵之间的距离(屏幕的像素数)，根据这三个参数我们用连续8次改变两个精灵的x坐标并
做适当的暂停，实现两个精灵换位的动画效果；后者是当两个精灵不必交换位置时的动画效果，左边的精灵不动，右边精灵的y坐标连续改变2次，实现精灵
跳跃的动画效果。

在冒泡排序过程中，我们只是根据两个数据的大小确定是否需要换位，如果需要需要换位则先对数据列表操作(换位)，然后对三个精灵的位置列表也做一次
位置交换并调用animation_chg函数用动画来演示位置交换过程；如果不必交换位置，则调用animation_nochg用动画显示右边的精灵跳跃2次落回原处
表示不必交换位置。

将这个示例程序保存到BlueFi的/CIRCUITPY/code.py文件中，你将看到BlueFi执行这个示例的效果。记住我们这个教程的目的，让冒泡排序算法可见，
这可以帮助我们理解该算法。

最后我们给出本教程开始时的那个gif图所展示的“可视化的冒泡排序算法“的完整程序代码，虽然看起来很长，但是与上面示例相比仅仅是增加了更多个
待排序的随机数以及对应的精灵，程序思路完全相同。

.. code-block::  python
  :linenos:

  import time
  import random
  import displayio
  from adafruit_display_shapes.rect import Rect
  from adafruit_display_shapes.circle import Circle
  from hiibot_bluefi.screen import Screen
  screen = Screen()
  speed = 0.1 # seconds for changing animation 
  height = [random.randint(10 , 100)  for _ in range(7)]
  gol = [0, 1, 2, 3, 4, 5, 6]          # list of the index of group elements
  x = [26, 58, 90, 122, 154, 186, 218] # list of x-coordinate for each sprite
  #  creat a group of sprites (5x rects)
  group = displayio.Group(max_size=9)
  #  draw each sprite (5x rects)
  s0 = {'x':x[0] , 'y':150-height[0] , 'x2':20 , 'y2':height[0] , 'ot':(0, 52, 255) , 'fl':(0, 26, 255)}
  S0 = Rect(s0['x'] , s0['y'] , s0['x2'] , s0['y2'] , outline = s0['ot'] , fill = s0['fl'])
  group.append(S0)
  s1 = {'x':x[1] , 'y':150-height[1] , 'x2':20 , 'y2':height[1] , 'ot':(255, 0, 0) , 'fl':(255, 0, 0)}
  S1 = Rect(s1['x'] , s1['y'] , s1['x2'] , s1['y2'] , outline = s1['ot'] , fill = s1['fl'])
  group.append(S1)
  s2 = {'x':x[2] , 'y':150-height[2] , 'x2':20 , 'y2':height[2] , 'ot':(212, 255, 0) , 'fl':(212, 255, 0)}
  S2 = Rect(s2['x'] , s2['y'] , s2['x2'] , s2['y2'] , outline = s2['ot'] , fill = s2['fl'])
  group.append(S2)
  s3 = {'x':x[3] , 'y':150-height[3] , 'x2':20 , 'y2':height[3] , 'ot':(63, 255, 0) , 'fl':(63, 255, 0)}
  S3 = Rect(s3['x'] , s3['y'] , s3['x2'] , s3['y2'] , outline = s3['ot'] , fill = s3['fl'])
  group.append(S3)
  s4 = {'x':x[4] , 'y':150-height[4] , 'x2':20 , 'y2':height[4] , 'ot':(0, 216, 255) , 'fl':(0, 216, 255)}
  S4 = Rect(s4['x'] , s4['y'] , s4['x2'] , s4['y2'] , outline = s4['ot'] , fill = s4['fl'])
  group.append(S4)
  s5 = {'x':x[5] , 'y':150-height[5] , 'x2':20 , 'y2':height[5] , 'ot':(255, 0, 255) , 'fl':(255, 0, 255)}
  S5 = Rect(s5['x'] , s5['y'] , s5['x2'] , s5['y2'] , outline = s5['ot'] , fill = s5['fl'])
  group.append(S5)
  s6 = {'x':x[6] , 'y':150-height[6] , 'x2':20 , 'y2':height[6] , 'ot':(255, 216, 0) , 'fl':(255, 216, 0)}
  S6 = Rect(s6['x'] , s6['y'] , s6['x2'] , s6['y2'] , outline = s6['ot'] , fill = s6['fl'])
  group.append(S6)
  #  draw a red dot to mark the current minimum
  red_dot = Circle( 36, 170, 5, outline=(255,0,0), fill=(255,0,0) )
  group.append(red_dot)
  white_dot = Circle( 66, 170, 5, outline=(127,127,127), fill=(127,127,127) )
  group.append(white_dot)
  #  show thoese sprites onto BlueFi LCD screen
  screen.show(group)

  #  changing animation
  def animation_chg(l, r, steps):
      global group
      for _ in range( 8 ):
          time.sleep(speed)
          group[l].x += 4*steps
          group[r].x -= 4*steps
          #time.sleep(speed)

  #  no-change animation
  def animation_nochg(l, r):
      global group
      tf = group[l].fill
      for _ in range(2):
          time.sleep(speed)
          group[l].y -= 40
          time.sleep(speed)
          group[l].y += 40
          #time.sleep(speed/4)
      group[l].fill = tf

  # sort and its animation
  for i in range(7): 
      red_dot.x = x[i]+4
      time.sleep(0.1)
      for j in range(i+1, 7):
          time.sleep(0.1)
          white_dot.x = x[j]+4
          time.sleep(0.1)
          if height[i] > height[j]:
              # Exchange their positions, and exchange the index of group elements
              c1, c2 = height[j], gol[j]
              height[j], gol[j] = height[i], gol[i]
              height[i], gol[i] = c1, c2
              animation_chg(gol[j], gol[i], j-i)
          else:
              animation_nochg(gol[j], gol[i])

  while True:
      pass

为了理解Python的字典(dict)型数据结构及其使用方法，本示例中的方块精灵的参数均使用字典来描述，绘制精灵时的参数分别从字典中取。字典
是一种无序的数据集合，访问方法与列表型数据集合不同(列表是有序的数据集)。

最好的理解程序代码的方法：运行程序观察执行效果/结果，对照效果/结果来理解程序代码的作用。将这个示例程序保存到BlueFi
的/CIRCUITPY/code.py文件中，根据BlueFi的执行效果帮助你理解本示例。

