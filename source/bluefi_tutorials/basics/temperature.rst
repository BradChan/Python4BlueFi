温度传感器和温度监测
======================

本节中我们将认识两类温度传感器，分别用于监测环境温度和芯片温度。


芯片内部温度
----------------------------

你的电脑电风扇是自动开启和关闭的，计算机系统根据CPU芯片、计算机主板的温度自动控制其风扇的开关。为什么CPU需要风扇来散热？
事实上，所有电子元件在工作时都会发热，只是大多数电子元件的发热量非常小，几乎被我们忽略，因为他们发热的量小于环境散热量，他们所产生
的热量不会积累而不会出现高温。引起电子元件发热的主要因素是他的工作电流和内阻，根据焦耳热定律，电子元件产生的热量与内阻成正比，与
电流的平方成正比。因此所有大功率电子元件都会产生很大的热量。现代高性能处理器由于工作速度很高需要消耗较大电流，自然会产生很多
热量，如果不能即使散热让产生的热量积累起来，将是芯片的温度越来越高，当温度超过芯片能承受的最大温度时，其内部电子单元会被热击穿
而损坏。反过来，低功耗的CPU，意味着工作速度比较低，产生的热量很少，无需风扇。

BlueFi使用的是低功耗的32位处理器，主时钟速度仅仅64MHz，与仅仅是桌面计算机CPU速度的2~5%，所以工作电流极小，处理一般性的I/O
控制事物，BlueFi的主CPU几乎不发热，或者更准确地说其发热量小于环境对其散热量。使用下面的示例程序，我们能够得到主CPU内部的温度。

.. code-block::  python
  :linenos:

    import time
    import microcontroller
    from hiibot_bluefi.screen import Screen
    screen = Screen()
    mcu = microcontroller.cpu
    show_data = screen.simple_text_display(title="BlueFi Main CPU", title_scale=1, text_scale=2)
    while True:
        show_data[2].text = "Frequency: {}MHz".format(mcu.frequency/1000000)
        show_data[2].color = screen.RED
        show_data[3].text = "Voltage: {:.2f}".format(mcu.voltage)
        show_data[3].color = screen.YELLOW
        show_data[4].text = "Temperature: {:.2f}".format(mcu.temperature)
        show_data[4].color = screen.GREEN
        show_data.show()
        time.sleep(0.1)

将上示例程序保存到BlueFi的/CIRCUITPY/code.py，BlueFi将实时地显示主CPU的时钟频率、工作电压、芯片温度。

示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，导入一个Python内建的模块“microcontroller”
    - 第3行，从“/CIRCUITPY/lib/hiibot_bluefi/screen.py”模块中导入一个名叫“Screen”的类
    - 第4行，将导入的“Screen”类实例化为一个实体对象，名叫“screen”
    - 第5行，定义一个名叫“mcu”类变量代表microcontroller模块的cpu子类
    - 第6行，定义一个名叫“show_data”的Screen类的多行文本显示子类，设置文本显示的标题为“BlueFi Gyroscope”，标题字体放大1倍，文本字体放大2倍
    - 第7行，一个无穷循环的程序块
    - 第8行(无穷循环程序块的第1行)，设置多行文本显示子类的第2行的文本内容为mcu.frequency/1000000的值 (主时钟频率)
    - 第9行(无穷循环程序块的第2行)，设置多行文本显示子类的第2行的文本颜色为红色 
    - 第10行(无穷循环程序块的第3行)，设置多行文本显示子类的第3行的文本内容为mcu.voltage的值 (工作电压)
    - 第11行(无穷循环程序块的第4行)，设置多行文本显示子类的第3行的文本颜色为绿色
    - 第12行(无穷循环程序块的第5行)，设置多行文本显示子类的第4行的文本内容为mcu.temperature的值 (芯片温度)
    - 第13行(无穷循环程序块的第6行)，设置多行文本显示子类的第4行的文本颜色为蓝色
    - 第14行(无穷循环程序块的第7行)，更新多行文本显示
    - 第15行(无穷循环程序块的第8行)，执行time的sleep方法，参数为0.1秒

当BlueFi的主CPU工作任务非常繁重时，消耗的电流也会增加，芯片温度也会随之增加。我们使用microcontroller模块的cpu子类能够获取主CPU的温度，
当CPU温度超过某个设定阈值时，我们可以给出警示。


环境温度和温度监视器
----------------------------

BlueFi反面的温度计符号附近有一颗数字型高精度温度传感器，我们可以使用这个传感器监测环境温度，当环境温度超过设定阈值时给予报警或其他处理。
示例程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    from hiibot_bluefi.soundio import SoundOut
    spk = SoundOut()
    spk.enable = 1
    screen = Screen()
    sensors = Sensors()
    temp = sensors.temperature
    maxTemp = 38
    warning = False
    show_data = screen.simple_text_display(title="Temp monitor", title_scale=1, text_scale=3)
    while True:
        temp = sensors.temperature
        show_data[2].text = "T: {:.2f}".format(temp)
        if temp<maxTemp:
            show_data[2].color = screen.GREEN
            warning = False
        else:
            show_data[2].color = screen.RED
            warning = True
        show_data.show()
        if warning:
            spk.play_midi(72, 8)
        time.sleep(0.2)

将本示例代码保存到BlueFi的/CIRCUITPY/code.py文件中，想法改变BlueFi的温度传感器附近的温度，譬如靠近燃烧的
火柴或打火机，温度上升至设定阈值(示例程序中设定为38度)后，你看到多行文本显示的温度变成红色，且伴有急促的声音警示。

本示例程序的具体细节不再详细赘述。

实际应用的警报器的声音提示是可以关闭的，就像闹钟响起后，我们被闹钟叫醒后第一件事就是关闭闹钟的声音提示，当我们发现温度
监视器的警示音响起后，也会先关闭警示音再处理温度超高。你能修改程序实现：当温度超过设定阈值时，将温度显示为红色，并响起警报，
如果按下A按钮则关闭声音警报，直到下次温度再次从低温到高温变化且超过设定阈值时警示音才会再次响起。


-----------------------------

.. admonition:: 
  总结：

    - 温度
    - 芯片温度和环境温度
    - 温度监测与报警
    - 多行文本显示的数据结构
    - 文本字体的缩放
    - 本节中，你总计完成了25行代码的编写工作

------------------------------------

.. Important::
  **Sensors类的温度传感器接口**

    - temperature (属性, 只读, 有效值: 0.0～65.0), BlueFi的Sensors类temperature属性, 当前环境温度, 精度为+/-0.2
  
.. Important::
  **主CPU的低级接口**

    - 
    - temperature (属性, 只读, 有效值: 0.0～85.0)

      - usage: microcontroller.cpu.temperature
