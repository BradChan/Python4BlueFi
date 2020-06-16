联网获取本地时间
==========================

前一个向导中我们已经认识了BlueFi的WiFi，并启动WiFi扫描周围AP(WiFi热点)。本向导帮助我们如何将BlueFi的WiFi连接
到周围的一个可用的AP，如果这个AP与互联网是连通的，我们就可以使用网络时间服务校准本地的日期和时间。

所谓可用的AP，BlueFi必须能够扫描到，而且需要你知道这个AP的密码。网络时间服务是一种公益性的网络服务，我国已经建有十余个开放性的网络时间
服务器。网络时间服务器使用网络时间服务协议(NTP, Network Time Protocol)向服务请求方提供标准的时间校准服务。NTP的诞生完全是为网络设备
提供时间同步服务，传统的计时器需要定期手工校准时间，譬如利用电台的半点或整点报时来手工校准，今天的大多数智能设备都能够连接到互联网，并使用
NTP服务器自动校准时间。

本向导的目的是回答两个问题：1) 如何让BlueFi连接到互联网？2) 如何使用NTP服务器校准本地计时器?

---------------------------

让BlueFi接入互联网
---------------------------

电脑、Pad、手机等设备可以通过WiFi与无线路由器连接，并通过无线路由器连接到互联网，我们就可以使用搜索引擎找到自己需要的信息或者观看视频等。
BlueFi几乎与智能手机或平板有完全相同的联网和使用网络的方法，因为BlueFi也有一个内置的“无线网络设备/网卡”。

根据前一个向导的经验，如果BlueFi的WiFi能够扫描到周围的一些AP，而且你已经知道其中某些AP的密码，我们下一步就是将这个AP的名称和密码告诉
BlueFi，并编程让BlueFi连接到这个AP。如果这个AP与互联网是连通的，我们的BlueFi即可通过这个AP接入互联网。

如何将AP的名称（ssid)和密码(password)告诉BlueFi呢? 我们有两种方法：

  - 第1种方法是将这个AP的ssid和password两个字符串分别保存在“/CIRCUITPY/secrets.py”文件的对应位置，BlueFi需要联网时自动去这个文件中读取这些信息
  - 第2种方法是将这个AP的ssid和password两个字符串直接用Python程序接口传给BlueFi的网卡

虽然两种方法是等价的，建议使用第1种方法，即便是你分享自己的代码给其他人时，你的AP信息不会泄露给别人。secrets.py文件的格式如下：

.. code-block::  
  :linenos:

  secrets = {
    "ssid": "your_ap_name",
    "password": "your_ap_password",
    "timezone": "Asia/Shanghai", 
    "broker": 'www.hiibotiot.com',
    "user": 'your_iot_name',
    "pass": 'your_iot_password'
  }

这是一个JSON格式化的文本型“key:value”信息对，也可以用Python字典型数据结构来访问。每一个“:”前的字符串是“key”，“:”后的字符串是这个“key”
对应的“value”。

下面我们使用第2种方法设计一个让BlueFi连接到互联网的程序示例。程序代码如下：

.. code-block::  
  :linenos:

  from hiibot_bluefi.wifi import WIFI
  wifi=WIFI()

  while not wifi.esp.is_connected:
      try:
          wifi.esp.connect_AP(b"your_ap_name", b"your_ap_password")
      except RuntimeError as e:
          print("could not connect to AP, retrying: ", e)
          continue
  print("Connected to", str(wifi.wifi.ssid, "utf-8"), "\tRSSI: {}".format(wifi.wifi.signal_strength) )
  print("My IP address is {}".format(wifi.wifi.ip_address()))

  wifi.esp.reset()

  while True:
      pass

本示例的前2行程序的作用分别是，从“/CIRCUITPY/lib/hiibot_bluefi/”文件夹的“wifi.py”模块种导入“WIFI”类Python接口，并实例化
为“wifi”名称。然后我们就可以引用“wifi”使用BlueFi的无线WiFi网卡。

第4～9行是一个条件循环，检测条件是“wifi.esp.is_connected”，当BlueFi的WiFi连接到AP后“wifi.esp.is_connected”被置为True。显然，
如果WiFi未连接到AP，这个条件是成立的，则执行第6行程序，即连接指定的AP(指定该AP的名称和密码)，如果连接失败就用BlueFi的LCD显示屏和
串口控制台输出“could not connect to AP, retrying: ('Failed to connect to ssid', b'your_ap_name')”，并再次返回第6行尝试
再次连接，如此重复。如果你给的AP名称和密码有任何错误，将导致第4～9行程序成为一个死循环。根据提示信息，你就可以掌握循环的次数。

一旦成功地连接到指定的AP，BlueFi将输出提示信息并输出自己的IP地址。

由于本示例仅仅是测试联网，当程序执行到第13行时，我们关闭BlueFi的WiFi以节电。相比较其他功能单元，BlueFi的WiFi属于高耗能，使用之后应
及时关闭。


用互联网同步本地日期和时间
---------------------------




