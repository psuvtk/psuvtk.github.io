---
title: HUGO 本博客部署
subtitle:
date: 2024-10-07T20:41:19+08:00
slug: 7398491
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
  - "BLOG"
  - "HUGO"
categories:
  - ""
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


在hugo生成的根目录部署发现会出问题，但是部署public目录就是正常的


那么每次生成的时候将public目录内容进行git commit, 然后推送到远端

原始内容怎么办呢？
在public目录下创建 repo目录, 然后将外层目录拷贝到repo目录推送到远端；
更换电脑后，克隆仓库，然后将public/repo目录内容拷贝到外层;