给计算机装上“耳朵”
======================

标准的桌面计算机内置声音的输入和输出设备，此类音频接口俗称声卡，将耳机或音箱音频线插入声卡后我们能听到声音，将麦克风音频线插入声卡后
可以使用计算机录音。BlueFi也带有与声卡相似的功能，上一届我们了解了BlueFi音频输出的用法，包括输出基本音调和播放音频文件。这一节我们
来了解BlueFi的“耳朵”。

计算机一旦能听到世界的声音就能实现很多很酷的功能，譬如我们在RGB像素灯珠的那一节中已经使用BlueFi的麦克风输入信号去绘制柱状图，
BlueFi的彩灯将随着我们播放音乐或敲击桌子的节奏一起跳动，还记得那个很酷的作品吗？

------------------------

让声音可见(声音的形状)
------------------------

耳朵能听到声音，我们这一节的第一个示例打算让声音可见，就是能看见声音！也就是使用计算机记录并绘制声音信号，这么酷的设想，先看看代码：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.soundio import SoundIn
    mic = SoundIn(numSamples=8)
    while True:
        print("s = {:.1f}".format(mic.sound_level))
        time.sleep(0.01)

仅用6行程序语句就可以让声音可见！在BlueFi上执行本示例程序，我们使用MU编辑器的“绘图器”就可以记录并绘制一段声音的形状。

.. image:: /../../_static/images/bluefi_basics/audio_record.jpg
  :scale: 40%
  :align: center

这就是某个场景中6秒时长的声音形状！声音长这个样子很奇怪吗？或许你想了解声音的细节，我们可以对一段音乐场景的声音形状的细节进行放大，
你会更加明白声音形状为何长成这样子。

.. image:: /../../_static/images/bluefi_basics/audio_zoom.gif
  :scale: 40%
  :align: center

返回来，我们再来分析本示例程序中的几个关键语句。前两个语句不必赘述。第3个语句是将SoundIn类实例化为mic，即启用BlueFi的麦克风，
并指定每次采样的数据个数为8。第5行程序是将mic的sound_level属性(连续8次声音采样的均方差)值输出到LCD屏幕(控制台)上。

很显然，我们不是直接读取麦克风的瞬时输出来绘制声音图，而是连续8次采样值进行均方差计算，用他们的均方差值当作音频信号。这是因为，
序列信号的均方差描述的是信号波动范围，或者说方差代表的是信号中交流成分的强弱，即信号的交流功率。我们在上图中记录的是声音信号强弱
的变化，建议你修改“numSamples=8”这个参数，试一试更多或更少的单次采样数的声音形状变化规律，从而为某些特定场景的声音记录设备确定
一个合理的采样处理方案。


随着节奏跳动的光效
------------------------

或许你已经发现，只要我们使用“print(xxx)”将变量或字符串显示到LCD屏幕(控制台)时，程序的执行速度明显下降，这是因为“print()”
函数使用串口将变量或字符串发送控制台和LCD屏幕上会消耗大量的时间，这些时间消耗甚至影响声音记录的速度。

我们的RGB像素灯珠的响应速度很快，而且仅有5颗灯珠，把声音信号的变化强弱用这些灯珠指示出来，显示数据的延迟几乎让人无感。

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import NeoPixel
    from hiibot_bluefi.soundio import SoundIn
    pixels = NeoPixel()
    pixels.brightness = 0.2
    mic = SoundIn(numSamples=8)
    vmin = mic.sound_level
    vamx = vmin + 500
    while True:
        pixels.drawPillar(mic.sound_level, vmin, vamx)
        time.sleep(0.01)

如果你修改实例化SoundIn类时将numSamples参数改为更大的值，尤其超过500时，你会发现光效的跳动与节奏变化之间存在明显的滞后。我们使用
numSamples=8时，完全无滞后感。这是为什么？


闻声自动开启的路灯
------------------------

晚上你走过一个漆黑的楼道时，会不会下意识地拍拍巴掌或跺跺脚希望能唤醒楼道的照明灯？生活中很多楼道灯都是这样设计的，有人路过时被声音
唤亮，延迟一会儿之后自动关闭，这样的工作模式可以节约耗电。

我们下面使用BlueFi的麦克风和5颗RGB像素灯珠来实现这一功能的楼道节能灯。

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

    def delayoff():
        global delayCnt
        time.sleep(0.01) # 10ms, x100times=1s
        if delayCnt<=0:
            pixels.clearPixels()
        else:
            delayCnt -= 1

    while True:
        delayoff()
        if mic.loud_sound(200):
            pixels.fillPixels((255,255,255)) # white
            delayCnt = 1000

看起来这个程序代码已经很长！下面我们逐行分析代码的执行效果。

示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/basedio.py”模块中导入一个名叫“NeoPixel”的类
    - 第3行，从“/CIRCUITPY/lib/hiibot_bluefi/soundio.py”模块中导入一个名叫“SoundIn”的类
    - 第4行，将导入的“NeoPixel”类实例化为一个实体对象，名叫“pixels”
    - 第5行，设置pixels的brightness属性(即RGB像素彩灯的整体亮度)为0.2，合理取值范围：0.05(亮度最小)~1.0(亮度最大)
    - 第6行，调用pixels的函数clearPixels，关闭所有RGB像素灯珠
    - 第7行，定义一个变量名叫delayCnt，用于处理节能开启后自动延迟关闭的延迟时长，初始赋0
    - 第8行，将导入的“SoundIn”类实例化为一个实体对象，名叫“mic”
    - 第10行，开始定义一个函数，名为delayoff，无输入参数无返回值，该函数用来处理灯珠开启后自动延时关闭的工作
    - 第11行(函数delayoff程序块的第1行)，声明本函数中使用到的全局变量delayCnt
    - 第12行(函数delayoff程序块的第2行)，执行time的sleep方法，参数为0.01秒，即系统空操作10ms
    - 第13行(函数delayoff程序块的第3行)，判断变量delayCnt的值是否小于等于0
    - 第14行(函数delayoff程序块的第4行)，如果变量delayCnt的值小于等于0，调用pixels的函数clearPixels关闭全部灯珠
    - 第15行(函数delayoff程序块的第5行)，否则，即如果变量delayCnt的值大于0
    - 第16行(函数delayoff程序块的第6行)，如果变量delayCnt的值大于0，将该变量自减1
    - 第18行开始一个无穷循环的程序块
    - 第19行(无穷循环程序块的第1行)，调用自定义函数delayoff，处理延时自动关闭灯珠的事务
    - 第20行(无穷循环程序块的第2行)，根据mic的函数loud_sound返回值判断是否感知到很大的声音(阈值设置为200)，如果条件为True，执行下面的程序块
    - 第21行(无穷循环程序块的第3行，逻辑判断条件为True时执行的程序块第1行)，调用pixels的函数fillPixels让所有灯珠发出白色照明光
    - 第22行(无穷循环程序块的第3行，逻辑判断条件为True时执行的程序块第2行)，设置变量delayCnt为1000

这个示例程序中，我们声明并定义一个名叫“delayoff”的函数来处理自动延迟关闭灯珠，在主程序的“while True:”循环中调用该函数，主循环中只是
根据mic的函数loud_sound返回值判断是否感知到很大的声音，如果感知到很大声音(代表有人需要开灯照明)则让灯珠亮起，并设置变量delayCnt的初始值
为1000，在delayoff函数中，我们会不停滴减少这个变量，当该变量变为0时关闭全部灯珠。

函数，几乎所有的程序都会用到函数的概念。用这个概念来设计程序有很多优点：1) 模块化程序设计风格，让程序的结构更合理，便于阅读和维护；2) 代码复用，
有些基础功能的程序被写出函数形式，可以在很多歌地方通过调用该函数实现需要的基础功能；3) 参数化，有些功能算法需要在程序的很多地方使用，但算法的输入
参数不完全相同，将功能算法封装成有输入参数的函数，我们可以大大提高编码效率。

在Python中声明和定义函数非常容，但需要区分函数内所用的变量是局部变量还是全部变量，如果使用到全部变量必须用“global”进行声明。此外，绝大多数
编程语言都要求先声明和定义函数，再调用函数，Python也不例外。


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
  **SoundIn类的接口**

    - sound_level (属性, 只读, 有效值：0.0~65535.0), BlueFi的麦克风感知到声音信号变化的强弱
    - loud_sound (函数, 输入参数: 声音很大的阈值(最小值), 返回值: 0或1), BlueFi的麦克风感知到声音信号变换大于设定阈值, 返回1:感知到很大声音，0:未感知到很大声音

