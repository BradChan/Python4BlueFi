====================
全面了解BlueFi
====================

从这里开始，我们将深入了解BlueFi的细节和板载资源情况，考虑到未来的编程应用目的，每一个板载资源所占用的MCU资源、引脚及其别名
都将一一列出，尤其你打算自己编写BlueFi的应用接口时，本向导是非常重要的。

BlueFi是一种掌上型mini计算机(或单板机)。按照计算机的基本模型：CPU+MEM+IO。我们将按照这个计算机模型的组成单元为线索来
“全面了解BlueFi的细节”。

-----------------------

用默认示例体验BlueFi
-----------------------

当你初次遇到BlueFi时，了解BlueFi的最佳姿势是使用USB数据线给BlueFi通电，然后观察BlueFi执行默认示例程序的效果，并用手触摸
P0/P1/P2触摸盘或按压A/B按钮或将手靠近B按钮上方的集成光学传感器，让BlueFi听音乐，你会发现BlueFi能够与你互动，你有动作时BlueFi
或发出声响或改变灯光颜色或屏幕开启或有数值不停地变化。

.. image:: /../_static/images/know_bluefi/defualt_example.gif
  :scale: 60%
  :align: center

体验BlueFi时的感觉与体验桌面型计算机完全不同。BlueFi没有鼠标和键盘，只有触摸盘、小按钮和接近传感器；BlueFi没有大屏显示器，
只有1.3寸视网膜级彩色LCD屏，但显示的图片确很细腻；BlueFi还有能够随着音乐节奏跳动的彩光；BlueFi还能发出和弦般声音；..

初步体验的感觉：

  - 图片显示十分地细腻
  - 彩光能随节奏敏捷地跳动
  - 触摸输入、按钮输入、接近输入, ..
  - 声音输出
  - 闪烁的红色LED

BlueFi远不止这些，想要深入了解BlueFi的丰富资源，请仔细阅读本向导。


BlueFi的主CPU
-----------------------

众所周知，ARM Cortex-M内核，尤其带有专用浮点计算硬件单元的内核，今天已经被广泛应用到各个领域，从家电到汽车、安防到机器人控制等
领域的产品中几乎都有该内核在使用，如果你打算了解嵌入式计算机系统的原理、掌握计算机编程或入门AI边缘计算，BlueFi将是你最佳的选择
之一。BlueFi的主CPU使用ARM Cortex-M4F内核，使得BlueFi具有高性能的算力来满足AI应用的计算需求。BlueFi的主CPU内置BlueTooth5
网络接口，以及专用的硬件加密和解密引擎单元，确保你的产品安全地使用网络而不受Hacker攻击。

BlueFi主CPU采用Nordic半导体的nRF52840



BlueFi的网络协处理器
-----------------------



BlueFi的存储器组织
-----------------------



BlueFi的传感器输入
-----------------------



BlueFi的控制输出
-----------------------



BlueFi的扩展接口
-----------------------


Scratch图形化语言
