=========================
digitalio库
=========================

数字型输入/输出(digital in/out)是所有嵌入式计算机系统具有的基本功能，按钮开关是典型的数字型输入，LED指示灯的亮/灭控制是
典型的数字型输出。虽然BlueFi的basedio.py库已经将BlueFi上所有的数字型输入/输出进行了封装，将basedio的子类实例化之后，
你就可以直接使用其中的接口方法访问BlueFi的所有数字型输入/输出资源。但是，BlueFi的40扩展接口上具有19个可编程的引脚，如何
将这些引脚用于数字型输入/输出呢？

首先看下面的示例程序：

.. code-block::  python
   :linenos:

    import time
    import board
    from digitalio import DigitalInOut, Direction, Pull
    
    led = DigitalInOut(board.P44)      # define "led" object (white LED)
    led.direction = Direction.OUTPUT   # set its direction to "out"
    button = DigitalInOut(board.P5)    # define "button" object (button A)
    button.direction = Direction.INPUT # set its direction to "in"
    button.pull = Pull.DOWN            # set its "pull" property into "down"

    while True:
        # We could also do "led.value = not button.value"!
        if button.value:
            led.value = False
        else:
            led.value = True
        time.sleep(0.01)

将该程序保存到BlueFi的CIRCUITPY磁盘code.py文件，当BlueFi执行该程序时，按下按钮A，释放按钮A，你将会发现一些规律。

本示例程序中，第1行导入“time”模块；第2行导入内建的“board”模块；第3行语句从“digitalio”模块中导入“DigitalInOut”、
“Direction”和“Pull”三个子类；第5行定义一个名叫“led”的数字型输入/输出对象，硬件引脚为“board.P44”，即BlueFi的白光灯
的控制引脚；第6行语句将对象“led”的方向属性“led.direction”设置为输出，即“Direction.OUTPUT”；第7行定义一个数字型输入/输出
对象“button”；第8行语句将对象“button”的方向属性“button.direction”设置为输入，即“Direction.INPUT”；第9行语句将
对象“button”的上拉/下拉电阻属性设置为下拉，即“Pull.DOWN”；在无穷循环的程序块内，我们根据数字型输入对象“button”的值
属性“value”确定数字型输出对象“led”的值。

通过本示例，我们使用Python语言轻松地实现BlueFi的数字型输入侦测和输出控制。

内建库“board”定义BlueFi的所有硬件资源，譬如按钮A使用“board.P5”引脚，白光LED灯使用“board.P44”引脚。你可以在
MU编辑器中，点击“串口”按钮打开控制台，并在控制台区按下“ctrl+c”键，强制终止BlueFi当前正在执行程序，并进入REPL模式，
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

BlueFi的全部硬件接口资源都已经列举在这里。


---------------------------------

数字型输入的电路结构
---------------------------------

现代的嵌入式系统微控制器不仅具有丰富的I/O资源，这些资源的输入和输出可以配置到任意I/O引脚，近些年把这种结构
的I/O引脚称作“现场可编程I/O”。BlueFi的主控制器——nRF52840的大多数I/O资源都支持现场可编程，但模拟输入单元
所用的引脚是固定的。本向导中我们只关心数字型输入/输出，其他I/O资源的用法在后续的向导中陆续呈现。

数字型输入仅有两个状态：高电平和低电平。以按钮为例，按钮仅有两个状态：释放状态和按下状态。因此，数字型输入有两种
电路结构，我们仍以按钮为例，如下图所示：

.. image:: /../../_static/images/cpython_essentials/digitalin_1.jpg
  :scale: 40%
  :align: center

BlueFi的按钮A和B使用上图的第2种结构，该电路结构中按钮被按下时P0引脚与电源VDD接通，即P0的值为True(高电平状态)，
当按钮释放时必须使用下拉电阻确保P0的值为False(低电平状态)。对照上面的示例程序的第9行语句“button.pull = Pull.DOWN”，
现在我们完全明白为啥使用下拉电阻属性“Pull.DOWN”。

上图的第1种数字型输入结构(左图)中，当按钮被按下时P引脚与GND接通，即P0的值为False(低电平)，当按钮释放时必须使用上拉电阻
确保P0的值为True(高电平状态)。

嵌入式计算机编程不同于纯软件编程，有些时候需要你了解一点硬件知识。


数字型输出的电路结构
---------------------------------

虽然上面示例中我们并没有明确指定数字型输出对象“led”的具体驱动模式，这是因为BlueFi的全部数字输出引脚默认采用推挽模式。
什么是推挽模式？什么是开漏极模式？如下图所示：

.. image:: /../../_static/images/cpython_essentials/digitalout_1.jpg
  :scale: 40%
  :align: center

推挽模式的微控制器内部采用2个MOS管(分别称作上臂和下臂)来驱动输出引脚，如上左图所示，上臂采用P-MOS、下臂采用N-MOS。
当内部输出为True时，经过反相器后的输出信号(False)让上臂的P-MOS导通且下臂的N-MOS截止，此时输出引脚P0与VDD接通，
外部LED阳极和阴极两端存在电势差形成的电流确保LED灯亮；当内部输出为False时，反相的信号(True)让下臂N-MOS导通且上臂
P-MOS截止，此时输出引脚P0与GND之间无电势差使得LED阳极和阴极之间无电流，LED灯灭。

BlueFi的红色LED和白色LED指示灯都采用推挽驱动模式。

再看开漏极驱动模式，如上右图，微控制器内部仅有下臂N-MOS驱动P0引脚，由于这种驱动电路的N-MOS内部偏置电压，外部电路必须
提供N-MOS导通所需要的偏置电压，如果用P0以开漏极模式控制LED指示灯，LED阳极端必须有电源提供偏置。

到底选择哪种输出驱动模式？根据负载的工作电压范围选择输出驱动模式。

嵌入式计算机系统的数字型输出控制对象远不止于LED指示灯，如继电器、电磁铁、电磁阀、直流电机等负载的工作电压范围都远超微控制
器的工作电压——3.3V，此类负载只能选择开漏极驱动模式。对于工作电压不大于微控制器的I/O工作电压的负载，我们可以选择使用
推挽驱动模式，但是微控制器引脚的驱动电流非常有限(一般不会超过20mA)，当负载电流超出微控制器引脚的电流时必须采用外部负载驱动。



.. admonition:: 
  总结：

    - 数字型输入/输出
    - digitalio
    - DigitalInOut
    - Direction
    - Pull
    - DriveMode
    - board
    - 输入电路结构
    - 输出驱动模式

------------------------------------


.. Important::
  **digitalio类的接口**

    - DigitalInOut(pin)，将引脚pin实例化为数字型输入/输出对象，该对象的接口函数和属性如下：

      - switch_to_output(), 切换为数字型输出的函数
      - switch_to_input(pull=x), 切换为数字型输入的函数, pull参数：digitalio.Pull.DOWN, 或digitalio.Pull.UP
      - direction, 方向属性, 有效值为digitalio.Direction.OUTPUT, 或digitalio.Direction.INPUT
      - value, 值属性, 有效值为True, 或False
      - drive_mode, 输出驱动模式属性, 有效值为digitalio.DriveMode.PUSH_PULL, 或digitalio.DriveMode.OPEN_DRAIN
      - pull, 上拉/下拉电阻的属性, 有效值为digitalio.Pull.UP, 或digitalio.Pull.DOWN
    
  - Direction, 数字型输入/输出的方向常数

      - INPUT, 输入方向
      - OUTPUT, 输出方向
  
  - Pull, 数字输入型引脚的上拉/下拉电阻属性常数

      - UP, 上拉电阻
      - DOWN, 下拉电阻
  
  - DriveMode, 数字输出型引脚的驱动模式属性的常数

      - PUSH_PULL, 推挽模式
      - OPEN_DRAIN, 开漏极模式

