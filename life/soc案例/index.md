# 内核编译2


- PMBUS 时序被打断


原因：判断写结束使用FIFO为空进行判断，但实际上FIFO为空并不能保证时序完全被打出
正确的方式是使用控制器的BUSY状态进行判断

---

> Author:   
> URL: http://localhost:1313/life/soc%E6%A1%88%E4%BE%8B/  

