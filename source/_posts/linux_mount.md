---
title: Linux 磁盘挂载
date: 2023-01-07 16:06
swiper: true # 将改文章放入轮播图中
swiperImg: 'https://pic.3gbizhi.com/2020/1010/20201010015227209.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'https://pic.3gbizhi.com/2020/1010/20201010015227209.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: Linux
tags: [Linux]
top: true
---

这里提供一些在 Linux 环境下磁盘挂载小技巧。

## 将U盘挂载到普通用户目录

```bash
mkdir $HOME/usb
sudo mount /dev/sdb1 $HOME/usb -o uid=$UID -o gid=`id -g $USER`
```
