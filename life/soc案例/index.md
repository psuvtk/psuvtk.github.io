# 内核编译2


### PMBUS 时序被打断

常见原因： 
1、调度打断

2、异常掉电

3、控制器状态判断错误
原因：判断写结束使用FIFO为空进行判断，但实际上FIFO为空并不能保证时序完全被打出
正确的方式是使用控制器的BUSY状态进行判断;
对于STM应该使用TC 而非TX EMPTY判断




### EL3返回EL1 运行模式设置错误导致异常



### 调度

---

> Author:   
> URL: https://psuvtk.github.io/life/soc%E6%A1%88%E4%BE%8B/  

