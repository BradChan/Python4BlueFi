无线电波与广播通讯
==========================

在前一节中我们使用BlueFi强大的蓝牙通讯技术很容易就模拟初一个电脑的键盘，我让们初步领略了蓝牙这种近距离无线通讯的便捷性。
从本节开始，我们将从零开始掌握无线电波通讯的原理和实现方法。了解这些知识的目的是为了更好地掌握蓝牙、WiFi等无线通讯技术
的原理和实现方法。

无线电波通讯对上世纪的第二次世界大战的最终走向的影响非常大。战争期间的无线电波通讯主要用于指挥部与战地之间的联络，
由于无线电波是一种开放的电磁波，发送信号的电波频段被敌方扫描到之后，通过无线电波发送的作战指令对敌方几乎是透明的，
为此人们就发明了密码学和信息加密技术，将作战指令先加密后再用无线电波发送出去，即使敌方能够接收到这些指令，但没有解密的密
码本也无法获取真正的作战指令。

然而，二战的最后阶段正是德国的作战指令被盟军解密后并制订针对性的作战方案才让德国军队开始节节败退。可以说，盟军能成功地
解密无线电波所传送的德军作战指令是赢得二战的关键因素之一(后来有人评价这是惟一因素，不是因素之一)。

基于无线电波的通讯始终是广播的形式，即使今天的科技已经远超过二战时期，我们仍使用无线电波通讯。无论蓝牙或是WiFi，基础
物理层的通讯仍采用无线电波的广播。但是今天的加密技术也已经远超二战时期的，人工智能的奠基人——图灵在二战末期的主要工作
就是解密德国军队的加密电文，为此他发明了图灵机，后世人们将图灵机称为AI机器的原型。虽然今天的计算机已经远超二战时期的
图灵机，如果用今天的计算机解密当时的加密电文应该是秒级以内的工作，遗憾的是今天的加密技术也得到快速发展，使用今天的计算机
解密当今的无线电波传输的密文仍是很难的。

无线电波的广播通讯是什么样的呢？我们今天可以使用BlueFi来模拟他，所以本节课我们不直接使用BlueTooth和WiFi这样的现代
无线电通讯技术，反而采用最原始的无线电波的广播通讯，我们能够体验到那个时代人们的通讯技术，也有助于我们理解蓝牙和WiFi
技术原理。

-----------------------

我们首先使用BlueFi设计一个无线电波的信息发送者/广播程序。该程序实现如下功能：当按下按钮A时，发送“BlueFi-A:1xx”；
当按下按钮B时，发送“BlueFi-B:2xx”。这两条信息每次都不完全一样，因为我们使用随机数发生器生成几个数据填充到信息的
最后两个位置。Python脚本程序如下：

.. code-block::  python
  :linenos:

    import time, random
    from hiibot_bluefi.basedio import Button
    from adafruit_ble_radio import Radio
    button = Button()
    rfc = Radio(channel=8)  # sender and receiver must use a same channel
    myhead = "BlueFi-"
    msgA = myhead + "A:{}"
    msgB = myhead + "B:{}"
    while True:
        button.Update()
        if button.A_wasPressed:
            ra=random.randint(100,200)
            rfc.send( msgA.format(ra) )
            print( msgA.format(ra) )
        if button.B_wasPressed:
            rb=random.randint(200,300)
            rfc.send( msgB.format(rb) )
            print( msgB.format(rb) )
        time.sleep(0.1)




-----------------------

然后我们需要设计一个无线电波的信息接受者/侦听程序。该程序实现如下功能：将接收到的全部信息都显示到LCD屏幕上。
无线电波的信息接收者程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import NeoPixel
    from adafruit_ble_radio import Radio
    pixels = NeoPixel()
    pixels.brightness = 0.2
    rfc = Radio(channel=8) # sender and receiver must use a same channel
    while True:
        rmsg = rfc.receive_full()
        if rmsg:
            pixels.pixels[0]=(0,255,0)
            pixels.pixels.show()
            rmsg_bytes = rmsg[0]
            rmsg_strength = rmsg[1]
            rmsg_time = rmsg[2]
            print("Recieved {} (strength {}, at time {})".format(
                  rmsg_bytes, rmsg_strength, rmsg_time))
            pixels.pixels[0]=(0,0,0)
            pixels.pixels.show()
        pass


-----------------------------

至此，我们已经把无线电波的广播通讯实验程序都已准备停当，为了能够更好地体验无线电波的广播通讯，建议你约上3位以上同学都带着各自的BlueFi,
或者你自己至少又3个或更多个BlueFi就不需要约其他同学，你一个人就可以完成本节内容的体验。

1) 将某个BlueFi当作无线电波的信息发送者，将本节的第1个示例程序保存到这个BlueFi的/CIRCUITPY/code.py文件中

2) 将另一个BlueFi当作无线电波的信息接收者，将本节的第2个示例程序保存到这个BlueFi的/CIRCUITPY/code.py文件中。请务必注意，这个程序的无线电频道必须与发送者的频道保持一致

3) 再将另一个BlueFi也当作无线电波的信息接收者，将本节的第2个示例程序保存到这个BlueFi的/CIRCUITPY/code.py文件中，但修改无线电频道与前两个BlueFi的频道不同

-----------------------------

无线电波的广播通讯实验分两步：

1) 第一步，使用上述的三个或以上个BlueFi，并确保这些BlueFi之间的距离都在10米左右的范围内，按下信息发送者的A或B按钮，观察信息接收者的反应

2) 第二步，将所有信息接收者的BlueFi的程序中无线电频道全部设置成与信息发布者的频道保持一致，再重复试验

试验之后，将所有同学组织在一起讨论本试验所看到的、自己所理解的，以及自己所思考的关于无线电波的广播通讯，对于二战时期关于无线电波通讯的很多案例
都可以对照这个试验进行讨论，谈谈自己对那个时候的事实是如何认识的。

通过本节教程的学习，最后请大家谈一谈对BlueTooth和WiFi通讯技术的认识。
