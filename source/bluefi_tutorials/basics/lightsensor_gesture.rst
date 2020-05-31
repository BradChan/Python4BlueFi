手势识别
======================

如果你遇到一种机器能识别自己的手势，用手势给机器发指令，机器按照手势指令工作，这想想都是一件很酷的事儿！

BlueFi的集成光学传感器具有手势识别功能，能够识别四个方向的挥手手势：向上、向下、向左、向右。

----------------------------

识别挥手的方向
----------------------------

为了了解手势传感器的识别结果，我们将手势识别的结果用多行文本简单地显示到屏幕上。示例代码如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    sensor = Sensors()
    gesture = sensor.gesture
    screen = Screen()
    show_data = screen.simple_text_display(title="BlueFi Light sensor", title_scale=1, text_scale=2)
    while True:
        gesture = sensor.gesture
        show_data[3].text = "gesture: {}".format(gesture)
        show_data.show()
        time.sleep(0.2)

将上面示例代码保存到BlueFi的/CIRCUITPY/code.py文件中，BlueFi执行程序期间，用你的手在集成光学传感器上方约2公分左右，
并分别向四个挥手，记录不同方向的手势与手势识别结果(0~4)之间的关系。


用手势向计算机发指令
----------------------------

这个示例中，我们用四种手势告知BlueFi播放不同的音调，让自己临空弹奏音乐。示例代码如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.soundio import SoundOut
    from hiibot_bluefi.sensors import Sensors
    from hiibot_bluefi.screen import Screen
    spk = SoundOut()
    spk.enable = 1
    sensor = Sensors()
    gesture = sensor.gesture
    screen = Screen()
    show_data = screen.simple_text_display(title="BlueFi Light sensor", title_scale=1, text_scale=2)
    while True:
        gesture = sensor.gesture
        show_data[3].text = "gesture: {}".format(gesture)
        show_data.show()
        if gesture==1:
            spk.play_midi(86, 1/2)
        elif gesture==2:
            spk.play_midi(84, 1/2)
        elif gesture==3:
            spk.play_midi(88, 1/2)
        elif gesture==4:
            spk.play_midi(89, 1/2)
        else:
            time.sleep(0.1)

这个示例程序的关键语句是“while True:”循环程序块内的嵌套逻辑，根据识别的手势结果播放对应的midi音节。具体程序代码的功能不再详细赘述。


-----------------------------

.. admonition:: 
  总结：

    - 手势指令
    - 手势识别和光学传感器
    - 多行文本显示的数据结构
    - 文本字体的缩放
    - 逻辑嵌套
    - 本节中，你总计完成了24行代码的编写工作

------------------------------------

.. Important::
  **Sensors类的手势传感器接口**

    - gesture (属性, 只读, 有效值：0, 1, 2, 3, 4), BlueFi的Sensors类gesture属性, 集成光学传感器的手势识别结果

      - 0: 未是被到任何手势
      - 1: 向上挥手(从B按钮向光学传感器方向)
      - 2: 向下挥手
      - 3: 向左挥手(从光学传感器向LCD屏幕方向)
      - 4: 向右挥手