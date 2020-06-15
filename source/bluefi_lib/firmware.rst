=========================
Bluefi固件
=========================


点击此处 `下载最新版BlueFi固件`_ 当前版本编号：5.4.0。

.. _下载最新版BlueFi固件: http://www.hibottoy.com:8080/static/install/micro/BlueFi_firmware_5.0.uf2

BlueFi是一个持续开发和更新的单板机，建议定期(至少每季度)检查此链接，下载最新版固件，并使用本向导更新固件。

下载最新版固件并保存到自己电脑上备份，默认的BlueFi固件文件名为“BlueFi_firmware_x.x.uf2”，其中“x.x”代表当前下载的
固件版本号。注意，该文件采用“uf2”格式，扩展名为“.uf2”。请记住保存该文件的文件夹位置。

-------------------------

如何找到BlueFi的固件版本?
-------------------------

当你使用USB数据线将BlueFi与电脑正确链接后，电脑的资源管理器中将出现一个名为“CIRCUITPY”的可卸载磁盘(相当于U盘)。展开这个磁盘
的文件列表，查看“boot_out.txt”文件，你将会找到BlueFi当前所用固件的版本号。譬如，下图的这个示例。

.. image::  ../../_static/images/bluefi_lib/bluefi_firmver.jpg
  :scale: 40%
  :align: center

(上图示例使用macOS电脑，其他OS系统的情况或有差别)

---------------------------

更新BlueFi固件
---------------------------

任何时候，你只需要双击BlueFi的“RESET”按钮，BlueFi都将自动侦测与电脑的连接状态，如果与电脑是可靠连接的，BlueFi的全部彩灯立即
切换为低亮度绿色(这个现象十分地明显)，且红色LED处于慢呼吸状态(亮度渐灭再渐亮地循环)，这些现象表示：BlueFi已经进入Boot模式。同时，
电脑上出现一个名为“BLUEFIBOOT”的可下载磁盘(相当于U盘)。

将前面下载好的固件文件“BlueFi_firmware_x.x.uf2”直接拖放到“BLUEFIBOOT”磁盘即可。如果你习惯用“ctrl+c”和“ctrl+v”快捷键来
复制电脑上下载好的BlueFi固件并粘贴到“BLUEFIBOOT”磁盘，操作效果完全相同。

将固件复制到“BLUEFIBOOT”磁盘即为更新BlueFi固件！

.. Caution::  更新固件期间(仅数十秒)切勿断电!

一旦更新好固件之后，BlueFi自动执行软复位，开始执行新固件。你的电脑上将出现一个名为“CIRCUITPY”磁盘，并执行你的“code.py”程序。

.. Caution::  更新固件并不会影响“CIRCUITPY”磁盘上的内容

  - BlueFi的“CIRCUITPY”磁盘是一个专用的FlashROM空间，与固件是相互独立的，因此更新固件不会影响“CIRCUITPY”磁盘的内容
  - BlueFi的“CIRCUITPY”磁盘用于存放你的程序文件——code.py，以及开源库、图片、声音、字库等资源文件，升级固件时不会影响这些文件
  - BlueFi的“CIRCUITPY”磁盘上的文件使用电脑资源管理器来维护，跟电脑上其他文件一样地使用

