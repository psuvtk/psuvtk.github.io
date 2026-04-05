# MCU裸机编程的代码架构


MCU裸机开发常用的系统架构:

1、轮训模式
对外部事件进行轮训

2、前后台模式
中断处理外部时间，后台循环处理其他事务

3、定时器模式
采用定时器来管理

时间基准：
1) 框架调用用户注册的回调获取 TICK
2) 用户(中断)周期性回调框架接口增加TICK计数

MultiTimer
https://github.com/0x1abin/MultiTimer


4、分级状态机、状态机
顶层状态机判断系统忙闲状态 做节能处理；

---

> Author: Kristoffer  
> URL: https://psuvtk.github.io/posts/25.-mcu%E8%BD%AF%E4%BB%B6%E7%9A%84%E8%BF%90%E8%A1%8C%E6%A8%A1%E5%BC%8F/  

