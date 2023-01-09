---
title: NetworkManager 一些技巧
date: 2023-01-09 11:48
swiper: true # 将改文章放入轮播图中
swiperImg: 'https://pic.3gbizhi.com/2020/1022/20201022030556374.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'https://pic.3gbizhi.com/2020/1022/20201022030556374.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: Linux
tags: [Linux, NetworkManager]
top: true
---

NetworkManager 是一个为系统提供检测和配置功能以便自动连接到网络的程序。NetworkManager 的功能对无线和有线网络都很有用。

## 使用 nmcli 进行安全认证

使用 nmcli 命令进入交互命令行模式：

```shell
nmcli c edit <connection_name>
```

配置 802.1x 认证：

```shell
nmcli> set 802-1x.eap peap              // peaph认证模式
nmcli> set 802-1x.phase2-auth gtc       // 认证方式
nmcli> set 802-1x.identity USERNAME     // 联网账号
nmcli> set 802-1x.password USERPASS     // 账号密码
nmcli> save                             // 保存
nmcli> quit                             // 退出
```
