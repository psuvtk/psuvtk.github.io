# Linux系统RT调度(SCHED_FIFO与SCHED_RR)


## RT调度与完全公平调度


引入多核的话事情会变得复杂起来，先从简单的场景入手

```sh
#!/bin/bash

echo &#34;&#34; &gt; /sys/kernel/tracing/trace 
echo 1 &gt; /sys/kernel/tracing/events/sched/enable
echo 80920 &gt; /sys/kernel/tracing/buffer_size_kb
echo 1 &gt; /sys/kernel/tracing/tracing_on
./a.out
echo 0 &gt; /sys/kernel/tracing/tracing_on
cat /sys/kernel/tracing/trace  &gt; /root/trace_out
```




4Core@Ubuntu
实验: 创建3个FIFO




```c
stop_task.c	117 DEFINE_SCHED_CLASS(stop)
idle.c	498 DEFINE_SCHED_CLASS(idle)
deadline.c	2696 DEFINE_SCHED_CLASS(dl)
rt.c	2680 DEFINE_SCHED_CLASS(rt)
fair.c	12355 DEFINE_SCHED_CLASS(fair)
```


调度器会放到单独的段里面
```c
#define DEFINE_SCHED_CLASS(name) \
const struct sched_class name##_sched_class \
	__aligned(__alignof__(struct sched_class)) \
	__section(&#34;__&#34; #name &#34;_sched_class&#34;)
```

---

> Author: Kristoffer  
> URL: http://localhost:1313/posts/28.-rt%E8%B0%83%E5%BA%A6sched_fifo%E4%B8%8Esched_rr/  

