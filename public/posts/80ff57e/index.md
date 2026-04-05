# 理解Linux内核抢占


&lt;!--more--&gt;

## 上下文
内核在执行系统调用的时候处于进程上下文
 - 可以睡眠
 - 可以被抢占 ---&gt; 需要保证系统是可以重入的


## 用户态抢占
从中断返回用户空间时
从系统调用返回用户空间时

## 内核态抢占
时机: 未持有锁



## Real-World Problem

---

> Author:   
> URL: https://psuvtk.github.io/posts/80ff57e/  

