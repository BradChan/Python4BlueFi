用按钮给计算机发指令
======================

按钮(Button)是计算机系统最简单的一种输入设备，计算机键盘上有100个左右的按键(不同键盘的按键个数不同)，每一个按键就是一个按钮，
每一个按钮赋予惟一的编号，如A按键的编号为0x41，B按键的编号为0x42，当我们按下按键的按钮时，键盘将对应按键的惟一编号发送给电脑
主机，实现人-机交互的输入。

绝大多数嵌入式系统的按键输入比较少，如电梯召唤按钮、轿箱内楼层按钮等都仅有几个或几十个按钮，这些按钮用于人向计算机系统发出指令，
按下电梯某楼层的召唤按钮告知电梯控制的计算机系统我需要乘坐电梯，电梯系统收到召唤后即让召唤按钮背后指示灯亮起，表示收到召唤，启动
响应。

BlueFi具有两个按钮，分别称作A按钮和B按钮，分别位于LCD显示屏的左右两侧，我们可以使用这两个按钮向BlueFi发指令，譬如开启白光灯、
关闭白光灯等。

--------------------------

按钮输入状态
--------------------------

按钮仅有两个状态：被按下和未被按下。如果被按下时的状态记为“1”，未被按下的状态记为“0”。也可以使用逻辑变量值来表示，按下时的状态记为
“True”，未被按下时的状态记为“False”。

我们使用以下程序，将BlueFi上按钮的状态显示在屏幕(和控制台)上：

.. code-block::  python
  :linenos:

  import time
  from hiibot_bluefi.basedio import Button
  button = Button()
  while True:
      print("A:{:d} B:{:d}".format(button.A, button.B))
      time.sleep(0.5)

你将会看到LCD屏幕(和控制台)上看到：A:0 B:0，当你按下A按钮时会显示：A:1 B:0。两个“:”后面分别是A和B按钮的状态，按下按钮时对应的
状态为1，否则为0。

  示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/basedio.py”模块中导入Button类
    - 第3行，将导入的“Button”类实例化为一个实体对象，名叫“button”
    - 第4行，一个无穷循环的程序块
    - 第5行(无穷循环程序块的第1行)，将按钮A和B的状态值格式化显示到LCD屏幕(控制台)上
    - 第6行(无穷循环程序块的第2行)，执行time的sleep方法，参数为0.5秒

第5行程序是重点，将变量值val格式化为一个字符串："{:d}".format(val)。其中，“{}”内的“:d”表示将val值显示十进制形式。
print("hello")是将字符串“hello”显示在LCD屏幕(控制台)上。如果将本示例程序稍作修改，你将会看到另外一种显示效果：

.. code-block::  python
  :linenos:

  import time
  from hiibot_bluefi.basedio import Button
  button = Button()
  while True:
      print("A:{} B:{}".format(button.A, button.B))
      time.sleep(0.5)

修改后的程序只是去掉“{}”内的“:d”，即使用默认的格式化(系统根据变量val的类型自动决定格式化的输出形式)。没有按下任何按钮时，
你将会看到LCD屏幕(控制台)上显示：A:False B:False。按下A按钮时，你会看到：A:True B:False。这样的显示结果，说明A和B
按钮状态的变量值是逻辑型的。


用按钮开关白色LED
--------------------------

下面这个示例，我们使用按钮B被按下时切换白色LED的亮和灭：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import Button, LED
    button = Button()
    led = LED()
    while True:
        button.Update()
        if button.B_wasPressed:
            led.whiteToggle()

运行本示例程序时，你会发现程序的效果：每按下B按钮一次，白色LED状态就被切换一次。这个效果像是一个被轻触开关控制的照明灯。

  示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/basedio.py”模块中导入Button和LED两个类
    - 第3行，将导入的“Button”类实例化为一个实体对象，名叫“button”
    - 第4行，将导入的“LED”类实例化为一个实体对象，名叫“led”
    - 第5行，一个无穷循环的程序块
    - 第6行(无穷循环程序块的第1行)，更新A和B按钮的状态
    - 第7行(无穷循环程序块的第2行，判断条件为True时的程序块)，判断B按钮是否已被按下
    - 第8行(无穷循环程序块的第3行，条件为True时的程序块的第1行)，如果B按钮已被按下，切换白色LED的状态

第7行和第8行是一个简单的逻辑判断和逻辑程序块，当“button.B_wasPressed”为True时，执行“led.whiteToggle()”。


用按钮调节白色LED的亮度
--------------------------

我们将上面的程序稍作修改，即可实现“使用B按钮增加白色LED的亮度，使用A按钮减小白色LED的亮度”：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import Button, PWMLED
    button = Button()
    led = PWMLED()
    b=32700
    while True:
        led.white=b
        button.Update()
        if button.B_wasPressed:
            b += 10000
        if button.A_wasPressed:
            b -= 10000
        if b<0:
            b=0
        if b>65535:
            b=65535

在BlueFi上运行本示例程序，试一试按下按钮A或B，你将观察到白色LED的亮度变化。

  示例代码分析：

    - 第1行，导入一个Python内建的模块“time”
    - 第2行，从“/CIRCUITPY/lib/hiibot_bluefi/basedio.py”模块中导入Button和PWMLED两个类
    - 第3行，将导入的“Button”类实例化为一个实体对象，名叫“button”
    - 第4行，将导入的“PWMLED”类实例化为一个实体对象，名叫“led”
    - 第5行，声明一个变量b，并赋初始值为32700
    - 第6行，一个无穷循环的程序块
    - 第7行(无穷循环程序块的第1行)，用变量b的值更新白色LED的亮度
    - 第8行(无穷循环程序块的第2行)，更新A和B按钮的状态
    - 第9行(无穷循环程序块的第3行，判断条件为True时的程序块)，判断B按钮是否已被按下
    - 第10行(无穷循环程序块的第4行，条件为True时的程序块的第1行)，如果B按钮已被按下，变量b的值增加10000
    - 第11行(无穷循环程序块的第5行，判断条件为True时的程序块)，判断A按钮是否已被按下
    - 第12行(无穷循环程序块的第6行，条件为True时的程序块的第1行)，如果A按钮已被按下，变量b的值减少10000
    - 第13行(无穷循环程序块的第7行，判断条件为True时的程序块)，判断变量b的值是否小于0
    - 第14行(无穷循环程序块的第8行，条件为True时的程序块的第1行)，如果变量b的值小于0，让变量b的值等于0
    - 第15行(无穷循环程序块的第9行，判断条件为True时的程序块)，判断变量b的值是否大于65535
    - 第16行(无穷循环程序块的第10行，条件为True时的程序块的第1行)，如果变量b的值大于65535，让变量b的值等于65535

本示例程序的最后4行非常重要，目的是确保变量b的值必须是在0~65535之间，如果小于0则等于0(亮度不能再小啦)，如果
大于65535则等于65535(亮度不能更大啦)。这是因为，变量b的实际意义是白色LED的亮度，取值范围只能是0~65535。

你可以删除最后的4行程序，试一试效果，如果出现错误而终止程序运行时，你将看到错误提示，根据错误信息推断问题的原因。


按钮的短按和长按
--------------------------

当你一直按着桌面计算机的某个按键时，相当于快速输入很多个相同的字母或数字，BlueFi的按钮也有相同的效果吗？

为了验证这一设想，我们可以修改前一个示例程序，如果发现长按A按钮时则直接让变量b的值变为0(最小亮度)，如果长按A按钮时则直接
让变量b的值变为65535(最大亮度)。修改后的程序如下：

.. code-block::  python
  :linenos:

    import time
    from hiibot_bluefi.basedio import Button, PWMLED
    button = Button()
    led = PWMLED()
    b=32700
    while True:
        led.white=b
        button.Update()
        if button.B_wasPressed:
            b += 10000
        if button.A_wasPressed:
            b -= 10000
        if b<0:
            b=0
        if b>65535:
            b=65535
        if button.A_pressedFor(2):
            b=0
        if button.B_pressedFor(2):
            b=65535

修改后的程序仅仅增加最后的4行，即第17～20行。第17行是条件判断，条件是按钮A是否已按下超过2s？如果条件成立则执行第18行，让变量b等于0。
第19行仍是条件判断，条件是按钮B是否已按下超过2s？如果条件成立则执行第20行，让变量b等于65535。其他程序语句与前一示例程序完全相同，
此处不再赘述。

请在BlueFi上测试本示例，检验程序的执行效果是否达到设想：长按A按钮，白色LED亮度变为0；长按B按钮，白色LED亮度变为最大(即65535)。
然后试一试修改第17和第18行的长按时间参数，观察执行效果，并思考为什么是这样的效果。


.. admonition:: 
  总结：

    - PWM信号
    - PWM信号的周期、频率和占空比
    - 实体对象的属性赋值
    - 变量
    - 变量赋值
    - 变量自增/自减
    - 本节中，你总计完成了14行代码的编写工作


.. Important::
  **Button类的接口**

    - A (属性, 只读, 有效值：0 或 1), FlueFi的A按钮状态
    - B (属性, 只读, 有效值：0 或 1), FlueFi的B按钮状态
    - A_wasPressed (属性, 只读, 有效值：0 或 1), FlueFi的A按钮已被按下
    - B_wasPressed (属性, 只读, 有效值：0 或 1), FlueFi的B按钮已被按下
    - A_wasReleased (属性, 只读, 有效值：0 或 1), FlueFi的A按钮已被释放
    - B_wasReleased (属性, 只读, 有效值：0 或 1), FlueFi的B按钮已被释放
    - A_pressedFor (函数, 输入参数: 时长, 返回值:0 或 1), BlueFi的A按钮是否被长按超过指定的时长
    - B_pressedFor (函数, 输入参数: 时长, 返回值:0 或 1), BlueFi的B按钮是否被长按超过指定的时长
    - Update (函数, 无参数, 无返回值), 更新BlueFi的两个按钮的状态, 必须放在循环体内调用