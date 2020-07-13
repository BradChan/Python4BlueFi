=========================
PWM(脉宽调制输出)
=========================

虽然BlueFi不能输出连续变化的模拟信号，无法直接驱动和控制一些模拟接口的电控阀门，对于一些响应速度较高的执行器
我们可以使用PWM信号控制其位置、亮度等。

PWM，即脉冲宽度调制。PWM信号是一种周期固定的、占空比可变的脉冲信号，虽然PWM仍是一种数字信号，可用于LED亮度、
伺服电机位置和转速等控制。使用外部积分电路单元也可以将PWM信号转换为连续可控的电压信号。本向导不涉及电路设计，
关于积分电路请参阅模拟电子电路相关的参考书。

.. image:: /../../_static/images/cpython_essentials/pwmout_1.jpg
  :scale: 50%
  :align: center

如上图所示，脉宽调制信号的占空比是可变的。所谓占空比就是“高电平的宽度与脉冲信号周期之间的比值”。或许你在考虑
“计算机是如何生成PWM信号呢？” 如果你知道定时/计数器已经是现代微控制器标配的功能单元之一，按照下图的示意，相信
你很快就能用HDL(硬件描述语言，这是一种纯粹面向硬件电路设计的计算机编程语言)设计出PWM信号发生器。

.. image:: /../../_static/images/cpython_essentials/pwmout_2.jpg
  :scale: 50%
  :align: center

无论你是否完全明白PWM信号发生器的工作原理，但肯定明白PWM信号的属性：周期/频率、占空比调节等相关的概念。

我们首先来看一个示例以了解改变PWM信号占空比带来的效果：

.. code-block::  python
   :linenos:

    import time
    import board
    import pulseio
    
    led = pulseio.PWMOut(board.WHITELED, frequency=1000, duty_cycle=0)
    
    while True:
        for i in range(100):
            if i < 50:
                led.duty_cycle = int(i * 2 * 65535 / 100)  # Up
            else:
                led.duty_cycle = 65535 - int((i - 50) * 2 * 65535 / 100)  # Down
            time.sleep(0.01)

将该程序保存到BlueFi的CIRCUITPY磁盘code.py文件，当BlueFi执行该程序时，你将会发现一些规律。

本示例程序中，第1行导入“time”模块；第2行导入内建的“board”模块；第3行导入脉冲型输入/输出“pluseio”模块；
第5行定义一个名叫“led”的PWM信号输出对象，硬件引脚为“board.WHITELED”(即BlueFi的白光灯控制引脚，或board.P44)，
该PWM输出信号的频率为1KHz(周期固定为1ms)，初始状态的占空比为0；在无穷循环程序块内，执行一个100次的循环且
循环变量为i。当i小于50时“led”的占空比设置为“int(i*2*65535/100)”，随着i逐步增加这个占空比亦单调递增；
当i不小于50时“led”的占空比设置为“65535-int((i-50)*2*65535/100)”，随着i逐步增加这个占空比亦单调递减。

这个程序的执行效果是，BlueFi的白光LED的亮度“从灭逐渐达最亮，然后从最亮逐渐灭掉”，如此不断地循环。

这个示例程序的第5行语句是本示例的关键：将BlueFi的白光灯控制引脚实例化为PWM信号输出端且PWM频率固定为1KHz。
第9和第11行语句的目的是改变PWM输出信号的占空比，占空比逐渐地变大，然后再逐渐地变小，如此无穷地循环。


.. admonition:: 
  总结：

    - PWM信号输出
    - pulseio
    - PWMOut
    - frequency
    - duty_cycle
    - board

------------------------------------


.. Important::
  **pulseio类的接口**

    - PWMOut(pin, frequency=xx, duty_cycle=0)，将引脚pin实例化为PWM信号输出对象，该对象的接口函数和属性如下：

      - deinit(), 清除实例化的PWM信号输出对象的函数
      - frequency, PWM信号的频率属性, 有效值为0～65535
      - duty_cycle, PWM信号的占空比属性
