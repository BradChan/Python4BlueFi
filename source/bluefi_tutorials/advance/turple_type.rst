tuple及其操作
======================

tuple(元组)是Python语言的一种基本数据类型，与列表型对象相似，但是列表中的每一项都是可以改写的，元组却不能改写。因此，
元组可以看作是“只读的”列表。元组的只读属性，意味着列表的大多数操作函数元组都不支持！

本节教程我们了解Python的元组型对象及其操作。


---------------------------------


定义tuple的方法
---------------------------------

tuple(元组)使用“()”将各项扩在一起，而列表使用的“[]”，切勿混淆！

使用USB数据项将BlueFi与电脑连接，并打开MU编辑器，
点击“串口”按钮，并按“ctrl+C”让BlueFi进入REPL模式，然后在“>>>”提示符后输入以下脚本代码并按“Enter”键执行对应代码，观察
执行结果。定义一个元组：

.. code-block::  python
  :linenos:

  >>> c1 = (120, 120, 10, 'RED')
  >>> c1
  (120, 120, 10, 'RED')
  >>> 

我们定一个包含有4个项目的元组c1，前三项都是数值，最后一项是字符串。这说明：元组跟列表相同，其中的数据项不必是相同类型的。
那么，我们自然会想：能否将一个列表转换为元组呢？答案是“可以的”！

将列表转换为元组: obj_tuple = tuple( list )。我们可以用下面的代码来验证：

.. code-block::  python
  :linenos:

  >>> l = [0,2,4,6,8]
  >>> t = tuple(l)
  >>> t
  (0, 2, 4, 6, 8)

首先使用“obj_list = [x, x, ..]”定义一个列表，obj_list是可以改写的，然后使用“obj_tuple = tuple( obj_list )”将列表
转换为元组，即只读的列表。

此时，你是否会猜想：元组可以转换为列表吗？答案仍然是“可以的”！譬如，接着前例执行下面代码：

.. code-block::  python
  :linenos:

  >>> lt = list(t)
  >>> lt
  [0, 2, 4, 6, 8]

变量lt是列表类型，将这个变量打印到屏幕上时使用“[]”将其中的数据项扩起来。当然，我们可以使用Python内置的函数“type()”来查询
某个变量的类型，用下面的代码，我们分别查询变量lt和t的类型：

.. code-block::  python
  :linenos:

  >>> type(lt)
  <class 'list'>
  >>> type(t)
  <class 'tuple'>
  >>> 

显然，变量lt和t分别为列表型和元组型。

.. Attention::  元组：只读的列表

  - 使用“tuple(obj_list)”将列表对象转换为元组，元组的各项与原列表各项完全相同，但元组的各项不能再改写
  - 使用“list(obj_tuple)”将元组对象转换为列表，列表的各项与原元组各项完全相同，但列表的各项允许改写


何时需要tuple?
-----------------------------

现在看起来元组与列表并无区别，只是元组的各项不允许改写，元组是只读的而已。我们既然有了列表，为什么还需要元组呢？

这是一个非常好的问题！

元组是只读的列表，既然列表可以搞定一切，何需元组？BlueFi的颜色传感器能够为我们提供三基色和亮度共4个光学通道的值，基于
这些值我们可以编写颜色识别算法感知当前对象的颜色；BlueFi的加速度传感器能够给出x-、y-、z-方向的加速度分量。BlueFi的
多种传感器输出的测量结果都包含有多个分量，将这些结果作为一个整体传递给一个接口函数时，最佳的数据类型就是使用元组！虽然
可以使用列表型，但列表不是最佳选择。

使用元组型变量来表示传感器输出的多分量数据，其意义在于元组的只读属性。对于传感器输出的结果，我们只能使用他们，不允许修改，
这样可以避免某些程序bug试图修改这些结果时会自动报错，因为这些代码违反元组型变量的使用规则。

现在我们来看一看BlueFi的传感器返回的数据，使用以下代码

.. code-block::  python
  :linenos:

  >>> from hiibot_bluefi.sensors import Sensors
  >>> sensors = Sensors()
  >>> sensors.color
  (1344, 1486, 957, 3432)
  >>> sensors.acceleration
  (-0.671186, -0.595813, -9.56889)

如果你不记得“Sensors”类的各个接口名称，可以在REPL模式下输入“sensors.”并按“Tab”键即可自动列举“Sensors”类的全部接口
名称。上面的代码中，我们看到颜色传感器“sensors.color”返回的结果是一个四项数据的元组(前三项分别是RGB三基色的分量，最后一项是亮度分量)，
加速度传感器“sensors.acceleration”返回的结果是一个三项数据的元组(分别是x-、y-、z-方向的加速度分量)。


访问tuple的某一项
-----------------------------

始终记住：tuple是只读的。访问时也只能使用只读的方法，我们将BlueFi的颜色传感器输入结果的最后一项取出来赋给变量brightness:

.. code-block::  python
  :linenos:

  >>> from hiibot_bluefi.sensors import Sensors
  >>> sensors = Sensors()
  >>> sensors.color
  (1344, 1486, 957, 3432)
  >>> brightness = sensors.color[3]
  >>> brightness
  3432

与列表相似，使用“obj_tuple[index]”来访问元组的某一项。

下面我们使用颜色传感器的亮度通道值来控制白光灯的亮度: 当环境光很亮的时候，白光灯亮度变暗；反之，环境光很暗时，白光灯亮度变得很亮。
这里环境光亮度从哪里获取呢？颜色传感器的亮度分量，即“sensor.color[3]”。示例代码如下：

.. code-block::  python
  :linenos:

  import time
  from hiibot_bluefi.basedio import PWMLED
  from hiibot_bluefi.sensors import Sensors
  sensors = Sensors()
  led = PWMLED()

  while True:
      c = sensors.color[3]
      print(c)
      led.white = 65535 - c
      time.sleep(0.1)

注意，元组与列表一样都是从第0项开始。


访问tuple的某些项(切片操作)
-----------------------------

这一点几乎与列表又是完全相同，支持以下几种切片：

  - [index]，取第index项，index从0开始
  - [:index]，截取第0～index-1项，即前index项组成的子元组
  - [n:m]，截取第n~m-1项，共m-n项，结果是一个子元组
  - [index:]，截取第index~最后一项，结果是一个子元组

我们只取BlueFi颜色传感器给出的RGB三基色分量时，可以使用“rgb = sensors.color[:3]”，变量rgb是一个三项元组。

tuple嵌套
-----------------------------

tuple(元组)允许包含tuple，即元组的嵌套。定义嵌套的元组使用“(())”嵌套即可，如下面示例：

.. code-block::  python
  :linenos:

  >>> t = ((262, 0.2), (294,0.2))
  >>> t
  ((262, 0.2), (294, 0.2))
  >>> t[0]
  (262, 0.2)
  >>> t[0][0]
  262
  >>> t[0][1]
  0.2

嵌套元组的访问遵循“[index1][index2]..”规则。事实上，如果使用中间变量，嵌套元组的访问几乎没有任何特殊之处。
我们用下面示例来了解嵌套元组的访问：

.. code-block::  python
  :linenos:

  import time
  import board
  import pulseio
  import digitalio

  a4_quarter = (440, 0.25)
  c4_half = (261, 0.5)
  notes = (a4_quarter, c4_half)

  def play_note(note):
      if note[0] != 0:
          pwm = pulseio.PWMOut(board.SPEAKER, duty_cycle = 0, frequency=note[0])
          pwm.duty_cycle = 0x7FFF
      time.sleep(note[1])
      if note[0] != 0:
          pwm.deinit()

  enSpk = digitalio.DigitalInOut(board.SPEAKER_ENABLE)
  enSpk.switch_to_output()
  enSpk.value = True

  play_note(notes[0])
  play_note(notes[1])

在函数play_note中，输入参数note是一个两项元组，第一项是基本音调的频率，第二项是播放该音调的时长。


tuple的遍历
-----------------------------

元组是只读的列表，遍历元组本身就是逐项读取元组，这是自然不过的事儿。遍历元组仍然使用

  - for  term  in tuple

程序结构。下面我们完善前一个示例，让喇叭播放一个简单的旋律，从而了解元组的遍历效果。示例代码如下：

.. code-block::  python
  :linenos:

  import time
  import board
  import pulseio
  import digitalio

  c4_half = (261, 0.5)
  d4_quarter = (293, 0.25)
  e4_half = (329, 0.5)
  f4_half = (349, 0.5)
  g4_quarter = (392, 0.25)
  a4_quarter = (440, 0.25)
  b4_half = (493, 0.5)

  notes = (c4_half, d4_quarter, e4_half, g4_quarter, a4_quarter, b4_half)

  def play_note(note):
      if note[0] != 0:
          pwm = pulseio.PWMOut(board.SPEAKER, duty_cycle = 0, frequency=note[0])
          pwm.duty_cycle = 0x7FFF
      time.sleep(note[1])
      if note[0] != 0:
          pwm.deinit()

  enSpk = digitalio.DigitalInOut(board.SPEAKER_ENABLE)
  enSpk.switch_to_output()
  enSpk.value = True

  for i in notes:
      play_note(i)

本示例程序的第6～12行代码定义了7个基本音调的频率和播放时长的元组，第9行代码将这些元组定义为一个更大的元组notes。
并在最后两行代码使用“for”程序结构来遍历元组notes，实现简单旋律的播放效果。

根据本示例的启发，你是否能设计出播放“生日快乐”、“两只老虎”或“天上星星亮晶晶”等旋律的Python脚本程序？

-----------------------------

.. admonition:: 
  总结：

    - tuple
    - 元组的定义
    - tuple( obj_list )：列表转换为元组
    - list( obj_tuple )：元组转换为列表
    - 访问元组中的某一项
    - 访问元组中的某些项：元组切片
    - 元组的嵌套及其访问
    - 元组的遍历
    - 元组的应用


