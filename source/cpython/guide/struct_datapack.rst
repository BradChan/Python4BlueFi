=====================================
struct和array: 处理C语言类型数据的模块
=====================================

熟悉C/C++编程语言的程序员在处理网络(如串口、RS485、CAN、Ethernet、WiFi等)数据流(data-stream)时异常地从容，
由于C/C++编程语言的基础数据类型很容易处理格式化的数据，当使用Python编程语言处理结构化的数据流时就显得费事。

Python解释器内置一个专门处理C型数据的模块——struct，使用USB数据线将BlueFi连接到电脑，然后打开MU编辑器的控制台，
强制让BlueFi进入REPL模式，然后输入命令行导入“struct”模块，并查看该模块支持的接口，如下：

.. code-block::  python
   :linenos:

    >>> import struct
    >>> help(struct)
    object <module 'struct'> is of type module
      __name__ -- struct
      calcsize -- <function>
      pack -- <function>
      pack_into -- <function>
      unpack -- <function>
      unpack_from -- <function>

总计5个接口函数，即“2对打包和解包”的4接口和1个“struct.calcsize(fmt)”接口。这些接口中，
“struct.calcsize(fmt)”接口返回一种特定打包格式所占用的内存字节个数；“pack(fmt, *value)”和“unpack(fmt, databuf)”是一对打包和解包接口，
前者将Python的一个或多个数据(如整数、浮点数及其列表或元组)按指定打包格式返回bytearray型databuf(即格式化的数据流)，
后者则将bytearray型databuf(即格式化的数据流)按照特定打包格式解包并返回一个元组型Python数据(该元组内各项即格式化的数据流中各数据单元)；
“pack_into(fmt, databuf, offset, *value)”和“unpack_from(fmt, databuf, offset)”是一对带有位置偏移参数的打包和解包接口，
前者是无返回值的，按指定的打包格式将value打包并从databuf偏移offset位置开始存放格式化的数据，
后者则按指定的解包格式将databuf从偏移offset位置开始解包并返回元组型Python数据。

上面5个接口函数的解释读起来比较生硬，而且也不能跟实际的应用联系起来。我们知道，C/C++中有uint8_t、uint16_t、uint32_t、float等基本数据类型，
但Python语言中仅有integer、float、bool等三种基本数据类型，在网络数据流中的数据信息单元都是由若干个位或若干个字节组成。
譬如1个字节的整数，在C/C++中直接用uint8_t即可，但Python语言却没有直接的方法，struct模块的接口正是为了解决Python的这个问题。

举个例子，Python程序中元组型(Year, Month, Day)数据包含3个整数分别代表年月日，Python语言统一为整数分配4字节存储空间，
根据三个整数的实际意义和大小，他们的实际值分别为2、1、1字节，在网络传输时为了节约带宽就直接用4个字节来传输这三个数据，如何对其格式化呢？
在BlueFi的REPL模式，输入以下Python语句并按回车键，将会看到以下结果：

.. code-block::  python
   :linenos:

    >>> import struct
    >>> year, month, day = 2021, 4, 12
    >>> struct.pack("<HBB", year, month, day)
    b'\xe5\x07\x04\x0c'
    >>> struct.pack(">HBB", year, month, day)
    b'\x07\xe5\x04\x0c'
    >>> struct.unpack(">HBB", b'\x07\xe5\x04\x0c')
    (2021, 4, 12)
    >>> struct.unpack("<HBB", b'\x07\xe5\x04\x0c')
    (58631, 4, 12)

我们使用“struct.pack("<HBB", year, month, day)”接口将“year, month, day”三个整数打包成“b'\xe5\x07\x04\x0c'”，
该接口输出的数据流是适合网络传输的字节流(即bytearray型)，前两个字节“b'\xe5\x07'”是“year”变量的2字节表示，即整数2021的2字节表示，
显然后面两个字节“b'\x04\x0c'”分别是4和12的单字节表示。

细心的你或许已经发现“struct.pack("<HBB", year, month, day)”和“struct.pack(">HBB", year, month, day)”输出的结果不同，
这是为什么？打包格式的字符串“<HBB”和“>HBB”中的“<”和“>”分别表示多字节数据的对齐格式按小端(little-endian)和大端(big-endian)。
所谓小端模式，当一个多字节组成的数据(如2、4、8等字节)的最高字节保存在数据流或地址单元的高序号位置。“struct.pack("<HBB", year, month, day)”使用小端模式，
数值2021的两个字节0xE5和0x07，0x07是高字节保存在数据流的高序号位置，即网络传输时低字节在前高字节在后；
所谓大端模式，当一个多字节组成的数据(如2、4、8等字节)的最高字节保存在数据流或地址单元的低序号位置。“struct.pack(">HBB", year, month, day)”使用大端模式，
数值2021的两个字节0xE5和0x07，0x07是高字节保存在数据流的低序号位置，即网络传输时高字节先发送。

现在我们知道打包格式中使用“<”和“>”指定字节顺序，还有其他顺序格式吗？Python共支持4种打包格式的字节顺序：@、<、>、!，
“@”表示使用本机的大小端模式，“!”表示使用网络的大小端模式(国际网络传输固定使用大端模式，即低字节先传输)。
struct模块支持的打包格式中，字节序/端模式选择字符如下图所示：

.. image:: /../../_static/images/cpython_essentials/python_struct_pack_characters2.jpg
  :scale: 10%
  :align: center


在对“year, month, day”打包时，指定的打包格式字符串“<HBB”和“>HBB”的第2～4位置的3个字符分别表示使用2字节无符号整数(即C/C++的uint16_t)、
1字节无符号整数，也就是说打包后的“year, month, day”共占用4个字节，这是我们预料之中的。当然也可以使用“struct.calcsize("HBB")”来测试该打包格式占用的字节个数：

.. code-block::  python
   :linenos:

    >>> import struct
    >>> struct.calcsize("HBB")
    4

打包格式字符的“H”代表无符号整数(即C/C++的uint16_t)，“B”代表无符号单字节整数(即C/C++的uint8_t和unsigned char)。
那么，struct总共支持多少种打包字符呢？实际上跟C/C++的基本数据类型有关，Python支持的所有打包字符如下图所示：

.. image:: /../../_static/images/cpython_essentials/python_struct_pack_characters1.jpg
  :scale: 20%
  :align: center

如果Python系统中需要将某年某月某日某时某分某秒测得的环境温度(范围：-75~+75摄氏度)等信息打包成数据流从网络接口发送出去，
应该选择使用什么样的格式化字符串呢？

----------------------------

使用struct模块的“pack”和“unpack”等接口可以将多个不同字长的数据打包或解包，对于相同字长的多个数据单元的打包和解包操作就简单很多。
在C/C++语言中，相同字长的多个数据单元被保存在一片地址连续的存储单元中，程序中将这些数据单元总称为“数组”，Python也支持数组，
使用USB数据线将BlueFi连接到电脑，然后打开MU编辑器的控制台，强制让BlueFi进入REPL模式，然后输入命令行导入“array”模块，
并查看该模块支持的接口，如下：

.. code-block::  python
   :linenos:

  >>> import array
  >>> help(array.array)
  object <class 'array'> is of type type
    append -- <function>
    extend -- <function>
  >>> data = array.array('I', [0]*5)
  >>> data
  array('I', [0, 0, 0, 0, 0])
  >>> sampleData = array.array('H', [])
  >>> len(sampleData)
  0
  >>> sampleData.append(65535)
  >>> sampleData
  array('H', [65535])
  >>> sampleData.append(65537)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  OverflowError: value must fit in 2 byte(s)

可以看出，BlueFi的Python解释器的“array”类仅有2种接口函数：append和extend，从这两个接口的名称即可知道他们分别为“向数组添加一项”、
“用指定的数组扩展原数组”。使用Python语言定义/声明一个数组时，由于数组中各项数据单元的字长都是相同的，
用来指定数据类型和字长的格式化字符串仅需要一个字符，其规则与前面“struct”的打包字符完全相同。“struct”模块用于通讯接口的数据打包与解包，
那么Python的“array”的使用场景有哪些呢？

数据采集和处理单元，譬如声音数据的采样和FFT变换处理。绝大多数的声音采样以固定的采样频率和分辨率(单采样点数据的字长)，
这是时域操作，保存时域采样结果最佳数据集就是数组，根据采样时间和采样频率可以确定采样点个数，即数组的长度是确定的。
其他数据采集，譬如IMU(惯性测量单元)、环境温湿度检测单元等都有相似的工作机制，“array”非常适合这些场景。

使用BlueFi的数字MEMS麦克风组件，我们用下面的示例代码可以采样声音信号并输出到屏幕上：

.. code-block::  python
   :linenos:

  import time
  import array
  import board
  import audiobusio
  mic = audiobusio.PDMIn(
          board.MICROPHONE_CLOCK, board.MICROPHONE_DATA,
          sample_rate=16000,   # 16KHz (=16000 sample-dots/second)
          bit_depth=16)        # 16-bit wordsize
  samples = array.array("H", [0] * 160)  # sample data of 10ms

  while True:
      mic.record(samples, len(samples))
      print( samples )
      time.sleep(0.5)

上面示例程序中，前4行分别导入4个库模块，包括“array”和“audiobusio”；第5～8行(实际是一个语句)声明一个麦克风，
第6行指定连接麦克风的2个引脚(这是BlueFi的固定用法)，第7和8行分别指定麦克风的采样频率和采样数据字长；
第9行声明一个数组“samples”，该数组共有160项，每一项都是16位无符号的数据，很显然“array.array(p1, p2)”参数p1必须与第8行的麦克风参数一致，
数组的项数决定我们每次采样的时长(根据采样频率来确定)；程序的无穷循环中首先调用麦克风的“record”接口获取采样数据，
即更新数组“samples”，然后将这些数据直接打印到屏幕上。

----------------------------

“array”和“struct”都是Python语言中专门处理C/C++数据类型的特殊模块，“array”机制仅适合相同字长的数据集，
“struct”根据打包格式字符串能够处理多种字长的数据组成的数据集。在计算机硬件的层面上，数组和结构体型等数据集被保存在一片地址连续的存储空间内，
如此保存的数据集的访问速度较快。



.. admonition:: 
  总结：

    - struct
    - array
    - data type

