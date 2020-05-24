====================
使用microbit周边
====================

microbit单板机以其简单易用为特色，支持图形化语言的在线编程环境——makecode(源自Microsoft的PXT开源项目)，Python脚本语言(MU编辑器)等。
全球很多开源社区都有丰富的microbit周边资源，包括各种扩展板、小车底盘、机器人功能板等硬件资源。全新设计的BlueFi在计算性能和网络连通性等
方面远超micrbit，但仍保留microbit硬件接口，目的是借助于丰富的microbit周边快速搭建高性能的AI边缘计算应用原型。

.. image:: /../_static/images/Bluefi_appexample1.jpg
  :scale: 10%
  :align: center

.. image:: /../_static/images/Bluefi_appexample2.jpg
  :scale: 10%
  :align: center

由于microbit的40-Pin金手指拓展接口上定义了19个可编程I/O引脚，每一个引脚都可以定义为DI/DO/PWM等模式，部分引脚具有模拟输入功能，部分引脚
还可以用作I2C/SPI/UART通讯接口，使用microbit专用的40-Pin拓展插槽连接器，利用这些引脚可以实现遥控手柄、彩灯条/阵列、传感器、按钮、喇叭
等I/O扩展板。据不完全统计，全球microbit周边多达300+种。

.. image:: /../_static/images/microbit-shield.jpg
  :scale: 10%
  :align: center

1. 拔掉microbit直接插入BlueFi？
--------------------------------

这完全是允许的，不会引起任何风险。BlueFi的40-Pin金手指拓展接口的设计，机械接和电气接口完全兼容microbit。


2. BlueFi直接支持哪些microbit扩展板？
-------------------------------------

支持任何适合于microbit的扩展板。


3. BlueFi支持makecode？
-------------------------------------

目前不支持。欢迎熟悉Microsoft开源PXT项目的开发者参与软件移植工作。

BlueFi不支持makecode环境，意味着你不能在makecode环境直接使用BlueFi+扩展板。


4. BlueFi支持哪些编程环境使用microbit周边？
--------------------------------------------

Scratch、Python、Arduino IDE等编程环境支持所有micribit周边。


5. 使用BlueFi+microbit周边需要了解哪些编程知识?
-----------------------------------------------

microbit周边与microbit接口采用DI/DO、AI、PWM(含Servo)、I2C、SPI和UART等，你需要根据具体的microbit拓展板接口以及
这些相关基础知识来完成对扩展板资源的编程控制。目前BlueFi支持的Scratch、Python和Arduino IDE等编程环境都已具备这些功能
接口的开源库，帮助你轻松实现对microbit扩展板的编程控制。


