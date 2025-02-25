---
title: MAC报文格式与LSW原理
subtitle:
date: 2024-09-04T23:59:42+08:00
slug: dced61b
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
  - draft
categories:
  - draft
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

## MAC 报文格式

### Ethernet 帧格式


### Ethernet II 帧格式

![Ethernet II 帧格式](../../images/EtherentII-format.png "ethernet-II-format")

### VLAN帧格式

IEEE 802.1Q标准对Ethernet帧格式进行了修改，在源MAC地址字段和协议类型字段之间加入4字节的802.1Q Tag。

![VLAN帧格式](../../images/mac-packet-format.png "vlan-format")


## LSW原理


## 参考
1. [报文格式地图](https://protocol.aymar.cn/)
