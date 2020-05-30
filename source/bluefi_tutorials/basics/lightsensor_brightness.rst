计算机如何感知光亮度
======================

为了保护我们眼睛，日常生活中常用的显示器的亮度大多数是可以随着环境光亮度调节。上一节我们实现BlueFi的LCD屏保来节能的案例，
长时间不按下A或B按钮，麦克风也未感知到较大的声音，说明我们在屏幕显示的内容没人关心，我们需要让LCD屏幕自动进入屏保，关闭
背光板这种大能耗设备以节能，当麦克风感知到很大的声音、A或B按钮被按下时，我们自动打开背光板，开启显示。

那么如何做到根据周围环境光亮度调节 BlueFi的LCD屏亮度呢？这个问题首先需要确定BlueFi如何感知环境光亮度。BlueFi的B按钮旁边
有一颗集成的光学传感器，他具备感知环境光亮度的能力。

------------------------

感知环境光亮度
------------------------

我们首先来了解BlueFi的集成光学传感器给出的环境光亮度范围和数值随光亮度变化的趋势。示例程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    sensor = Sensors()
    screen = Screen()
    show_data = screen.simple_text_display(title="BlueFi Text Lines", title_scale=1, text_scale=2)
    while True:
        bv = sensor.color[3]
        show_data[3].text = "Lightness: {}".format(bv)
        show_data.show()
        time.sleep(0.1)

本示例中，我们仍使用上一节用过的Screen类中多行文本显示子类，将Sensors类中的color[3]属性(环境光亮度)值显示在屏幕上，
通过手电筒改变环境光，观察环境光传感器的数值变化，确定其范围(最小值和最大值)，以及数值随光变化的趋势。

示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/sensors.py”模块中导入一个名叫“Sensors”的类
    - 第3行，从“/CIRCUITPY/lib/hiibot_bluefi/screen.py”模块中导入一个名叫“Screen”的类
    - 第4行，将导入的“Sensors”类实例化为一个实体对象，名叫“sensor”
    - 第5行，将导入的“Screen”类实例化为一个实体对象，名叫“screen”
    - 第6行，定义一个名叫“show_data”的Screen类的多行文本显示子类，设置文本显示的标题为“BlueFi Text Lines”，标题字体放大1倍，文本字体放大2倍
    - 第7行，一个无穷循环的程序块
    - 第8行(无穷循环程序块的第1行)，定义一个变量名叫“bv”并将sensor的color属性值的第4个分量(即环境光亮度)赋给该变量
    - 第9行(无穷循环程序块的第2行)，设置多行文本显示子类的第3行的文本内容为变量bv的格式化字符串
    - 第10行(无穷循环程序块的第3行)，更新多行文本显示
    - 第11行(无穷循环程序块的第4行)，执行time的sleep方法，参数为0.1秒

请你把环境光亮度-sensor.color[3]之间的关系列表记录下来。


根据环境光照自动调节屏幕亮度
-------------------------------

下面的示例中，我们将根据环境光照自动调节BlueFi的LCD屏幕亮度，虽然数据映射的方法简单粗暴，但是效果非常明显。示例代码如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import NeoPixel
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    sensor = Sensors()
    screen = Screen()
    pixels = NeoPixel()
    pixels.clearPixels()
    show_data = screen.simple_text_display(title="BlueFi Text Lines", title_scale=1, text_scale=2)
    while True:
        bv = sensor.color[3]
        show_data[3].text = "Lightness: {}".format(bv)
        show_data.show()
        screen.brightness = max(bv/65535, 0.1)
        time.sleep(0.1)

本示例代码是对前一个示例程序稍作修改而来，我们不必再赘述其每一行程序的效果。只是解释关键的第14行程序语句，我们采用一个简单
粗暴的数据映射方法“max(bv/65535, 0.1)”将环境光照数值bv除以65535作为BlueFi的LCD屏幕亮度，如果bv/65535小于0.1则取0.1，
这样可以确保即使环境光亮度非常低，即使为0光照，LCD屏的亮度仍维持最低10%。

我们使用的max函数，执行效果是“取两数中较大的数”，在本示例中，max(bv/65535, 0.1)返回的最小值将是0.1，最大值将是1.0(因为bv的
最大值为65535)。


.. admonition:: 
  总结：

    - LCD显示器
    - LCD显示器背光板的亮度及其调节
    - 多行文本显示的数据结构
    - 文本字体的缩放
    - 数据映射
    - max函数
    - 本节中，你总计完成了14行代码的编写工作

------------------------------------


.. Important::
  **Sensors类的环境光亮度接口**

    - color[3] (属性, 只读, 有效值：0~65535), FlueFi的Sensors类属性, 集成光学传感器的color属性的第4个分量, 即环境光亮度
