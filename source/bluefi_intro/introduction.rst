介绍和技术参数
====================

BlueFi是一种硬件接口完全兼容microbit的全新单板机，拥有ARM CM4F高性能计算能力，
板载BlueTooth5和WiFi无线通讯接口，满足AI边缘计算的需求。兼容microbit硬件接口，
你可以使用丰富的microbit周边硬件单元快速搭建自己的创意原型作品。全新设计的硬件系
统和编程软件接口，让BlueFi不仅可以为基础教育服务，还适合Python、C/C++等编程语言
学习者使用，也适合工业级原型产品搭建。

BlueFi采用Nordic的nRF52840(64MHz ARM Cortex-M4F)作为主控，不仅拥有强大的
计算能力，能运行Google Tensor Flow Lite等机器学习软件库，还具备业界领先的
BlueTooth无线通讯方案。同时，BluFi采用上海乐鑫(Espressif)的ESP32(240MHz 
Tensilica LX6双核处理器)作为网络协处理器，专门处理WiFi无线通讯事务，支持
http/https和MQTT等物联网(IoT)应用协议。

.. Attention::

  BlueFi出厂时，网络协处理器已经具有网络事务处理的固件，用户无需对ESP32编程

.. image:: /../_static/images/BlueFi_fore.png
  :scale: 30%
  :align: center

BlueFi技术参数
-----------


硬件特性:

  - 主控CPU (Nordic nRF52840)
  
    - 64MHz ARM Cortex-M4F内核
    - 1MB FlashROM, 256KB SRAM
    - 1x UAB2.0, 4x SPI, 2x I2C, 2x UART, 2x I2S
    - ARM® TrustZone® Cryptocell 310 security subsystem
    - Bluetooth® 5, IEEE 802.15.4-2006, 2.4 GHz transceiver
    - 开启蓝牙且CPU全速运行时消耗电流: 26mA

  - 主控CPU外扩2MB FlashROM用作SPI文件系统

  - 网络协处理器 (Espressif ESP32)

    - 240MHz Tensilica LX6双核处理器
    - 4MB FlashROM, 520KB SRAM
    - 采用SPI从模式与主控CPU连接
    - 执行WIFI 802.11 b/g/n(2.4 GHz), 速度高达150 Mbps
    - 内置固件支持http/https和MQTT等IoT协议, 支持Web client/Sevser应用
    - 开启WIFI且CPU全速运行时消耗电流: 180mA

  - 板载传感器和输入

    - 2x 轻触按钮输入(兼容microbit)
    - 3x 触摸盘输入(兼容microbit)
    - 3-DoF/3-axis 加速度计(兼容microbit)
    - 3-DoF/3-axis 陀螺仪
    - 3-DoF/3-axis 地磁计(兼容microbit)
    - CPU芯片温度(兼容microbit)
    - 数字型环境温度和湿度计
    - 数字型MEMS声音传感器
    - 集成光学传感器(环境光亮度, 接近感知, 手势识别和颜色识别)

  - 板载显示屏和输出

    - 1.3" TFT-LCD (IPS 240x240点阵)显示器
    - 5x NeoPixel RGB彩灯
    - 1x 喇叭和D类音频放大器
    - 1x 可编程的红色LED 
    - 1x 可编程的高亮度白色LED(可用作颜色识别辅助照明)

  - BlueFi供电单元

    - Micro USB供电(5V)
    - 3.7V锂电池供电(具备2mm电池插座)
    - 金手指扩展接口供电(3.3V)
    - 板载高效的开关型DC-DC单元, 可为扩展接口的负载供电(最大输出电流1.5A)
    - 板载3.7V锂电池充放电管理单元(充电电流达500mA, 支持最大充电电压4.2V的锂电池)

  - 拓展接口

    - 40-Pin金手指(兼容microbit, 含19个独立可编程I/O引脚)
    - mini-Grove接口(4-Pin, 1mm间距)
    - 所有拓展接口引脚均支持：DI/DO/PWM/I2C/I2S/SPI/UART等模式
    - 7个拓展引脚支持模拟输入(A0~A6)

  - 电源指示灯(绿色)
  - 充电指示灯(红色)
  - 协处理器子系统电源独立可控(关闭WIFI电源和RGB彩灯时整板功耗小于40mA)
  - 功耗：关闭WiFi、彩灯、喇叭时不超过40mA，开启全功能时不小于600mA
  - 外型尺寸: 52x43x9mm

软件特性:

  - 支持Scratch图形化编程语言
    - 出厂时默认支持的编程语言
    - 在线编程模式, Scratch3.0环境
    - 打开Chrome浏览器即开始编程，无需安装任何软件 
      - 易造云平台
      - https://www.ezaoyun.com/
    - 在线下载(保存code.py文件到/CIRCUITPY/磁盘即为下载)
    - 升级固件(拖放文件的操作模式)
      - 下载最新版固件(点击此处下载文件)
      - 双击Reset按钮, 等待所有彩灯变为绿色, 出现BLUEFIBOOT磁盘
      - 将固件文件拖放到BLUEFIBOOT磁盘
  
  - 支持Python脚本编程语言
    - 出厂时默认支持的编程语言
    - 推荐使用开源的MU作为软件工具
      - MU网址
      - https://codewith.mu/
    - Pycom， Visual Studio等代码编程软件
    - 使用任意文本编辑器编写py代码, 保存为code.py文件, 拖放到/CIRCUITPY/即为下载
    - 升级固件(拖放文件的操作模式)
      - 下载最新版固件(点击此处下载文件)
      - 双击Reset按钮, 等待所有彩灯变为绿色, 出现BLUEFIBOOT磁盘
      - 将固件文件拖放到BLUEFIBOOT磁盘
  
  - 支持C/C++编程语言
    - 双击Reset按钮, 出现BLUEFIBOOT磁盘, 即进入该模式
    - 推荐使用Arduino IDE作为软件工具
      - Arduino IDE的下载和使用说明
      - https://www.arduino.cc 
    - 点击此处进入详细的编程向导







