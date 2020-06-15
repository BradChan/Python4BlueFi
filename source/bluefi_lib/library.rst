======================
Bluefi开源库及其下载
======================

BlueFi的开源库分为两种版本：1) mpy格式(压缩的二进制脚本格式)；2) py源码格式(Python源码，可读性好，占用更大存储空间)。
每一种版本都包含有开源库和示例程序，而且所有示例程序都是Python源码格式。

当前版本的BlueFi开源库压缩包的发布日期为：2020-6-12

点击此处 `下载最新版BlueFi开源库(mpy格式)压缩包`_  (建议使用这个压缩包，为了节约BlueFi有限的存储空间)

点击此处 `下载最新版BlueFi开源库(py源码格式)压缩包`_  (打算自行增减或修改BlueFi开源库的高级用户可以使用这个压缩包)

BlueFi开源库的每一个库文件都带有自己的版本编号，此处下载的

.. _下载最新版BlueFi开源库(mpy格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-mpy-20200612.zip
.. _下载最新版BlueFi开源库(py源码格式)压缩包: http://www.hibottoy.com:8080/static/install/micro/CircuitPython/HiiBot_BlueFi_CircuitPy/bluefi-circuitpython-library-5.x-py-20200612.zip

BlueFi是一个持续开发和更新的单板机，建议定期(至少每季度)检查此链接，下载最新版开源库压缩包，并使用本向导更新固件。

-------------------------------


关于BlueFi开源库格式
---------------------------

用上面下载链接所下载的文件是压缩包形式的BlueFi开源库文件，需要使用RAR或ZIP软件解压后方可使用。解压后你将会看到两个文件夹，即
“example”和“lib”文件夹，他们分别是Python源码格式的示例文件，以及库文件。库文件采用两种形式：节约存储空间的mpy格式和Python源码格式。

为什么要使用mpy文件格式？

你可以自行baidu或google等工具详细了解mpy文件格式。Python源码的脚本文件带有一些提高程序可读性的注释或调试的说明性信息，使用“import”导入
这样的Python源码模块时会明显增加内存开销。mpy是一种压缩的二进制格式的Python源码文件，除了没有可读性之外，去掉源码中的所有注释信息，只保留
有用的代码，并以二进制格式存储，大大地节约存储空间。因此，建议大家使用mpy格式的开源库，除非你打算自行修改库文件的源码，才有必要直接使用.py
格式的开源库。




