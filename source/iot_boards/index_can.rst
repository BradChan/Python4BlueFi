=========================
CAN总线扩展板及其应用
=========================

BlueFi开源板本身支持Radio(2.4G RF)、BLE(单模)、WiFi等无线通讯接口，不仅能够实现多个BlueFi之间的近距离无线互联，很容易实现个人局域网的应用场景，
还能通过无线路由器接入互联网实现万物互联的应用场景。BlueFi还支持经典的异步串行通讯(P0:RxD，P1:TxD)，使用交叉电缆实现近距离有线连接。
可以说，BlueFi开源板与生俱来的IoT功能已十分强大。

本向导涉及UART(异步串行通讯)、RS485/RS422和Modbus协议、CAN(2.0)总线、(100MBase)Ethernet等有线通讯接口及BlueFi的相关扩展板，
以及窄带物联网(NB-IoT)接口及其扩展板。这些接口或IoT功能拓展板都是基于BlueFi开源板的40-Pin金手指的应用，
使用BlueFi的40-Pin金手指扩展接口，未来我们还将实现ZigBee、LoRa、4G/5G宽带等无线网络接口，进一步丰富BlueFi的IoT应用。



.. toctree::
   :maxdepth: 1

   canbus/can_intro.rst
   canbus/can_protocol.rst
   canbus/can_app.rst







