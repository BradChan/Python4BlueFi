====================
使用Bluefi开源库
====================

BlueFi具有丰富的开源库(5.x版本已经包含的Python开源达328种)，加上50+种内建库，BlueFi支持的开源库接近300种！

与其他支持MicroPython的单板机相比，BlueFi所支持的库非常丰富，将助你快速实现自己的创意。

如此丰富的开源库，我们不可能在一个项目中全部用到他们。BlueFi的Python库基本理念：一个项目需要用到那些库，就将这些库文件保存到
BlueFi的/CIRCUITPY/lib/文件夹内。

-------------------------------------------

将项目依赖的库保存到/CIRCUITPY/lib/文件夹
-------------------------------------------

根据“使用时再将需要的库文件复制到/CIRCUITPY/lib/文件夹”规则，我们只需要在电脑的资源管理器中，从下载并解压后的BlueFi开源库
文件夹内将需要用到的库文件(包含文件夹)直接复制-粘贴或拖放到/CIRCUITPY/lib/文件夹。整个操作非常便捷。

如何确定一个项目到底用到哪些开源库？

按照Python的import(导入)模块的规则，我们很容易确定每一个项目具体所用到的开源库清单。事实上，一个Python项目的设计者事先一定清楚
自己需要用到哪些开源库。我们用一个BlueFi出厂时默认的示例代码为例来说明开源库的用法。

.. image::  ../../_static/images/bluefi_lib/default_example.jpg
  :scale: 20%
  :align: center

请打开已经解压到你电脑磁盘上的BlueFi开源库的文件夹，进入/examples/bluefi_advance/DefaultExample/文件夹，这里的code.py就是
BlueFi默认示例程序文件，这个源文件大小占4KB，另外有一个/images/子文件夹，其中包含有一个名叫“bg_music.bmp”的240x240点阵16位
颜色的位图图片，如果你在电脑上预览该图片时，你会发现这个图片就是BlueFi默认示例的显示器的背景图。

再打开BlueFi的“CIRCUITPY”磁盘，你能看到“code.py”文件以及“/images/bg_music.bmp”与开源库的
/examples/bluefi_advance/DefaultExample/文件夹的文件完全一致。无论你如何修改BlueFi的“CIRCUITPY”磁盘文件，如果某一天
你想恢复出厂默认程序，那么你需要 1)将“/examples/bluefi_advance/DefaultExample/code.py”文件复制到BlueFi的“CIRCUITPY”磁盘
的根目录；2)将“/examples/bluefi_advance/DefaultExample/images/bg_music.bmp”图片文件复制到BlueFi的“/CIRCUITPY/images/”
文件夹；3)将该示例程序所依赖的Python库复制到“/CIRCUITPY/lib/”文件夹中。

那么问题来了，这个示例程序的依赖库包括哪些？我们来看看该示例程序的源码。

.. code-block::  python
  :linenos:

  """
      this code is the defult example of HiiBot BlueFi. The steps:
      1. import we used python modules and classes
      2. instantiate we used Classes
      3. creat a group of graphics elements (named as: graphicsGroup)
      4. load a bitmap as the background, and append into graphicsGroup
      5. creat a Text Label to show the Sound Level from microphone
        (font, the maximum length text, x- and y- coordinate, color, and font scale)
      6. append the Text Label into graphicsGroup
      7. show graphicsGroup on the BlueFi LCD screen
      8. set the default brightness of RGB pxiels (5%) and LCD screen (100%)
      9. set the default value of the screensaverCnt
      10. define two functions to realize the screensaver
      11. enter a an infinite loop until the shutdown
          1) get the sound level with microphone
          2) draw pillar according to the sound level
          3) update the value of the sound level to screen
          4) call the screensaver function
          5) toggle the red LED
      
      welcome to Python world with HiiBot BlueFi !
  """
  import time
  import displayio, terminalio
  from adafruit_display_text.label import Label
  from hiibot_bluefi.basedio import LED, NeoPixel, Button, TouchPad
  from hiibot_bluefi.soundio import SoundIn, SoundOut
  from hiibot_bluefi.screen import Screen
  from hiibot_bluefi.sensors import Sensors
  #  instantiate we used Classes
  led = LED()            # LED class be instantiated as "led"
  rgbpixels = NeoPixel() # NeoPixel class be instantiated as "rgbpixels"
  button = Button()      # Button class be instantiated as "button"
  touch = TouchPad()     # TouchPad class be instantiated as "touch"
  mic = SoundIn()        # SoundIn class be instantiated as "mic"
  speak = SoundOut()     # SoundOut class be instantiated as "speak"
  screen = Screen()      # Screen class be instantiated as "screen"
  sensors = Sensors()    # Sensors class be instantiated as "sensors"
  #  creat a group of graphics elements
  graphicsGroup = displayio.Group(max_size=2, scale=1) 
  #  load a bitmap as the background
  bgFigure = displayio.OnDiskBitmap(open("/images/bg_music.bmp", "rb"))
  bgGrid = displayio.TileGrid(bgFigure, pixel_shader=displayio.ColorConverter())
  graphicsGroup.append(bgGrid)
  # text for sound level (必须给出显示Label的最长字符串的占位, 以及坐标位置、字体颜色和缩放)
  text_soundLevel = Label(terminalio.FONT, text="12345.78", 
                          x=146, y=230, color=0xFF0000, scale=2)
  graphicsGroup.append(text_soundLevel)
  screen.show(graphicsGroup) # 将graphicsGroup显示在screen(BlueFi的LCD彩屏)
  rgbpixels.brightness=0.05     # set the brightness of RGB pixels as 5%
  screen.display.brightness=1.0 # set the brightness of LCD screen as 100%
  screensaverCnt=1000
  slf = False
  #  the effect of Function: code reuse!
  def logicComb():
      return button.A or button.B or \
            sensors.proximity>2  or \
            touch.P0 or touch.P1 or touch.P2
  #  we define a screensaver to save power and prolong screen lifespan
  def screenSaver():
      global screensaverCnt, slf  #  a global variable be used
      #  if screen awakened, set its brightness into the 100%
      if logicComb():  #  the function be called
          screen.display.brightness = 1.0
          speak.play_midi(103, 16)
          #  waiting some times
          while logicComb(): #  the function be called
              time.sleep(0.1)
          screensaverCnt=1000
          slf = True if not slf else False
      if screensaverCnt<=0:
          screen.display.brightness=0.0
      else:  #  screen brightness gradually dim
          screensaverCnt -= 1
          screen.display.brightness = min(1.0, screensaverCnt/600.0)

  speak.volume = 0.3
  speak.enable = 1
  led.white = 0
  minLevel = mic.sound_level+10 # the ambient noise
  maxLevel = minLevel + 1000    # peak of sound level
  loopCounter = 0
  #  enter a an infinite loop until the shutdown!
  while True:
      soundLevel = mic.sound_level # get 
      rgbpixels.drawPillar( soundLevel )
      if slf:
          text_soundLevel.text = "{:.2f}".format(soundLevel)
      screenSaver()  #  call a function
      loopCounter+=1
      if 0 == loopCounter%10:
          led.redToggle()
          #led.whiteToggle()

这个示例程序源码的前22行是多行注释语句，第23行和第24行分别导入BlueFi的三个内建库——time、displayio、terminalio。
第25行是从BlueFi的“/CIRCUITPY/lib/adafruit_display_text/”文件夹的“label.py”模块中导入Label库，
第26～29行是从BlueFi的“/CIRCUITPY/lib/hiibot_bluefi/”文件夹的“basedio.py”、“soundio.py”、“screen.py”和
“sensors.py”等4个模块中分别导入LED、NeoPixel等库。

什么是BlueFi的内建库？已经包含在BlueFi固件内的Python库模块都是内建的，使用时你只需要先用import导入相应库即可。

如何知道BlueFi有哪些内建库呢？让BlueFi进入REPL模式，使用“help("modules")”即可看到BlueFi内建库的列表。

除了BlueFi内建的库，其他非内建库，使用前必须将相应的库源码(py或mpy格式)复制-粘贴或直接拖放到BlueFi的“/CIRCUITPY/lib/”
文件夹中，否则执行到导入该库的Python语句时，BlueFi将自动发出错误提示并终止程序执行。BlueFi的默认示例实际上用到两个库：
1) adafruit_display_text；2) hiibot_bluefi。这两个库文件中都包含有多个Python类模块，我们在第25～29行中分别将这些
本示例用到的Python类模块逐个导入。如果执行该示例程序(code.py)之前，你没有将adafruit_display_text和hiibot_bluefi两个
开源库文件夹复制-粘贴或拖放到BlueFi的“/CIRCUITPY/lib/”文件夹中，BlueFi将发出错误提示并终止程序执行。

至此，你已经完全掌握BlueFi开源库的使用规则，操作方法。在电脑资源管理器中如何复制-粘贴文件或拖放文件的操作都不是重点。

其他代码这里不关心，你可以根据程序执行过程中的现象对照每一行代码了解每一行代码的作用。


找到自己需要使用的BlueFi开源库
----------------------------------

现在我们剩下最后一个问题，一个Python项目需要使用的开源库都在哪里可以找到？

前一个向导下载的BlueFi开源库压缩包解压之后就是Python类开源库的源码和示例文件，你所需要全部非Python内建库都在这个文件夹中，
所有Python内建库都是固件自带的，只需要import即可使用。如何从两百多个开源库中找到自己需要的库呢？

这就需要来了解BlueFi开源库的命名规则，掌握这个规则能够帮助你快速找到自己需要用到的库是那一些。BlueFi开源库的命名规则：

  . 开发者-功能名称/器件名称

这个规则是由CircuitPython项目的发起人——Adafruit制订，几乎与Arduino开源社区的库/项目的命名规则一致。“开发者”名称为首，便于
我们根据开发者快速地定位到某一类开源；“功能名称”便于根据功能分类快速定位某一类功能的开源库；“器件名称”是因为CircuitPython
注意面向嵌入式系统，根据嵌入式系统功能部件的硬件来斋宿定位到某个开源库。

譬如，你需要使用到BlueFi的基本硬件功能单元的开源库，只需要根据开发者名称搜索“hiibot bluefi”即可找到所有适用于BlueFi的开源库。
如果你需要使用microbit的小车底盘硬件，你只需要知道该硬件单元的公开名称，譬如cutebot(由深圳恩孚开发的一款智能小车套件)，你只需要
在BlueFi开源库中搜索“cutebot”即可定位到该硬件套件的开源库，将该文件/文件夹复制-粘贴或拖放到BlueFi的“/CIRCUITPY/lib/”文件夹中
即可使用。

同样地，BlueFi开源库文件夹中的examples也有特殊命名规则以方便查找：

  . 板名称/开源库名称_示例功能名称.py

譬如，“/examples/bluefi_wifi_apscan.py”示例文件实现wifi扫描周边AP的功能，“/examples/bluefi_wifi_connectAP.py”示例
文件实现wifi连接到一个指定的AP热点。这两个示例文件都适合于BlueFi单板机。

注意，你不能直接把BlueFi开源库的examples文件夹中的示例文件直接复制-粘贴或拖放到BlueFi的磁盘，因为BlueFi只能接受“code.py”作为
用户程序名称，其他名称的py文件会BlueFi处理成普通的文件，不会自动执行！正确的使用示例程序的方法：将需要使用的示例名称修改为code.py
之后在复制-粘贴或拖放到BlueFi的“CIRCUITPY”磁盘跟目录即可。

---------------------------------

如果你想要自己定义BlueFi的库文件，请点击“Next”按钮进入下一个向导。

本向导的总结如下：

.. admonition::  使用BlueFi开源库

  - step1: 根据自己需要找到所用到的BlueFi开源库源文件(py或mpy格式都可以)
  - step2: 将用到的BlueFi开源库文件/文件夹复制-粘贴或拖放到BlueFi的“/CIRCUITPY/lib/”文件夹中

    - 用合适的方法在code.py文件中将用到的库模块逐个import(导入)

