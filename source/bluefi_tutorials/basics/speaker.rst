如何让计算机发出声音
======================

现在你已经知道BlueFi能够产生绚丽的光效，BlueFi能产生声音吗？当然可以，本节我们来认识BlueFi的音频输出设备——喇叭及其周边的用法，
使用这些设备可以产生基本音调、播放音乐文件。

BlueFi的喇叭在B按钮的反面，出声孔正好对准板子的右边缘，用手握住BlueFi板时，请勿堵住出声孔！

------------------------

自制钢琴
------------------------

我们可以借助于BlueFi的三个触摸盘模拟一个钢琴，虽然仅有三个琴键。示例程序如下：

.. code-block::  python
  :linenos:

    from hiibot_bluefi.basedio import TouchPad
    from hiibot_bluefi.soundio import SoundOut
    touch = TouchPad()
    spk = SoundOut()
    spk.enable = 1
    while True:
        if touch.P0:
            spk.start_tone(1047) #(523)
        elif touch.P1:
            spk.start_tone(1175) #(587)
        elif touch.P2:
            spk.start_tone(1319) #(659)
        else:
            spk.stop_tone()

将本示例程序保存到/CIRCUITPY/code.py文件，BlueFi将立即执行该程序。用你的右手大拇指和食指夹住BlueFi的Gnd信号盘，左右的食指
去触摸或按压触摸盘P0、P1和P2，你将有“弹钢琴”的感觉，当然这个钢琴的音键仅有3个。如何才能实现更多的钢琴琴键呢？

  示例代码分析：

    - 第1行，从“/CIRCUITPY/lib/hiibot_bluefi/basedio.py”模块中导入一个名叫“TouchPad”的类
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/soundio.py”模块中导入一个名叫“SoundOut”的类
    - 第3行，将导入的“TouchPad”类实例化为一个实体对象，名叫“touch”
    - 第4行，将导入的“SoundOut”类实例化为一个实体对象，名叫“spk”
    - 第5行，设置spk的enable属性为1，即允许喇叭输出声音
    - 第6行，开始一个无穷循环的程序块
    - 第7行(无穷循环程序块的第1行)，判断触摸盘P0是否被触摸
    - 第8行(无穷循环程序块的第2行)，如果触摸盘P0被触摸则开始播放频率为1047Hz的音调
    - 第9行(无穷循环程序块的第3行)，否则，判断触摸盘P1是否被触摸
    - 第10行(无穷循环程序块的第4行)，如果触摸盘P1被触摸则开始播放频率为1175Hz的音调
    - 第11行(无穷循环程序块的第5行)，否则，判断触摸盘P2是否被触摸
    - 第12行(无穷循环程序块的第6行)，如果触摸盘P2被触摸则开始播放频率为1319Hz的音调
    - 第13行(无穷循环程序块的第7行)，否则，即以上条件都不满足时
    - 第14行(无穷循环程序块的第8行)，停止播放基本音调

本示例程序的无穷循环程序块内包含一个逻辑嵌套的程序结构：if cond1: ..  elif cond2:  ..  elif cond3:  ..  else:  ..。虽然这种
嵌套逻辑的程序流程不难理解，但要注意“elif”的拼写(“elif  cond”相当于“else if  cond”)，逻辑上能够执行elif这个语句时表面前一个测试条件
为False，判断当前这一行的elif后面条件是否为True，如果为True则执行本程序块，否则就继续测试下一个条件。

结合本示例，或许能帮助你理解这种结构的条件嵌套。我们首先判断P0是否被触摸，如果被触摸则播放1047Hz音调并忽略后续的条件判断和else；如果P0未被
触摸则跳到第2个elif语句，即判断P1是否被触摸，如果被触摸则播放1175Hz音调并忽略后续的条件判断和else；如果P1未被触摸则跳到第2个elif语句，
即判断P2是否被触摸，如果被触摸则播放1319Hz音调并忽略else；如果P2也未被触摸(即三个触摸盘都未被触摸)，执行else后面的程序块，即停止播放音调。

触摸P0/P1/P2时我们播放不同频率的声音输出，你的耳朵肯定也完全能分辨出三种不同的音调。的确，忽略声音的高低，声音不同就是频率不同。
无论是钢琴或是其他弦乐器，不同弦的粗细不同，发出的声音频率不同，我们的耳朵能根据声音频率分辨音调。


自制旋律
------------------------

使用频率-音调的对照关系，我们只需要让BlueFi喇叭输出不同频率的声音就是产生不同音调，每个音调持续的节拍数不同、暂停时间不同，我们就能用编程
的方式产生不同的旋律。当你用百度查找“频率-音调”的对照关系时，你会发现那些数字很难直接使用。这一节我们使用midi号和节拍来设计旋律。先用下面这个
示例来了解midi号和节拍的用法：

.. code-block::  python
  :linenos:

    from hiibot_bluefi.basedio import TouchPad
    from hiibot_bluefi.soundio import SoundOut
    touch = TouchPad()
    spk = SoundOut()
    spk.enable = 1
    spk.bpm = 120
    while True:
        if touch.P0:
            spk.play_midi(84, 2)
        if touch.P1:
            spk.play_midi(86, 2)
        if touch.P2:
            spk.play_midi(88, 2)

本示例的逻辑没有逻辑嵌套，仅仅是判断P0/P1/P2触摸盘被触摸时就分别播放第84/86/88号midi音调，并持续1/2拍。我们这里不必逐行说明每个语句的执行
效果。请你先在BlueFi上执行本示例程序，并尝试改变节拍数：1代表1拍，2代表1/2拍，依次类推8代表1/8拍。如果你需要改变节拍的时长，修改本示例的第
6行语句中spk的bpm属性值，120bpm相当于每拍时长为0.5秒：1分钟(60秒)120拍-->0.5s/bea。增加spk的bpm属性值将导致更短的节拍，减小bpm属性值
意味着拉长每个节拍时间。

下面我们使用两个列表分别定义一段旋律的midi号和节拍数，再用循环的形式播放这段旋律：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import TouchPad
    from hiibot_bluefi.soundio import SoundOut
    touch = TouchPad()
    spk = SoundOut()
    spk.enable = 1
    spk.bpm = 120
    midis=[83, 84, 86, 88, 89, 91, 93]
    beats=[1, 2, 2, 1, 1, 2, 2]
    while True:
        for i in range(len(midis)):
            spk.play_midi(midis[i], beats[i])
        time.sleep(1)

虽然我们只是顺序地播放基本音调，由于持续的节拍数稍微变化，听起来就有点旋律的味道，你可以参考本示例自行设计悦耳的旋律。

关于midi编号与钢琴琴键之间的关系，请你使用搜索引擎自行查找、阅读，当你get到这一关系后，你可以把自己在钢琴上弹奏的旋律给记录下来，包括
琴键序列和持续时长，很容使用BlueFi重复播放你弹奏的旋律，如果需要修改某些不满意的音节(音调和节拍)是很容易的，然后再重新播放。如此一来，
BlueFi将成为你谱曲的小助手。


播放音频文件
------------------------

或许你觉得播放基本音调并不能满足自己的需要，BlueFi支持wav格式音频文件中的音乐或声音。BlueFi采用磁盘映射和文件拖放等操作，你很容易把
音频文件拖放至/CIRCUITPY/sound/文件夹，然后使用spk的play_wavfile函数播放这种格式的音频文件。对于wav格式的文件，要求采用16KHz采
样率，16位分辨率。关于wav格式的文件更多属性和介绍，请使用搜索引擎自行查阅。

此处我们使用A和B按钮播放不同的音频文件作为示例，帮助你掌握使用BlueFi播放wav格式音频文件的编程方法：

.. code-block::  python
  :linenos:

    from hiibot_bluefi.basedio import Button
    from hiibot_bluefi.soundio import SoundOut
    button = Button()
    spk = SoundOut()
    spk.enable = 1
    files = ["/sound/Coin.wav", "/sound/rise.wav"]
    while True:
        if button.A:
            spk.play_wavfile(files[0])
        if button.B:
            spk.play_wavfile(files[1])

运行本示例程序之前，务必将两个wav格式文件保存在BlueFi的/CIRCUITPY/cound/文件夹内，此处可以下载本示例程序使用到的两个音乐文件
:download:`Coin.wav </../../_static/sound/Coin.wav>` 和
:download:`rise.wav </../../_static/sound/rise.wav>` 到你的电脑磁盘上，
然后在BlueFi的CIRCUITPY磁盘根目录新建一个sound文件夹，并把这两个音频文件拖放至/CIRCUITPY/sound/文件夹。过程如下图所示：

.. image:: /../../_static/images/bluefi_basics/sound_files.gif
  :scale: 20%
  :align: center

请注意，BlueFi的SPI文件系统仅有2MB空间，存放Python库文件和用户程序会占用1/4空间，其余空间可以用于保存你的sound、image等格式文件，
但务必注意文件大小，有限的存储空间务必节约使用，否则一不小心就把BlueFi磁盘塞满了。


.. admonition:: 
  总结：

    - 喇叭和声音输出
    - 声音和频率
    - 实体对象的属性赋值
    - 变量
    - 变量赋值
    - 变量自增/自减
    - 本节中，你总计完成了14行代码的编写工作


.. Important::
  **PWMLED类的接口**

    - enable (属性, 可读可写, 有效值：0或1), BlueFi喇叭的使能控制，0:禁止声音输出; 1:允许声音输出
    - bpm (属性, 可读可写, 有效值：30~360), 每分钟的节拍数(Beats Per Minute)，指定节拍的长短
    - start_tone (函数, 输入参数: 音调频率, 无返回值), 开始播放指定频率的音调，直到执行stop_tone()才停止
    - stop_tone (函数, 无参数, 无返回值), 停止播放声音
    - play_tone (函数, 输入参数: 频率和持续时长, 无返回值), 播放指定频率的音调并持续指定的时长
    - play_midi (函数, 输入参数: midi号和节拍数, 无返回值), 播放指定midi号的音调并持续指定的节拍
    - play_wavfile (函数, 输入参数: wav格式文件名和路径, 无返回值), 播放指定音频文件
