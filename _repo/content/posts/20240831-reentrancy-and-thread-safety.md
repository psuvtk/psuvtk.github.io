---
title: 可重入性(Reentrancy)与线程安全(Thread-Safety)
subtitle:
date: 2024-08-31T22:47:05+08:00
slug: 6a476d5
draft: false
author:
  name: Kristoffer
  link:
  email:
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - "可重入"
  - "线程安全"
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
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->
## 概念


## 区别与联系

## 应用场景

### 什么时候必须使用 可重入函数？
在信号处理函数、中断上下文使用的函数，当然是同时在普通上下文使用的函数

### 什么时候必须使用 线程安全函数？
线程之间使用共享数据，即存在临界区


## 改造

### 如何将函数改造成 可重入函数？

1、屏蔽信号、中断

2、针对全局变量得使用

3、针对静态变量得使用

3、浮点运算


### 如何将函数改造成 线程安全函数？
加锁

## 参考