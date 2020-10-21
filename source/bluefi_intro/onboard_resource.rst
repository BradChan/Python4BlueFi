丰富的I/O资源
====================

单板机与CPU模块的本质区别：单板机拥有丰富的I/O资源。BlueFi不仅具有同时可用的BlueTooth5和WiFi无线通讯接口和强大的计算能力，
板载丰富的I/O资源满足大多数IoT应用场景的需求。

1. BlueFi板载资源的布局
-------------------------

BlueFi板正面的资源布局，如下图：

.. image:: /../_static/images/bluefi_intro/Bluefi_Resource1.jpg
  :scale: 10%
  :align: center

BlueFi板背面的资源布局，如下图：

.. image:: /../_static/images/bluefi_intro/Bluefi_Resource2.jpg
  :scale: 10%
  :align: center


2. BlueFi的40-Pin金手指拓展接口
----------------------------------

虽然兼容microbit硬件接口，全新设计的BlueFi拥有更丰富的I/O资源，两者之间的资源对比如下图：

.. image:: /../_static/images/bluefi_intro/BlueFivsMicribot.jpg
  :scale: 10%
  :align: center

microbit采用40-Pin金手指型拓展接口，包含19个可编程I/O引脚(注意，其中部分引脚与板载资源共享，不能同时使用这些引脚实现拓展和
板载功能)，以及3.3V电源和地。microbit的拓展接口如下图：

.. image:: /../_static/images/bluefi_intro/microbit.jpg
  :scale: 10%
  :align: center

BlueFi的40-Pin金手指型拓展接口的每一个引脚的功能完全兼容microbit，但用法更灵活，且不与板载资源共享引脚。换句话说，BlueFi的40-Pin拓展引脚
全部可以独立编程用于拓展外部资源，同时保持板载资源照常使用。此外，BlueFi的40-Pin拓展接口上的19个引脚可编程作为DI/DO/PWM/I2C/I2S/SPI/UART等功能使用，
比microbit的原始接口更加灵活。BlueFi的40-Pin金手指拓展接口如下图：

.. image:: /../_static/images/bluefi_intro/BlueFi_board_IF.jpg
  :scale: 10%
  :align: center


3. mini-Grove接口
---------------------------

Gorve最初由矽递科技(Seeed)定义的4-Pin拓展接口，使用2.0mm间距的防呆连接器，Grove接口的机械、电气接口标准被全球开源社区广泛使用。
Grove接口电气信号排列非常科学，即使使用最廉价的2.54mm间距排针或排母当作连接器，偶尔将插座/插头反接也不会引起电气损坏，只是造成接
口功能失效。BlueFi带有一个Grove接口，并默认将两个信号线分别定义为I2C的SCL(P19)和SDA(P20)。为节约PCB空间，BlueFi采用1mm间距
的4-Pin防呆连接器，因此被称作mini-Grove。这个的机械和电气标准完全兼容国外开源社区的STEMMA QT和Qwiic两种接口，知名的开源硬件供
应商——Adafruit和SparkFun等都提供多种扩展模块。尤其，BlueFi的moni-Grove接口与STEMMA QT、Qwiic两种接口一致，仅支持I2C接口。

.. image:: /../_static/images/bluefi_intro/mini-grove.jpg
  :scale: 10%
  :align: center

I2C接口标准虽然已经诞生数十年(最初的接口标准由Philips公司所定义)，目前仍焕发青春活力！今天所有的计算机系统，几乎都采用或支持I2C。
正如"Inter-Integrated Circuit bus"其名字，I2C非常适合计算机系统内部各功能部件之间互联，如电池供电单元的控制接口等。I2C作为一种
bus接口，其协议定义允许多个I2C接口设备连接到一个I2C bus上仍能实现半双工的主从通讯，理论上一个I2C bus可以挂接127个从设备。

目前，I2C接口的通讯速度有两种：100KHz和400Khz，由I2C接口设备的硬件和软件确定。BlueFi的mini-Grove接口支持两种速度的接口，使用时
由软件来定义具体执行的通讯速度。


