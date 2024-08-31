# 图解Linux系统RT调度(SCHED_FIFO与SCHED_RR)


&lt;!--more--&gt;

## RT调度与完全公平调度


引入多核的话事情会变得复杂起来，先从简单的场景入手



## 调度轨迹抓取及可视化工具

### 调度轨迹抓取脚本

```sh
#!/bin/bash

echo &#34;&#34; &gt; /sys/kernel/tracing/trace 
echo 1 &gt; /sys/kernel/tracing/events/sched/enable
echo 80920 &gt; /sys/kernel/tracing/buffer_size_kb
echo 1 &gt; /sys/kernel/tracing/tracing_on
# 执行需要抓取调度轨迹的程序
# ./a.out
echo 0 &gt; /sys/kernel/tracing/tracing_on
cat /sys/kernel/tracing/trace  &gt; /root/trace_out
```

### 可视化工具

Huawei SmartPerf


## 实验

### 单核实验
实验: 2个SCHED_RR任务, 均绑定到核3

![](../../images/Snipaste_2024-08-31_23-54-12.png)



实验: 2个SCHED_FIFO任务, 均绑定到核3

![](../../images/Snipaste_2024-09-01_00-01-02.png)



实验: 1个SCHED_RR任务, 1个SCHED_FIFO任务, 均绑定到核3

![](../../images/Snipaste_2024-09-01_00-08-05.png)


实验代码
```c
#define _GNU_SOURCE
#include &lt;sched.h&gt;
#include &lt;stdint.h&gt;
#include &lt;pthread.h&gt;
#include &lt;stdio.h&gt;
#include &lt;unistd.h&gt;
#include &#34;time.h&#34;

void payload(int s)
{
	int a,b;
	for (uint64_t i = 0; i &lt; 100000000 * s; i&#43;&#43; ) {	
		a = b &#43; i;	
	}
}

void set_affinity(int p)
{
	cpu_set_t cpuset;
	CPU_ZERO(&amp;cpuset);
	CPU_SET(p, &amp;cpuset);
	if (sched_setaffinity(0, sizeof(cpuset), &amp;cpuset)) {
		printf(&#34;bind cpu faild\n&#34;);
	}
}

void* rt_fifo_entry1(void *data)
{
    struct sched_param sp = {0};
    sp.sched_priority = 91;
    if (sched_setscheduler(0, SCHED_FIFO, &amp;sp)) {
    //if (sched_setscheduler(0, SCHED_RR, &amp;sp)) {
    	printf(&#34;thread set sched_fifo failed\n&#34;);
    }
    set_affinity(3);
    printf(&#34;thread-1 create, tid: %u\n&#34;, gettid());
    
    payload(10);
}

void* rt_fifo_entry2(void *data)
{
    struct sched_param sp = {0};
    sp.sched_priority = 91;
    // if (sched_setscheduler(0, SCHED_FIFO, &amp;sp)) {
    if (sched_setscheduler(0, SCHED_RR, &amp;sp)) {
    	printf(&#34;thread set sched_fifo failed\n&#34;);
    }
    set_affinity(3);
    printf(&#34;thread-2 create, tid: %u\n&#34;, gettid());
    payload(10);
}


int main()
{
	pthread_t t;

	pthread_create(&amp;t, NULL, rt_fifo_entry2, NULL);
	pthread_create(&amp;t, NULL, rt_fifo_entry1, NULL);

    	printf(&#34;main thread: %u\n&#34;,getpid());
	payload(15);
	struct timespec tv={0};
	sched_rr_get_interval(getpid(), &amp;tv);
	printf(&#34;sched_rr_interval: %.2lf ms\n&#34;, tv.tv_sec * 1e6 &#43; tv.tv_nsec / 1e6);
	printf(&#34;main thread done, exit\n&#34;);
	return 0;
}

```



### 多核实验
实验: 2个SCHED_FIFO任务, 均绑定到 

tested on 4Core@Ubuntu-24.04-VirtualBox
实验: 3个SCHED_FIFO任务, 分别绑定1~3核


实验: 2个SCHED_FIFO任务, 均绑定到 






-----------------

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
> URL: http://localhost:1313/posts/0ec2c86/  

