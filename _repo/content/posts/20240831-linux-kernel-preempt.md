---
title: 理解Linux内核抢占
subtitle:
date: 2024-08-31T22:52:50+08:00
slug: 80ff57e
draft: false
author:
  name:
  link:
  email:
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - draft
  - "抢占"
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

## 上下文
内核在执行系统调用的时候处于进程上下文
 - 可以睡眠
 - 可以被抢占 ---> 需要保证系统是可以重入的


## 用户态抢占
从中断返回用户空间时
从系统调用返回用户空间时

## 内核态抢占
时机: 未持有锁



## Real-World Problem