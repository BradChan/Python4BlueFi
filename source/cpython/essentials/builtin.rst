=========================
CircuitPython内建库
=========================

CircuitPython的大部分内建库完全兼容标准的Python3，另外也有一些专门针对嵌入式系统应用的内建库是标准Python3所没有的。


---------------------------------------------
流程控制
---------------------------------------------

流程控制是所有计算机编程语言都具备的基础语法，CircuitPython的流程控制包括：

  - if
  - else
  - elif
  - for
  - in
  - while
  - break
  - continue
  - try
  - except
  - return
  - yield

这些基本语法完全兼容标准Python3的流程控制，请查阅Python3的相关参考书掌握他们的用法。


代码块、行缩进和冒号
---------------------------------------------

所有编程语言都有“代码块”的概念，譬如我们判断“条件A”并根据判断结果执行不同的程序块，即“如果条件A成立则执行程序块1，否则执行程序块2”；
譬如重复执行某个代码块若干次，即“当条件A成立则执行代码块”。Python编程语言采用“行缩进”形式代表程序块：隶属于同一个代码块的全部代码
必须具有相同的行缩进。

示例程序：

.. code-block::  python
   :linenos:

    import time
    from hiibot_bluefi.basedio import LED
    led = LED()
    while True:
        led.white = 1
        time.sleep(0.5)
        led.white = 0
        time.sleep(0.5)


本示例有2个层次的代码块组成，其中第5～8行是一个层次的代码块，这个代码块隶属于第4行“while True:”流程控制语句。前4行是一个层次
的代码块，其中第4行的“:”表示其后有一个代码块。

在使用Python编程语言时，“if、else、elif、for、while、try”等流程控制语句的末尾必须使用“:”表示其后有一个隶属于该流程控制语句的代码块，
其后的代码块的所有程序语句必须具有相同的缩进。值得注意的是，代码块是可以嵌套的。

示例程序：


.. code-block::  python
   :linenos:

    import time
    from hiibot_bluefi.basedio import PWMLED
    led = PWMLED()
    b=0
    d=1
    while True:
        led.white = b
        b += 655 if d==1 else -655
        if b>65535:
            b=65535
            d=0
        if b<0:
            b=0
            d=1
        time.sleep(0.01)

本示例有3个层次的4个代码块，除了主程序的代码块之外，“while True:”代码块又包含又两个“if xx:”代码块。


数据和变量
---------------------------------------------

Python支持的数据和变量类型包括：字符/字符串、数组(array)、元组(turple)、列表(list)、字典(dictionary)等基本类型，
我们可以使用‘单引号’、“双引号”、(小括号)、[中括号]和{大括号}等形式组织基本数据和变量，并使用[index]访问第index个数据项。

Python支持的基本数据包括：整数(int)、浮点数(float)、复数(complex)，其中复数由浮点型实部和虚部组成，如a+bj或complex(a,b)的
实部和虚部分别为a和b。

Pthon支持不同数制来表示整数(int)，包括二进制、八进制、十进制(默认的)、十六进制等，并又内建库接口实现不同数制之间的转换。


函数
---------------------------------------------

函数(function)是一种特殊的代码块，允许我们使用函数名调用已定义的函数，调用时可传入若干参数，如果含有有返回值，调用者可以得到
函数的返回值。

示例程序：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import NeoPixel
    from hiibot_bluefi.soundio import SoundIn
    pixels = NeoPixel()
    pixels.brightness = 0.2
    pixels.clearPixels() # black
    delayCnt = 0
    mic = SoundIn(numSamples=8)

    def delayoff(dt):
        global delayCnt
        time.sleep(dt) # 10ms, x100times=1s
        if delayCnt<=0:
            pixels.clearPixels()
        else:
            delayCnt -= 1

    while True:
        delayoff(0.01)
        if mic.loud_sound(200):
            pixels.fillPixels((255,255,255)) # white
            delayCnt = 1000

本示例程序的第10～16行定义一个名为“delayoff”的函数，有一个输入参数“dt”，无返回值。在第19行语句调用该函数时，
输入参数赋值为0.01，意味着执行该函数时“dt=0.01”。

在Python编程语言中，函数定义必须使用“def”关键词，并使用“:”指定函数的代码块。

类和对象
---------------------------------------------

类(class)和对象(object)是所有面向对象编程语言都支持的基本功能，允许编程者将同类的数据信息及其操作封装成类，使用者
将类实例化为具体的对象，进而使用类内的数据及其操作方法。

Python的类封装和实例化的对象与其他编程语言相似，但是使用类之前必须使用“import”导入已封装好的类。根据“import”的规则，
我们可以把Python的一个类看作一个模块。


数学计算
---------------------------------------------

CircuitPython的内建数学计算库——math兼容标准的Python3。使用USB数据线将BlueFi与计算机连接好，你的电脑上将出现
一个名叫“CIRCUITPY”的可卸载磁盘，打开MU编辑器并点击“串口”按钮打开MU控制台，在控制台区域按“ctrl+c”键强制终止BlueFi当前
正在执行的py程序并进入REPL模式，在“>>>”提示符后面输入以下命令：

.. code-block::  python
   :linenos:

    >>> import math
    >>> dir(math)
    ['__class__', '__name__', 'acos', 'asin', 'atan', 'atan2', 'ceil', 
    'copysign', 'cos', 'degrees', 'e', 'exp', 'fabs', 'floor', 'fmod', 
    'frexp', 'isfinite', 'isinf', 'isnan', 'ldexp', 'log', 'modf', 'pi', 
    'pow', 'radians', 'sin', 'sqrt', 'tan', 'trunc']
    >>> 

使用“import math”首先导入CircuitPython内建的“math”库；使用“dir(math)”可以查看内建的math库所支持的全部数学计算方法。

对于其他内建库，我们可以使用同样的方法获得帮助。


其他内建库
---------------------------------------------

CircuitPython到底支持多少种内建库？让BlueFi进入REPL模式，并在“>>>”提示符后输入“help("modules")”即可查看CircuitPython
支持的全部内建库。

.. code-block::  python
   :linenos:

    >>> help("modules")
    __main__          binascii          io                storage
    _bleio            bitbangio         json              struct
    _os               board             math              supervisor
    _pixelbuf         builtins          microcontroller   sys
    _time             busio             micropython       terminalio
    aesio             collections       neopixel_write    time
    analogio          digitalio         os                touchio
    array             displayio         pulseio           ulab
    audiobusio        errno             random            usb_hid
    audiocore         fontio            re                usb_midi
    audiomixer        framebufferio     rgbmatrix         vectorio
    audiomp3          gamepad           rotaryio          watchdog
    audiopwmio        gc                rtc
    Plus any modules on the filesystem
    >>> 

对于某一种内建库，可以使用“import xxx”和“dir(xxx)”分别导入并查看接口方法。

如，想要了解“analogio”——模拟输入和输出库，可以使用以下语句：

.. code-block::  python
   :linenos:

    >>> import analogio
    >>> dir(analogio)
    ['__class__', '__name__', 'AnalogIn', 'AnalogOut']
    >>> help(analogio)
    object <module 'analogio'> is of type module
      __name__ -- analogio
      AnalogIn -- <class 'AnalogIn'>
      AnalogOut -- <class 'AnalogOut'>
    >>> 

可以看到，“analogio”包含2个子类：“AnalogIn”和“AnalogOut”。根据子类的访问方法，进一步地操作：

.. code-block::  python
   :linenos:

    >>> help(analogio.AnalogIn)
    object <class 'AnalogIn'> is of type type
      deinit -- <function>
      __enter__ -- <function>
      __exit__ -- <function>
      value -- <property>
      reference_voltage -- <property>
    >>> help(analogio.AnalogOut)
    object <class 'AnalogOut'> is of type type
      deinit -- <function>
      __enter__ -- <function>
      __exit__ -- <function>
      value -- <property>
    >>> 

子类“AnalogIn”包含有两种属性类方法：value和reference_voltage；子类“AnalogOut”仅包含一个属性“value”。
如何使用呢？请在提示符“>>>”后输入以下命令：

.. code-block::  python
   :linenos:

    >>> import analogio
    >>> import board
    >>> a0 = analogio.AnalogIn(board.P0)
    >>> a0.value
    720
    >>> a0.reference_voltage
    3.3
    >>> a0.value
    37760
    >>> a0.value
    0
    >>>

本示例中，前两个语句分别导入内建库“analogio”和“board”，第3行语句将BlueFi的P0端口定义为模拟输入通道，然后
我们就可以使用“AnalogIn”的“value”和“reference_voltage”属性获取P0端口的值以及参考电压。试着用手指放在P0
触摸盘上并读取这个模拟输入通道的值，观察“value”属性值。被触摸或不被触摸时，这个模拟输入通道的属性值是不同的，
这是为什么？


lambda函数
------------------------------------

使用lambda定义一些函数非常便捷，譬如

.. code-block::  python
   :linenos:

    >>> f = lambda x, y: x**y
    >>> f(2, 3)
    8
    >>> f(8, 3)
    512
    >>> 

定义一个名为“f”的lambda函数计算变量x的y次方。


随机数
------------------------------------

CircuitPython内建的随机数发生器库，用法如下：

.. code-block::  python
   :linenos:

    >>> import random
    >>> random.random()
    0.429787
    >>> random.random()
    0.815612
    >>> random.randint(10, 20)
    11
    >>> random.randint(10, 20)
    20
    >>> 

第1行导入内建库“random”，使用“random.random()”函数随机地生成一个0.0~1.0之间的浮点数；使用“random.randint(10, 20)”函数
随机地生成一个10~20之间的整数，其中20包含其中，整数随机数发生器函数原型为“random.randint(min, max)”。

