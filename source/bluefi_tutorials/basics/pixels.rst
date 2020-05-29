流光溢彩的RGB像素灯珠
======================

RGB像素灯珠串是非常有趣的一种输出，不仅可以用于照明和产生彩光，还能输出动态的流光溢彩的光效。

BlueFi的LCD屏幕上方有5颗RGB像素灯珠串，你可以利用他们产生各种有趣的光效，包括进度条、剩余电量、彩虹、流动的光等。整体亮度可编程，
每一个像素的颜色独立可编程。

如果你已经使用BlueFi一段时间了，相信你一定注意过BlueFi的5个RGB像素灯珠，本节我们将掌握如何对这个灯珠串编程让他们产生绚丽的、运动的
光效。

------------------------------------

显示阈值的彩光图案
------------------------------------

oks





.. admonition:: 
  总结：

    - RGB像素灯珠
    - RGB三基色
    - 子类
    - 变量赋值
    - 变量自增/自减
    - 逻辑判断和逻辑程序块
    - 本节中，你总计完成了20行代码的编写工作


.. Important::
  **NeoPixel类的接口**

    - pixels (子类), FlueFi的NeoPixel子类
    - num_pixels (属性, 只读, 有效值：5或更多), FlueFi的RGB像素灯珠的个数
    - brightness (属性, 可读可写, 有效值：0.0～1.0), FlueFi的RGB像素灯珠串的整体亮度
    - clearPixels (函数, 无输入参数, 无返回值), 关闭BlueFi的所有RGB像素灯珠
    - fillPixels (函数, 输入参数：三基色分量的元组, 无返回值), 让BlueFi的所有RGB像素灯珠显示指定的颜色
    - drawPattern (函数, 输入参数:5或更多颗灯珠的颜色列表, 无返回值), 让BlueFi的RGB像素灯珠显示给定的图案
    - shiftRight (函数, 无输入参数, 无返回值), 让BlueFi的RGB像素灯珠显示的图案循环右移一步
    - shiftLeft (函数, 无输入参数, 无返回值), 让BlueFi的RGB像素灯珠显示的图案循环左移一步
    - drawRainbow (函数, 输入参数: 彩虹颜色序号, 无返回值), 让BlueFi显示彩虹图案(第n步), 0 <= n <= 255
    - showAnimation_star 
    - showAnimation_rainbow 
    - showAnimation_comet 
    - showAnimation_wipe 