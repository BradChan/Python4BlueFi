======================
麦昆小车(Maqueen)
======================

麦昆小车(Maqueen)由DFRobot推出的智能小车底盘，属于microbit周边的小车底盘类，包含以下资源：

  - 1x 超声波避障传感器，使用P1(Trig)和P2(Echo)
  - 2x 光电反射型循迹传感器，使用P13(leftTrackSensor)和P14(rightTrackSensor)
  - 2x 红色LED头灯，使用P8(leftHeadLED)和P8(rightHeadLED)
  - 4x 底盘像素彩灯(兼容WS2812B)，使用P15
  - 2x 直流有刷电机(N20型)，使用IIC接口(IIC从地址：0x10)，左电机控制(寄存器地址为0)，右电机控制(寄存器地址为2)
  - 2x 舵机控制接口，S1(寄存器地址为20)和S2(寄存器地址为21)
  - 1x 红外遥控器输入(BlueFi暂不支持红外遥控接收)

---------------------------------

BlueFi+MaQueen=智能小车，使用方法方面几乎与使用microbit+MaQueen一样地容易，但功能方面强大很多：BlueFi具有丰富的传感器很容易
实现更多种交互，BlueFi的BlueTooth、WiFi等

.. image::  ../../_static/images/peripheral/maqueen_car.jpg
  :scale: 25%
  :align: center






.. admonition::  麦昆(MaQueen)接口库总结：

  - 导入麦昆小车库：from  hiibot_maqueen import MaQueen
  - 实例化麦昆小车：car = MaQueen()
  - 使用麦昆小车的传感器：

    - “超声波避障/测距”传感器当前的障碍物距离：dist = car.distance 

      - 有效值为2~4000mm，超出此范围都属于异常
      
    - “左循迹”传感器的状态：ls = car.leftTrackSensor

      - True/1，表示有反射信号
      - False/0，表示无反射信号

    - “右循迹”传感器的状态：ls = car.rightTrackSensor

      - True/1，表示有反射信号
      - False/0，表示无反射信号
    
  - 麦昆小车的输出控制类：

    - “左头灯”亮： car.leftHeadLED = 1
    - “左头灯”灭： car.leftHeadLED = 0
    - “右头灯”亮： car.rightHeadLED = 1
    - “右头灯”灭： car.rightHeadLED = 0
    - “底盘彩灯”控制： car.pixels[i] = (255,0,0), car.pixels.show()。详细方法请参考NeoPixel类的用法
    - “舵机”控制，舵机S1转到90度： car.servo(0, 90) 
    - “舵机”控制，舵机S2转到120度： car.servo(1, 120) 
    - “小车双电机”控制，小车以50的速度前进： car.motor(50, 50)
    - “小车双电机”控制，小车以80的速度后退： car.motor(-80, -80)
    - “小车双电机”控制，小车以40的速度绕自身中心左转(原地左转)： car.motor(-40, 40)
    - “小车双电机”控制，小车以100的速度绕自身中心右转(原地右转)： car.motor(100, -100)
    - “小车双电机”控制，小车以60的速度绕右轮左转： car.motor(60, 0)
    - “小车双电机”控制，小车以60的速度绕右轮右转： car.motor(-60, 0)


