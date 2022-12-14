---
title: 使用zram创建swap分区
date: 2023-01-07 18:33
swiper: true # 将改文章放入轮播图中
swiperImg: 'https://pic.3gbizhi.com/2020/1214/20201214113101309.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'https://pic.3gbizhi.com/2020/1214/20201214113101309.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: Linux
tags: [Linux]
top: true
---

zram 通过在 RAM 内的压缩块设备上分页，直到必须使用硬盘上的交换空间，以避免在磁盘上进行分页，从而提高性能。由于 zram 可以用内存替代硬盘为系统提供交换空间的功能，zram 可以在需要交换/分页时让 Linux 更好利用 RAM，在物理内存较少的旧电脑上尤其如此。
以下是一个简单的脚本用来创建 zram swap 设备，注意，这个脚本假设你还没有使用 zram，并提供了启动的 systemd 配置:

1. `/usr/local/bin/zramswap-on`:

   ```shell
   #!/bin/bash
   # Disable zswap
   echo 0 > /sys/module/zswap/parameters/enabled

   # Load zram module
   modprobe zram

   # use zstd compression
   echo zstd > /sys/block/zram0/comp_algorithm

   # echo 512M > /sys/block/zram0/disksize
   echo 2G > /sys/block/zram0/disksize

   mkswap /dev/zram0

   # Priority can have values between -1 and 32767
   swapon /dev/zram0 -p 32767
   ```

2. `/usr/local/bin/zramswap-off`:

   ```shell
   #!/bin/bash
   swapoff /dev/zram0
   echo 0 > /sys/class/zram-control/hot_remove

   # Not required, but creating a blank uninitalzed drive
   # after removing one may be desired
   cat /sys/class/zram-control/hot_add
   ```

3. `/etc/systemd/system/create-zram-swap.service`：

   ```shell
   [Unit]
   Description=Configures zram swap device
   After=local-fs.target

   [Service]
   Type=oneshot
   ExecStart=/usr/local/bin/zramswap-on
   ExecStop=/usr/local/bin/zramswap-off
   RemainAfterExit=yes

   [Install]
   WantedBy = multi-user.target
   ```

   然后执行服务配置加载和激活:

   ```shell
   sudo chmod +x /usr/local/bin/zramswap-on
   sudo chmod +x /usr/local/bin/zramswap-off
   sudo systemctl daemon-reload
   sudo systemctl enable --now create-zram-swap.service
   ```

   *参考*：[https://cloud-atlas.readthedocs.io/zh_CN/latest/linux/redhat_linux/kernel/zram.html](https://cloud-atlas.readthedocs.io/zh_CN/latest/linux/redhat_linux/kernel/zram.html)
