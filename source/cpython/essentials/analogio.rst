=========================
analogio(模拟输入/输出)
=========================

连续的模拟信号输入/输出(analog in/out)是大多数嵌入式计算机系统的基本接口功能。

加热器的温度、自来水管道内的水压等都是连续变化的物理量，这些连续变化的物理量必须使用传感器感知物理变化并转换为连续的
电压或电流变化，我们的计算机系统只能接受离散的二进制数据，因此必须借助于“能够将连续的电压或电流转换为离散的数值信号”
的专用接口器件——ADC，即模拟-数字转换器，将模拟信号转换为计算机认识的数值信号。

电控阀门能够通过计算机控制其开度而控制管道内煤气或自来水的流量、流速等，这类电控阀门的控制需要计算机输出连续的模拟信号。
计算机不能直接输出连续的模拟信号，必须借助于“数字-模拟转换器件”，即DAC。

ADC和DAC都属于计算机系统的硬件功能单元，现代的大多数嵌入式系统的微控制器都带有这样的硬件功能单元，但不是必备的。
BlueFi的主控制器——nRF52840微控制器内部带有一个12位分辨率的、200K采样率的、8个输入通道的ADC功能单元，但没有DAC单元。

通过本向导的学习你将会掌握使用Python语言编程将外部世界的连续模拟信号转换为数字信号。

首先看下面的示例程序：

.. code-block::  python
   :linenos:

    import time
    import board
    from analogio import AnalogIn

    analog_in = AnalogIn(board.A0)

    def getVoltage(pin):
        return (pin.value * pin.reference_voltage) / 65536

    while True:
        print( (getVoltage(analog_in), ) )
        time.sleep(0.2)

将该程序保存到BlueFi的CIRCUITPY磁盘code.py文件，当BlueFi执行该程序时，点击MU编辑器的“串口”和“绘图仪”按钮并打开控制台
和绘图仪窗口，然后用手指触摸P0触摸盘，你将会发现一些规律。

本示例程序中，第1行导入“time”模块；第2行导入内建的“board”模块；第3行语句从“analogio”模块中导入“AnalogIn”子类；
第5行定义一个名叫“analog_in”的模拟输入对象，硬件引脚为“board.A0”，即BlueFi的40P扩展接口的P0触摸盘，这是BlueFi的ADC
的输入通道0；第7和第8行定义一个名为“getVoltage”函数，将指定模拟输入信号转换为模拟电压值并返回，这里的“pin.value”和
“pin.reference_voltage”分别为AD转换结果的数值和ADC的参考电压；在无穷循环程序块内调用函数“getVoltage(analog_in)”
并将“analog_in”作为输入参数传给该函数。

通过本示例，我们使用Python语言轻松地将连续的模拟信号转换为数值信号。如果你熟悉“print()”函数，一定能明白控制台中显示的
数值的意义，以及绘图仪所显示的图线与手指触摸P0触摸盘的状态之间的关系。

内建库“board”定义BlueFi的所有硬件资源，譬如本示例中用到的“board.A0”引脚。想知道BlueFi到底有多少个引脚支持模拟输入？
你可以在MU编辑器中，点击“串口”按钮打开控制台，并在控制台区按下“ctrl+c”键，强制终止BlueFi当前正在执行程序，并进入REPL模式，
使用以下命令获取“board”内建库的详细信息：

.. code-block::  python
   :linenos:
  
    >>> import board
    >>> dir(board)
    ['__class__', 'A0', 'A1', 'A2', 'A3', 'A4', 'A5', 'A6', 
    'ACCELEROMETER_INTERRUPT', 'AUDIO', 'BUTTON_A', 'BUTTON_B', 
    'D0', 'D1', 'D10', 'D11', 'D12', 'D13', 'D14', 'D15', 'D16', 
    'D17', 'D18', 'D19', 'D2', 'D20', 'D21', 'D22', 'D23', 'D24', 
    'D25', 'D26', 'D27', 'D3', 'D34', 'D35', 'D36', 'D37', 'D38', 
    'D39', 'D4', 'D40', 'D41', 'D42', 'D43', 'D44', 'D45', 'D46', 
    'D5', 'D6', 'D7', 'D8', 'D9', 'DISPLAY', 'I2C', 'IMU_IRQ', 
    'MICROPHONE_CLOCK', 'MICROPHONE_DATA', 'MISO', 'MOSI', 
    'NEOPIXEL', 'P0', 'P1', 'P10', 'P11', 'P12', 'P13', 'P14', 
    'P15', 'P16', 'P17', 'P18', 'P19', 'P2', 'P20', 'P21', 'P22', 
    'P23', 'P24', 'P25', 'P26', 'P27', 'P3', 'P34', 'P35', 'P36', 
    'P37', 'P38', 'P39', 'P4', 'P40', 'P41', 'P42', 'P43', 'P44', 
    'P45', 'P46', 'P5', 'P6', 'P7', 'P8', 'P9', 'REDLED', 'RX', 
    'SCK', 'SCL', 'SDA', 'SENSORS_SCL', 'SENSORS_SDA', 'SPEAKER', 
    'SPEAKER_ENABLE', 'SPI', 'TFT_BACKLIGHT', 'TFT_CS', 'TFT_DC', 
    'TFT_MOSI', 'TFT_RESET', 'TFT_SCK', 'TX', 'UART', 'WHITELED', 
    'WIFI_BUSY', 'WIFI_CS', 'WIFI_MISO', 'WIFI_MOSI', 'WIFI_PWR', 
    'WIFI_RESET', 'WIFI_SCK']
    >>> 

可以看出，A0~A6共7个模拟输入，这说明BlueFi支持7个模拟输入通道。

你看到下图这样的绘图仪效果？

.. image:: /../../_static/images/cpython_essentials/analogin_plotter.jpg
  :scale: 40%
  :align: center

注意，MU编辑器的绘图仪同时可以绘制多个数据的变化曲线，如何区分这些数据呢？将待绘制图线的数据项以元组
的形式打印到控制台和绘图仪，绘图仪同时自动地绘制不同颜色的图线分别代表元组的一项。本示例中仅有一个
P0的电压量，第11行语句的写法不仅能够将电压值以文本形式输入到控制台，也可以由绘图仪绘制成折线图，
更直观地展示P0的电压变化。


.. admonition:: 
  总结：

    - 连续的模拟信号输入/输出
    - analogio
    - AnalogIn
    - A0~A6
    - value
    - reference_voltage
    - board
    - 元组
    - 绘图仪

------------------------------------


.. Important::
  **analogio类的接口**

    - AnalogIn(pin)，将引脚pin实例化为模拟输入对象，该对象的接口函数和属性如下：

      - deinit(), 清除实例化的模拟输入对象的函数
      - value, 值属性, 有效值为0～65535
      - reference_voltage, ADC的基准电压属性, 只读的，BlueFi的ADC基准电压固定为3.3V
