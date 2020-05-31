相对湿度和环境监测
======================

人体对环境湿度的要求并不高，很多人不仅能适应我国北方的干燥环境，也能适应南方的潮湿环境。今天的我们已经掌握改变局部环境湿度
的方法，譬如加湿器、抽湿机或空调机等都可以改变局部环境的湿度。所有能改变环境湿度的设备或仪器都需要测量环境湿度，环境湿度的
度量常常使用“相对湿度”，记为RH。为什么不使用绝对湿度呢？相对湿度是如何定义？

相对湿度
----------------------

相对湿度定义为，湿空气中所含水蒸气的质量与同温度和气压下饱和空气中所含水蒸气的质量之比。从定义上看，相对湿度是一个比值，有效
取值范围为0.0～1.0，人类对0~100范围的整数的大小最为敏感，所以我们用百分数来表示相对湿度。如某室内环境的相对湿度为70%。

据实验测定，最宜人的室内温湿度是：冬天温度为20至25℃，相对湿度为30%至80%；夏天温度为23至30℃，相对湿度为30%至60%。
95%以上的人对这样的温度和相对湿度环境感觉为舒适。

相对湿度的定义看起来似乎有点复杂，如何测量环境的相对湿度呢？BlueFi带有一个工业级标准的数字湿度传感器，能够直接给出人们
习惯的环境相对湿度值，精度为+/-2%RH。我们修改前一节的最后一个示例程序，实现一个湿度监视器功能。示例代码如下：

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
    hum = sensors.humidity
    minHum = 30
    maxHum = 70
    warning = False
    show_data = screen.simple_text_display(title="Humi. monitor", title_scale=1, text_scale=3)
    while True:
        hum = sensors.humidity
        show_data[2].text = "H: {:.2f}".format(hum)
        if hum>minHum and hum<maxHum:
            show_data[2].color = screen.GREEN
            warning = False
        else:
            show_data[2].color = screen.RED
            warning = True
        show_data.show()
        if warning:
            spk.play_midi(72, 8)
        time.sleep(0.2)

将本示例代码保存到BlueFi的/CIRCUITPY/code.py文件中，当BlueFi执行示例程序期间，尝试改变BlueFi湿度传感器附近环境的相对湿度，
譬如使用加湿器或者对着湿度传感器哈气，你会发现相对湿度传感器的数值会快速变化。

示例程序的更详细功能不再赘述。


环境温湿度监视器
-----------------------------

到这一节教程，我们已经完全掌握环境温度和相对湿度的测量，利用已经掌握的这些知识，我们来设计一个完整的环境温湿度监视器，实现：
当温度大于设定阈值时，或相对湿度小于设定舒适湿度阈值最小值时，或相对湿度大于设定舒适湿度阈值最大值时，响起警报，并使用红色字体
指示超过设定范围的温度或湿度，在响警报声期间，允许我们按下A按钮关闭警报声，直到下次再报警时才会再次响起。脚本程序代码如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    from hiibot_bluefi.soundio import SoundOut
    from hiibot_bluefi.basedio import Button
    button = Button()
    spk = SoundOut()
    spk.enable = 1
    screen = Screen()
    sensors = Sensors()
    temp = sensors.temperature
    hum = sensors.humidity
    minHum = 30
    maxHum = 50
    maxTemp = 38
    hwarning = False
    twarning = False
    offspk = False
    show_data = screen.simple_text_display(title="T&H Monitor", title_scale=1, text_scale=3)
    while True:
        temp = sensors.temperature
        hum = sensors.humidity
        show_data[1].text = "T: {:.2f}".format(temp)
        if temp<maxTemp:
            show_data[1].color = screen.GREEN
            twarning = False
        else:
            show_data[1].color = screen.RED
            twarning = True
        show_data[2].text = "H: {:.2f}".format(hum)
        if hum>minHum and hum<maxHum:
            show_data[2].color = screen.GREEN
            hwarning = False
        else:
            show_data[2].color = screen.RED
            hwarning = True
        show_data.show()
        if button.A:
            offspk = True
        if temp<maxTemp and hum>minHum and hum<maxHum:
            offspk = False
        if hwarning or twarning:
            if offspk:
                time.sleep(0.1)
            else:
                spk.play_midi(72, 8)
        time.sleep(0.2)

虽然看起来这个程序很长，为了更好理解这个示例程序的细节代码功能，建议你将代码保存到BlueFi的/CIRCUITPY/code.py文件中，
当BlueFi执行示例程序期间，尝试改变BlueFi湿度传感器附近的环境温度或湿度，触发警报，当警报声响起后你可以按下A按钮观察
是否可以消除报警声。

程序的细节功能不再详细赘述。该示例的逻辑功能在前一节教程中已经提到，你曾经有过深入的思考，或许你已经实现了相似的功能。
实现相同或相似功能的脚本代码没有惟一的写法，本示例程序仅供参考。


-----------------------------

.. admonition:: 
  总结：

    - 相对湿度
    - 人体舒适湿度和温度环境
    - 湿度监测与报警
    - 环境温湿度监视器
    - 多行文本显示的数据结构
    - 文本字体的缩放
    - 本节中，你总计完成了47行代码的编写工作

------------------------------------

.. Important::
  **Sensors类的相对湿度传感器接口**

    - humidity (属性, 只读, 有效值: 0.0～100.0), BlueFi的Sensors类humidity属性, 当前环境相对湿度, 精度为+/-2%RH
  