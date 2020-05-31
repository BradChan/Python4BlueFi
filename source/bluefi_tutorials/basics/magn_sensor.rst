地磁计和指南针
======================

望文生义，地磁计就是利用传感器测量地球磁场的方法来确定物体水平方向(与地面垂直的方向)上的朝向。我们四大发明之一的指南针正是
利用地球磁场的原理，古代人们就已发现地球是一个很大的磁铁，而且南北极朝向南北极始终保持不变，周围无更强磁场的小磁铁受到地磁
方向的影响也始终朝向一个固定的方向。指南针的小磁极的指向始终保持不变，这样就可以帮助我们确定正确的方向，指南针诞生后对大航海
时代的发展起到积极推进作用，那个时代在海上行船全靠指南针(罗盘)和北斗星的指引，他们都具有始终不变的方向。

今天我们知道地球磁场并不稳定，甚至有科学预言，地球磁极会发生翻转。地磁南北极与地球南北极并不一致，如下图所示，他们方向相反，
而且两对极点连线之间存在夹角。

.. image:: /../../_static/images/bluefi_basics/magnetic_sn.jpeg
  :scale: 100%
  :align: center


--------------------------------

地磁计的数据
--------------------------------

使用下面的程序示例，观察地磁计数据的变化规律。示例程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    screen = Screen()
    sensors = Sensors()
    magn = sensors.magnetic
    show_data = screen.simple_text_display(title="BlueFi Magnetometer", title_scale=1, text_scale=2)
    while True:
        magn = sensors.magnetic
        show_data[2].text = "X: {:.2f}".format(magn[0])
        show_data[2].color = screen.RED
        show_data[3].text = "Y: {:.2f}".format(magn[1])
        show_data[3].color = screen.GREEN
        show_data[4].text = "Z: {:.2f}".format(magn[2])
        show_data[4].color = screen.BLUE
        show_data.show()
        time.sleep(0.1)

将本示例程序保存到BlueFi的/CIRCUITPY/code.py文件中，当BlueFi执行示例程序时，旋转BlueFi并观察地磁计给出的三个分量数值的变化。

示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/sensors.py”模块中导入一个名叫“Sensors”的类
    - 第3行，从“/CIRCUITPY/lib/hiibot_bluefi/screen.py”模块中导入一个名叫“Screen”的类
    - 第4行，将导入的“Screen”类实例化为一个实体对象，名叫“screen”
    - 第5行，将导入的“Sensors”类实例化为一个实体对象，名叫“ensors”
    - 第6行，定义一个名叫“magn”的Sensors类gyro型变量，并赋初值为sensors.magnetic
    - 第7行，定义一个名叫“show_data”的Screen类的多行文本显示子类，设置文本显示的标题为“BlueFi Gyroscope”，标题字体放大1倍，文本字体放大2倍
    - 第8行，一个无穷循环的程序块
    - 第9行(无穷循环程序块的第1行)，用sensors.magnetic更新变量magn
    - 第10行(无穷循环程序块的第2行)，设置多行文本显示子类的第2行的文本内容为magn0]的值
    - 第11行(无穷循环程序块的第3行)，设置多行文本显示子类的第2行的文本颜色为红色
    - 第12行(无穷循环程序块的第4行)，设置多行文本显示子类的第3行的文本内容为magn[1]的值
    - 第13行(无穷循环程序块的第5行)，设置多行文本显示子类的第3行的文本颜色为绿色
    - 第14行(无穷循环程序块的第6行)，设置多行文本显示子类的第4行的文本内容为magn[2]的值
    - 第15行(无穷循环程序块的第7行)，设置多行文本显示子类的第4行的文本颜色为蓝色
    - 第16行(无穷循环程序块的第8行)，更新多行文本显示
    - 第17行(无穷循环程序块的第9行)，执行time的sleep方法，参数为0.1秒

通过本示例的数据信息，你观察到地磁计的数据与BlueFi的姿态和朝向之间存在什么样的关系？


指北针和电子罗盘
--------------------------------

我们修改前一个示例，增加北方的指示，我们以BlueFi金手指的方向为准，当BlueFi的LCD显示器保持与地水平面平行时，
金手指对着正北方时地磁计指北针归零，旋转BlueFi过程中，将给出BlueFi的金手指水平方向与正北方的夹角。示例程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    screen = Screen()
    sensors = Sensors()
    sensors.MagnRange = 0
    magn = sensors.compassHeading
    show_data = screen.simple_text_display(title="BlueFi Magnetometer", title_scale=1, text_scale=2)
    while True:
        magn = sensors.magnetic
        show_data[2].text = "X: {:.2f}".format(magn[0])
        show_data[2].color = screen.RED
        show_data[3].text = "Y: {:.2f}".format(magn[1])
        show_data[3].color = screen.GREEN
        show_data[4].text = "Z: {:.2f}".format(magn[2])
        show_data[4].color = screen.BLUE
        show_data[5].text = "North: {}".format(sensors.compassHeading)
        show_data[5].color = screen.YELLOW
        show_data.show()
        time.sleep(0.1)

在前一个示例的基础上，把BlueFi的金手指水平方向与正北方的夹角显示在多行文本的第5行，并使用黄色字体。

现在你知道为什么指南针并不指向南方而是指向北方的原因了吗？

-----------------------------

.. admonition:: 
  总结：

    - 地球磁场
    - 地磁南北极和地理南北极
    - 地磁计
    - 指南针
    - 多行文本显示的数据结构
    - 文本字体的缩放
    - 本节中，你总计完成了17行代码的编写工作

------------------------------------

.. Important::
  **Sensors类的地磁传感器接口**

    - magnetic (属性, 元组类型, 只读, 每个分量的有效值: -10.24~+10.24), BlueFi的Sensors类magnetic属性, 地磁计的三个分量值

      - magnetic[0]: x方向分量
      - magnetic[1]: y方向分量
      - magnetic[2]: z方向分量

    - compassHeading (属性, 只读, 有效值: 0~359), BlueFi的Sensors类指北针方向属性, 给出当前方向与正北方向的夹角
    - MagnRange (属性, 可读可写, 有效值: 0~3), BlueFi的Sensors类地磁计量程属性, 0:4G, 1:8G, 2:12G, 3:16Guass
    - MagnRate (属性, 可读可写, 有效值: 0~10), BlueFi的Sensors类地磁传感器数据更新率属性, 0:0.625Hz, 1:1.25Hz, 2:2.5Hz, 3:10Hz, .., 9:560hz, 10:1KHz
    - MagnPerforMode ((属性, 可读可写, 有效值: 0~10), BlueFi的Sensors类地磁传感器性能模式, 0:LP, 1:MD, 2:HP, 3:UP
