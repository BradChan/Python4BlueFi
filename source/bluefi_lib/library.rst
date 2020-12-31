======================
Bluefi开源库及其下载
======================

BlueFi的开源库分为两种版本：1) mpy格式(压缩的二进制脚本格式，源码不可见，占用很小的存储空间)；
2) py源码格式(Python源码，可读性好，占用更大存储空间)。
每一种版本都包含有开源库和示例程序，而且所有示例程序都是Python源码格式。

---------------------------------------------

当前版本的BlueFi开源库压缩包的发布日期为：2020-12-31

点击此处 `下载最新版BlueFi开源库(mpy格式)压缩包`_  (建议使用这个压缩包，为了节约BlueFi有限的存储空间)

点击此处 `下载最新版BlueFi开源库(py源码格式)压缩包`_  (打算自行增减或修改BlueFi开源库的高级用户可以使用这个压缩包)

此处下载的BlueFi开源库的每一个库文件都带有自己的版本编号，文件名中有明确地标注。

.. _下载最新版BlueFi开源库(mpy格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-mpy-20201231.zip
.. _下载最新版BlueFi开源库(py源码格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-py-20201231.zip

BlueFi是一个持续开发和更新的单板机，建议定期检查此链接，下载最新版开源库压缩包，并使用本向导更新固件。

.. image::  ../../_static/images/bluefi_lib/download_lib.gif
  :scale: 20%
  :align: center


.. admonition::  更新说明：

  - (2020.12.31)
  - 与CircuitPython官网同步更新(2020.12.31)
  - 增加CANBus(使用MCP2515控制器)接口库(hiibot_mcp2515文件夹)和几个应用示例(在examples文件夹中查找hiibot_mcp2515_can*.py)
  - 增加NB-IoT(使用SIM7020X模块)接口库(hiibot_sim7020x文件夹)和MQTT的应用示例(在examples文件夹中查找hiibot_sim7020x*.py)
  - 增加HiiBot IoTs2(使用ESP32-S2)BSP库以及多个应用示例，IoTs2支持WiFi、8MB FlashROM和8MB pSRAM，带有3轴加速度传感器、1.1寸TFT-LCD(135*240点阵)
  - 增加HiiBot Circle(编程圆)的BSP库以及多个应用示例，Circle带有7个可触摸输入、2个按钮输入、光强度和颜色识别传感器、环境温度传感器、3轴加速度传感器、MEMS数字麦克风(PDM)、10颗可编程彩灯、喇叭等资源
  - (2020.11.2)
  - 与CircuitPython官网同步更新(2020.11.2)
  - 增加HiiBot JoyStick接口库，JoyStick带有一个模拟型2D摇杆(x, y)输入、4个按钮、1个振动马达等资源
  - 增加DFRobot的麦昆(maqueen)接口库(hiibot_maqueen.py)和2个应用示例(在examples文件夹中查找maqueen_corral.py和maqueen_trackline.py) 
  - 完善LSM6DS3库版本的兼容问题，Adafruit升级的库(2.1)不支持计步器功能，重新完善

---------------------------------------------

2020-11-2发布的版本

点击 `下载2020-11-2版BlueFi开源库(mpy格式)压缩包`_  (建议使用这个压缩包，为了节约BlueFi有限的存储空间)

点击 `下载2020-11-2版BlueFi开源库(py源码格式)压缩包`_  (打算自行增减或修改BlueFi开源库的高级用户可以使用这个压缩包)

.. _下载2020-11-2版BlueFi开源库(mpy格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-mpy-20201102.zip
.. _下载2020-11-2版BlueFi开源库(py源码格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-py-20201102.zip

---------------------------------------------

2020-6-21发布的版本

点击 `下载2020-6-21版BlueFi开源库(mpy格式)压缩包`_  (建议使用这个压缩包，为了节约BlueFi有限的存储空间)

点击 `下载2020-6-21版BlueFi开源库(py源码格式)压缩包`_  (打算自行增减或修改BlueFi开源库的高级用户可以使用这个压缩包)

.. _下载2020-6-21版BlueFi开源库(mpy格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-mpy-20200621.zip
.. _下载2020-6-21版BlueFi开源库(py源码格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-py-20200621.zip

-------------------------------


2020-6-12发布的版本

点击 `下载2020-6-12版BlueFi开源库(mpy格式)压缩包`_  (建议使用这个压缩包，为了节约BlueFi有限的存储空间)

点击 `下载2020-6-12版BlueFi开源库(py源码格式)压缩包`_  (打算自行增减或修改BlueFi开源库的高级用户可以使用这个压缩包)

.. _下载2020-6-12版BlueFi开源库(mpy格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-mpy-20200612.zip
.. _下载2020-6-12版BlueFi开源库(py源码格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-py-20200612.zip

-------------------------------

关于BlueFi开源库格式
-------------------------------

用上面下载链接所下载的文件是压缩包形式的BlueFi开源库文件，需要使用RAR或ZIP软件解压后方可使用。解压后你将会看到两个文件夹，即
“example”和“lib”文件夹，他们分别是Python源码格式的示例文件，以及库文件。库文件采用两种形式：节约存储空间的mpy格式和Python源码格式。

为什么要使用mpy文件格式？

你可以自行baidu或google等工具详细了解mpy文件格式。Python源码的脚本文件带有一些提高程序可读性的注释或调试的说明性信息，使用“import”导入
这样的Python源码模块时会明显增加内存开销。mpy是一种压缩的二进制格式的Python源码文件，除了没有可读性之外，去掉源码中的所有注释信息，只保留
有用的代码，并以二进制格式存储，大大地节约存储空间。因此，建议大家使用mpy格式的开源库，除非你打算自行修改库文件的源码，才有必要直接使用.py
格式的开源库。


关于BlueFi开源库管理
-------------------------------

BlueFi的开发和维护仍在持续中，开源库与固件存在一定耦合关系，固件和开源库的升级迭代中仍保留早期的版本供使用者下载，这些资源都将保留在本
页面，因此本向导或许会变得很长，但最新版的始终保持在最上面，方便查找。

开源库文件名的规则：开源库名称-版本-格式-打包发布日期.zip，如“bluefi-circuitpython-library-5.x-mpy-20200612.zip”表示BlueFi的
circuitpy-library，版本为5.x，格式为mpy，打包发布日期为20200612。注意，BlueFi的开源库分两种：mpy格式和py格式。

版本号说明：BlueFi的开源库压缩包标记的版本，譬如5.x，这个版本与固件保持一致，譬如固件版本为5.4.0，意味着5.x的开源库完全适用。但是，随着
固件和开源库的迭代，固件6.4.0就无法使用5.x的开源库，建议那个时候自行下载6.x版本的开源库。

BlueFi的每一个开源库基本保持独立，虽然也有很多库依赖其他Python模块，都会在该库的源码注释文件中说明，而且每一个开源库的内部版本也是独立的，
与BlueFi开源库压缩包所标记的版本号并无直接关系，我们只是根据适用的固件版本对其编号。每一个开源库的内部版本都已经明确地在源码文件中注明，
如果需要确认每一个单独开源库的版本，请下载py源码格式的库压缩包，解压后直接在lib文件夹中找到对应库的源码文件，用文本编辑器查看对应版本号。


解压BlueFi开源库
-------------------------------

请使用RAR或ZIP软件解压所下载的BlueFi开源库压缩包，解压后的文件夹如下图：

.. image::  ../../_static/images/bluefi_lib/unzip_lib.jpg
  :scale: 50%
  :align: center



如何使用BlueFi开源库，请点击“Next”按钮进入下一个向导。

本向导的总结如下：

.. admonition::  下载BlueFi开源库的步骤：

  - step1: 选择适合自己的库格式(mpy或py)，点击本页面上方的下载链接，并保存固件文件到电脑本地磁盘
  - step2: 使用RAR或ZIP软件将下载到本地的BlueFi开源库压缩包解压到指定位置，文件夹中包含有/examples和/lib两个子文件夹

    - examples文件夹中包含有BlueFi全部的Python源码示例文件
    - lib文件夹中包含有BlueFi全部的Python开源库，这些库文件或文件夹可以直接拖放至BlueFi的/CIRCUITPY/lib/文件使用
