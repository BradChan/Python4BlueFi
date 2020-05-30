使用LCD显示器显示文本
======================

BlueFi正面有一个1.3寸的彩色LCD屏幕，分辨率为240x240点阵，点间距和像素点都很小，这个屏幕几何达到视网膜级别(据说iPhone和iPad
都采用视网膜级别的显示器)，显示文字或图案时非常细腻。在准备阶段我们已经介绍过BlieFi的LCD屏幕的用途，他是我们的控制台，无论任何
时候只要脚本程序遇到错误停止执行时，详细的错误提示信息都会显示在这个屏幕上，方便我们快速排查问题所在，这一功能在执行Python等脚本
程序的计算机相同中尤为重要。

控制台只接受“print()”方法输出的信息和相同的提示信息等，如果用户编程需要使用LCD屏幕显示自己订制的信息，那就需要进一步了解BlueFi
的LCD屏幕的用法。本节仅介绍如何显示简单的文本，虽然只是文本显示，但是颜色、字体大小和位置等都是可编程的。

-----------------------

把文字放大显示
-----------------------

用本节的第一个示例程序来回复“BlueFi的显示文字太小”，当我们把LCD屏当作控制台使用使用时，其显示的相同提示等信息尽可能多，
采用越大的字体意味着整屏能显示的信息就更少，当你嫌控制台信息的字体过小时，那就不用“print()”来show自己的程序输出，参考
本示例的方法，相信可以满足你的“显示大文字”需求。示例程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import Button
    from hiibot_bluefi.soundio import SoundIn
    from hiibot_bluefi.screen import Screen
    button = Button()
    mic = SoundIn()
    screen = Screen()
    bluefi_data = screen.simple_text_display(title="BlueFi LCD", title_scale=2, text_scale=2)
    while True:
        bluefi_data[2].text = "A:{}".format(button.A)
        bluefi_data[3].text = "B:{}".format(button.B)
        bluefi_data[5].text = "SoundIn:{:.1f}".format(mic.sound_level)
        bluefi_data.show()
        time.sleep(0.1)

将本示例程序保存到BlueFi到/CIRCUITPY/code.py文件，执行程序的显示效果与控制台的显示效果做一个对比：

.. image:: /../../_static/images/bluefi_basics/lcd_font_zoom.jpg
  :scale: 40%
  :align: center

本示例程序中，我们仅仅将显示的文字放大2倍



.. admonition:: 
  总结：

    - 声音传感器
    - 声音的形状(记录并绘制声音)
    - 声音采样
    - 声音信号的变化强弱和序列信号的均方差
    - 函数及其定义和调用
    - 全局变量和局部变量
    - 本节中，你总计完成了22行代码的编写工作

------------------------------------


.. Important::
  **Screen类的接口**

    - display (子类), FlueFi的Screen子类
    - width (属性, 只读, 有效值：240), FlueFi的Screen类属性，屏幕宽度(x-)方向的像素个数
    - height (属性, 只读, 有效值：240), FlueFi的Screen类属性，屏幕高度(y-)方向的像素个数
    - loud_sound (函数, 输入参数: 声音很大的阈值(最小值), 返回值: 0或1), FlueFi的麦克风感知到声音信号变换大于设定阈值, 返回1:感知到很大声音，0:未感知到很大声音