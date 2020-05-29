正在工作中..
====================

LED指示灯是最简单的一种输出设备，常用于指示计算机系统的内部状态。在BlueFi单板机上有两个可编程控制的LED指示灯，一个是红色的
(靠近电源指示灯)，另一个是白色的(靠近集成光学传感器)。所谓可编程控制的LED，就是我们可以用程序控制其亮或灭。

红色LED
----------------------

首先看一个示例：

.. code-block::  python
   :linenos:

    import time
    from hiibot_bluefi.basedio import LED
    led = LED()
    while True:
        led.red = 1
        time.sleep(0.5)
        led.red = 0
        time.sleep(0.5)

打开MU编辑器，点击“新建”按钮，并将本示例代码复制-粘贴的MU编辑器中，然后点击“保存”按钮，并在弹出的窗口中输入文件名为“code.py”,
保存文件的磁盘为“CIRCUITPY”，路径为该磁盘的根目录。一旦将code.py文件保存到BlueFi的CIRCUITPY磁盘上，BlueFi立即开始执行这个
脚本程序。

这个示例程序的执行效果是：红色LED指示灯不停地闪烁。闪烁的LED指示灯常用于指示计算机系统正在工作中，如果程序一旦停止，指示灯也停止闪烁。
如果你打开MU编辑器的串口控制台，按下“Ctrl+C”键让BlueFi终止执行code.py程序，进入REPL模式，你会发现红色LED指示灯不再闪烁。

下面我们逐行来分析每行脚本程序的效果，就像REPL一样地执行程序。

  示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/basedio.py”模块中导入一个名叫“LED”的类
    - 第3行，将导入的“LED”类实例化为一个实体对象，名叫“led”
    - 第4行，一个无穷循环的程序块
    - 第5行(无穷循环程序块的第1行)，led对象的red属性(led.red)设置为1
    - 第6行(无穷循环程序块的第2行)，执行time的sleep方法，参数为0.5秒
    - 第7行(无穷循环程序块的第3行)，led对象的red属性(led.red)设置为0
    - 第8行(无穷循环程序块的第4行)，执行time的sleep方法，参数为0.5秒

其中第1行和第2行都是导入Python的模块，为什么有两种不同的写法呢？第1行的导入方法，目的是将整个time模块导入到code.py中；
第2行的导入方法仅仅从hiibot_bluefi.basedio模块中导入LED类。在后续的学习中，你会发现hiibot_bluefi.basedio模块中包含多个类，
本示例仅仅使用LED类。事实上，Python的导入(import)模块的方法远不止这两种，如果需要深入了解import的其他方法，可以使用网络引擎
搜索“python import”查阅更多的介绍。

第3行是本示例的重点，执行这个语句的目的是将LED类实例化。

第5行和第7行也是本示例的重点，从两个语句中等号右边的值我们可以想象得出，一个语句是让红色LED亮，另一个语句是让红色LED灭。到底哪个才是让
红色LCD亮？我们可以借助于REPL模式分别单步执行其中一个语句，并观察红色LED指示灯的状态来确定。在MU编辑器的串口控制台窗口，按下“Ctrl+C”
键，让BlueFi进入REPL模式，然后在“>>”提示符后依次输入以下语句并按“Enter”键执行每一个语句：

.. image:: /../../_static/images/bluefi_basics/led_blink_repl1.jpg
  :scale: 40%
  :align: center

然后，观察执行“led.red=0”之后红色LED的状态：

.. image:: /../../_static/images/bluefi_basics/led_blink_repl2.jpg
  :scale: 40%
  :align: center

你现在是否已经能够确认“led.red=1”和“led.red=0”中哪一个是让红色LED灯亮？事实上，我们还有很多方法可以确认到底是哪一个能让红色LED亮。


白色LED
----------------------

修改前一个示例的代码，如下：

.. code-block::  python
   :linenos:

    import time
    from hiibot_bluefi.basedio import LED
    led = LED()
    while True:
        led.white = 1
        time.sleep(0.5)
        led.white = 0
        time.sleep(0.5)

我们仅仅修改亮第5和第7行程序中led的属性，red修改为white。点击Mu编辑器的“保存”按钮，修改后的文件默认保存到/CIRCUITPY/code.py，
BlueFi将重新开始执行新的程序，你很容易就发现白色LED灯的闪烁效果，因为被色LED灯非常亮(白色LED的实际作用是为集成光学传感器提供辅助光)。

对比两个示例容易发现，原来led对象的两个属性——red和white的值是分别用来控制红色LED和白色LED的。我们不禁想问，这个led对象到底有多少个属性？
找个问题的答案仍可以求助于REPL。现在就点击“串口”按钮，在串口控制台窗口按“Ctrl+C”键，让BlueFi进入REPL模式，在REPL的提示符“>>”依次执行
以下语句，最后一句是输入“led.”并按Tab键，REPL将把led对象的全部属性和方法都列举出来。如下图：

.. image::  /../../_static/images/bluefi_basics/led_blink_ledtab.jpg
  :scale: 20%
  :align: center

led对象总共有4个属性？这些信息并不能确定led对象所支持的4个接口是什么，如果我们使用“help(led)”将会得到更详细的信息。如下图：

.. image::  /../../_static/images/bluefi_basics/led_blink_helpled.jpg
  :scale: 20%
  :align: center

其中，red和white分别是led对象的两个属性，redToggle和whiteToggle分别是led对象的两个函数，修改示例程序，观察这两个函数的功能。

.. code-block::  python
   :linenos:

    import time
    from hiibot_bluefi.basedio import LED
    led = LED()
    while True:
        led.whiteToggle()
        time.sleep(0.5)

这个程序中的无穷循环程序块仅有两个语句，一个是调用led对象的函数——whiteToggle()另一个仍是延时0.5秒。从程序执行效果看，这个程序与示例2
几乎完全相同。说明led对象的whiteToggle()函数是在切换白色LED的亮和灭。

你能使用led对象的另一个函数来修改程序，实现红色LED指示灯闪烁？

.. admonition:: 
  总结：

    - Python的程序块使用相同的行缩进空格数来界定
    - Python的import有很多种用法，本节我们使用过两种方法
    - Python中的导入的类，使用前必须先实例化, led=LED()
    - 实体对象的属性赋值

      - led.red=1(红色LED亮)
      - led.red=0(红色LED灭)
      - led.white=1(白色LED亮)
      - led.white=0(白色LED灭)
      
    - 实体对象的函数调用
      
      - led.redToggle() (切换红色LED的状态)
      - led.whiteToggle() (切换白色LED的状态)

    - 本节中，你总计完成了12行代码的编写工作

