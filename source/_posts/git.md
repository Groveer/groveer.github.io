---
title: git 命令使用方法
date: 2023-01-07 18:46
swiper: true # 将改文章放入轮播图中
swiperImg: 'https://pic.3gbizhi.com/2020/1014/20201014094513254.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'https://pic.3gbizhi.com/2020/1014/20201014094513254.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: Tool
tags: [Tool]
top: true
---

本文章只提供一些不常见的用法，对于 git 基本用法，网上教程很多，这里不再赘述。

## 统计两个tag间代码增删情况

```shell
git diff tag1..tag2 --shortstat ':(exclude)translations'
```
