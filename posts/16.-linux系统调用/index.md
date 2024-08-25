# 理解LINUX系统调用



## 上下文
内核在执行系统调用的时候处于进程上下文
 - 可以睡眠
 - 可以被抢占 ---&gt; 需要保证系统是可以重入的


## 用户态抢占
从中断返回用户空间时
从系统调用返回用户空间时

## 内核态抢占
时机: 未持有锁

---

> Author: Kristoffer  
> URL: https://psuvtk.github.io/posts/16.-linux%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8/  

