“请保持距离”——接近感知
======================

2020年初爆发的“新冠肺炎(COVID-19)”疫情，全球逾600万人感染此病，近40万人因被感染而罹难。从此我们不仅佩戴口罩出行，遇到任何陌生人时都
刻意保持距离，人与人之间近距离接触是此疫情蔓延的主要传播途径。为了防止陌生人考自己太近，我们需要给自己的衣服上增加一些装置，检测与他人的
距离，当我们的距离过近时，此装置能给我们报警提醒大家“请保持距离”。

BlueFi的集成光学传感器能够实现这一功能。

----------------------------------

接近感知的(光学)传感器
----------------------------------

这一节的第一个示例仍然是采用Screen类中多行文本显示子类将接近传感器的数值显示在屏幕上，我们先观察手与传感器之间距离和接近传感器输出的数值
之间关系。示例代码如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    sensor = Sensors()
    screen = Screen()
    show_data = screen.simple_text_display(title="BlueFi Light sensor", title_scale=1, text_scale=2)
    while True:
        pv = sensor.proximity
        show_data[3].text = "Proximity: {}".format(pv)
        show_data.show()
        time.sleep(0.1)

将本示例程序保存到BlueFi的/CIRCUITPY/code.py文件，BlueFi在运行程序期间，用手慢慢靠近B按钮旁边的集成光学传感器，观察

示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/sensors.py”模块中导入一个名叫“Sensors”的类
    - 第3行，从“/CIRCUITPY/lib/hiibot_bluefi/screen.py”模块中导入一个名叫“Screen”的类
    - 第4行，将导入的“Sensors”类实例化为一个实体对象，名叫“sensor”
    - 第5行，将导入的“Screen”类实例化为一个实体对象，名叫“screen”
    - 第6行，定义一个名叫“show_data”的Screen类的多行文本显示子类，设置文本显示的标题为“BlueFi Light sensor”，标题字体放大1倍，文本字体放大2倍
    - 第7行，一个无穷循环的程序块
    - 第8行(无穷循环程序块的第1行)，定义一个变量名叫“pv”并将sensor的proximity属性值(即接近度)赋给该变量
    - 第9行(无穷循环程序块的第2行)，设置多行文本显示子类的第3行的文本内容为变量pv的格式化字符串
    - 第10行(无穷循环程序块的第3行)，更新多行文本显示
    - 第11行(无穷循环程序块的第4行)，执行time的sleep方法，参数为0.1秒

请你把手与传感器之间距离-sensor.proximity(传感器感知到的接近度)之间的关系列表记录下来。从记录数据明显可以看出，sensor.proximity变化
和手-传感器之间距离几乎没有线性关系。换句话数，BlueFi的集成光学传感器中的接近度感知并不等同于测距传感器，譬如飞秒激光测距、超声波测距、红外
测距等传感器都能给出目标物体和传感器之间的准确距离。请你将自己记录的数据和手-传感器之间的距离关系绘制成一张图，我们就能明显地看出他们不是线性
关系。但是，我们仍能确定一个基本的规律：目标物体与传感器的距离越近，sensor.proximity的值越大，最大值是255。

事实上，接近度传感器的正确用法是，设置一个接近度阈值，当接近传感器输出的数值大于设定的阈值时，代表目标物体与传感器的距离过小，产生警报或提示
信息即可。


请保持距离
----------------------------------

下面我们使用设置阈值，当接近度大于设定阈值时，屏幕上显示“Keep the distance”信息提示，5颗RGB像素灯珠显示红色，并用喇叭发出警告提示音。
示例代码如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import NeoPixel
    from hiibot_bluefi.soundio import SoundOut
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    sensor = Sensors()
    screen = Screen()
    spk = SoundOut()
    spk.enable = 1
    pixels = NeoPixel()
    pixels.brightness = 0.1
    pixels.clearPixels()
    show_data = screen.simple_text_display(title="BlueFi Light sensor", title_scale=1, text_scale=2)
    while True:
        pv = sensor.proximity
        if pv<1:
            show_data[3].text = ""
            pixels.clearPixels()
        else:
            show_data[3].text = "Keep the distance!"
            show_data[3].color = screen.RED
            pixels.fillPixels((255,0,0))
            spk.play_midi(60, 16)
        show_data.show()
        time.sleep(0.1)

为了更好理解本示例程序，我们将示例程序保存到BlueFi的/CIRCUITPY/code.py文件中，BlueFi执行示例程序期间，我们用手靠近BlueFi的集成光学
传感器，你会听到急促的提示音，5颗全红色的警示灯亮起，LCD屏幕上也有相应的红色文字提示“Keep the distance!”。效果如下图：

.. image:: /../../_static/images/bluefi_basics/sensor_proximity.gif
  :scale: 20%
  :align: center

实现本示例功能的关键程序语句是“while True:”循环程序块内的条件判断，如果sensor.proximity接近度值小于1(即等于0)则清除多行文本显示的第3行
文本内容，并关闭5颗RGB像素灯珠；否则，说明有物体接近，则让多行文本的第3行显示“Keep the distance!”且文本颜色为红色，并让5颗RGB像素灯珠显示
全红色，同时让喇叭播放第60号midi音调，并持续1/16拍。

其他程序语句都是前一个示例修改而来，不必详细赘述具体功能。

至此，你是否已经能设计一件“独特的衣服”，穿着这件衣服出门，遇到任何与你距离过近的人，都会发出声、光警示，提醒你们保持距离。

-----------------------------

.. admonition:: 
  总结：

    - 接近感知和接近传感器
    - 接近度
    - 多行文本显示的数据结构
    - 文本字体的缩放
    - 穿戴设备和防疫措施
    - 本节中，你总计完成了25行代码的编写工作

------------------------------------

.. Important::
  **Sensors类的接近度接口**

    - proximity (属性, 只读, 有效值：0~255), FlueFi的Sensors类属性, 集成光学传感器的proximity属性, 即接近度
