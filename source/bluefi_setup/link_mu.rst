连接BlueFi和MU编辑器
====================

绝大多数情况，将BlueFi连接到宿主计算机系统都是为了更新/下载程序。与其他嵌入式系统或单板机不同的时，当你使用USB数据线
将BlueFi插入自己的电脑时，BlueFi是一个“移动磁盘”。根据BlueFi的工作状态，这个可移动磁盘使用两个固定的名称：BLUEFIBOOT和
CIRCUITPY。


.. Attention:

  - BlueFi使用常用的Micro USB数据线与电脑连接
  - 很多设备使用Micro USB接口供电。因此市面上很多USB电源线，他们并不是数据线！此类电源线无法让BlueFi与电脑连接
  - 验证BlueFi是否与电脑可靠连接的方法就是，检查电脑的资源管理器是否出现BLUEFIBOOT或CIRCUITPY磁盘

-------------------------------------

我们会在以下几种情况使用BlueFi磁盘：

  - 更新BlueFi Bootloader

    - 双击Reset按钮，当BlueFi所有彩灯变为绿色时，出现BLUEFIBOOT磁盘
    - 下载最新的BlueFI Bootloader固件(点击此处下载)
    - 拖放BlueFI Bootloader固件到BLUEFIBOOT磁盘
  
  - 更新BlueFi固件(Python脚本解释器)

    - 双击Reset按钮，当BlueFi所有彩灯变为绿色时，出现BLUEFIBOOT磁盘
    - 下载最新的BlueFi固件(点击此处下载)
    - 拖放BlueFi固件(Python脚本解释器)到BLUEFIBOOT磁盘
  
  - 更新用户程序

    - 将BlueFi插入电脑USB端口，出现CIRCUITPY磁盘
    - 将用户程序文件(必须使用main.py或code.py)拖放至CIRCUITPY磁盘

-------------------------------------

如果系统未按预期出现相应的磁盘，请首先检查USB数据线和连接的可靠性。如果BlueFi的电源指示灯(BlueFi正面的左上角的绿色LED)都
不能正常工作，请更换到其他USB端口，电脑的某些USB端口或许已经不可靠的或损坏！

-------------------------------------

1. 当BlueFi与MU编辑器成功连接时
----------------------------------

当BlueFi通过USB数据线与宿主电脑成功连接时，


2. CIRCUITPY磁盘
----------------------------------



3. 问题及其解决
----------------------------------



