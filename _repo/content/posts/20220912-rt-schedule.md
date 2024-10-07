---
title: "图解Linux系统RT调度(SCHED_FIFO与SCHED_RR)"
subtitle:
date: 2024-08-31T22:54:52+08:00
slug: 0ec2c86
draft: false
author:
  name: "Kristoffer"
  link:
  email:
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - "调度"
  - "SCHED_FIFO"
  - "SCHED_RR"
  - "ftrace"
categories:
  - "Linux"
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRss: false
hiddenFromRelated: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: true
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

## RT调度与完全公平调度


引入多核的话事情会变得复杂起来，先从简单的场景入手

## SCHED_FIFO 调度

那么SCHED_FIFO是否可以一直占用CPU？答案是否定的。
在单核系统中，如果允许SCHED_FIFO任务一直占用CPU不释放，则普通任务永远得不到调度;


## 调度轨迹抓取及可视化工具

### 调度轨迹抓取脚本

```sh
#!/bin/bash

echo "" > /sys/kernel/tracing/trace 
echo 1 > /sys/kernel/tracing/events/sched/enable
echo 80920 > /sys/kernel/tracing/buffer_size_kb
echo 1 > /sys/kernel/tracing/tracing_on
# 执行需要抓取调度轨迹的程序
# ./a.out
echo 0 > /sys/kernel/tracing/tracing_on
cat /sys/kernel/tracing/trace  > /root/trace_out
```

### 可视化工具

Huawei SmartPerf


## 实验

Tested on 4Core@Ubuntu-24.04-VirtualBox

### 单核实验
实验: 2个SCHED_RR任务, 均绑定到核3

实验预期:

![](../../images/Snipaste_2024-08-31_23-54-12.png "2rr_bindcore3")



实验: 2个SCHED_FIFO任务, 均绑定到核3

![](../../images/Snipaste_2024-09-01_00-01-02.png "2fifo_bindcore3")



实验: 1个SCHED_RR任务, 1个SCHED_FIFO任务, 均绑定到核3

![](../../images/Snipaste_2024-09-01_00-08-05.png "1rr_1fifo_bindcore3")


实验代码
```c
#define _GNU_SOURCE
#include <sched.h>
#include <stdint.h>
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#include "time.h"

void payload(int s)
{
	int a,b;
	for (uint64_t i = 0; i < 100000000 * s; i++ ) {	
		a = b + i;	
	}
}

void set_affinity(int p)
{
	cpu_set_t cpuset;
	CPU_ZERO(&cpuset);
	CPU_SET(p, &cpuset);
	if (sched_setaffinity(0, sizeof(cpuset), &cpuset)) {
		printf("bind cpu faild\n");
	}
}

void* rt_fifo_entry1(void *data)
{
    struct sched_param sp = {0};
    sp.sched_priority = 91;
    // if (sched_setscheduler(0, SCHED_RR, &sp)) {
    if (sched_setscheduler(0, SCHED_FIFO, &sp)) {

    	printf("thread set sched_fifo failed\n");
    }
    set_affinity(3);
    printf("thread-1 create, tid: %u\n", gettid());
    
    payload(10);
}

void* rt_fifo_entry2(void *data)
{
    struct sched_param sp = {0};
    sp.sched_priority = 91;
    // if (sched_setscheduler(0, SCHED_RR, &sp)) {
    if (sched_setscheduler(0, SCHED_FIFO, &sp)) {
    	printf("thread set sched_fifo failed\n");
    }
    set_affinity(3);
    printf("thread-2 create, tid: %u\n", gettid());
    payload(10);
}


int main()
{
	pthread_t t;

	pthread_create(&t, NULL, rt_fifo_entry2, NULL);
	pthread_create(&t, NULL, rt_fifo_entry1, NULL);

    	printf("main thread: %u\n",getpid());
	payload(15);
	struct timespec tv={0};
	sched_rr_get_interval(getpid(), &tv);
	printf("sched_rr_interval: %.2lf ms\n", tv.tv_sec * 1e6 + tv.tv_nsec / 1e6);
	printf("main thread done, exit\n");
	return 0;
}
```

管理员可以限制 SCHED_FIFO 带宽，以防止实时应用程序程序员启动对处理器进行单调执行的实时任务。

以下是此策略中使用的一些参数：

/proc/sys/kernel/sched_rt_period_us
此参数以微秒为单位定义时间，它被视为处理器带宽的 10%。默认值为 1000000 InventoryServices，或 1 秒。
/proc/sys/kernel/sched_rt_runtime_us
此参数以微秒为单位定义运行实时线程的时间周期。默认值为 950000 μs，即 0.95 秒。

### 多核实验
实验: 2个SCHED_FIFO任务, 均绑定到 


实验: 3个SCHED_FIFO任务, 分别绑定1~3核


实验: 2个SCHED_FIFO任务, 均绑定到 




## 参考
1. [Linux进程管理 (9)实时调度类分析，以及FIFO和RR对比实验](https://www.cnblogs.com/arnoldlu/p/9025981.html)