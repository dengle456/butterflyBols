---
title: IStoreOS 高并发网卡缓存调整
comments: false
tags:
  - OpenWRT
  - Linux
  - 网卡
keywords: 'OpenWRT,Linux,网卡,J4125'
description:
top_img:
abbrlink: 21cfbf12
cover:
---

## 调整网卡缓存
```shell
   ethtool -g [网卡] #查看当前值
   ethtool -G [网卡] #调整网卡缓存值
   #增加tcp_max_syn_backlog： 允许更多未完成的连接请求。
   echo 8192 > /proc/sys/net/ipv4/tcp_max_syn_backlog
   #增大somaxconn： 允许更多的同时监听连接。
   echo 8192 > /proc/sys/net/core/somaxconn
   # 增加TCP接收缓冲区
   echo "4096 524288 16777216" > /proc/sys/net/ipv4/tcp_rmem
   # 增加TCP发送缓冲区
   echo "4096 65536 16777216" > /proc/sys/net/ipv4/tcp_wmem   
```
## 持久化
```shell
vi /etc/rc.local
```
### 编辑保存 示例
```shell
#!/bin/sh -e
# 其他启动命令...
ethtool -G eth0 rx 4096 tx 4096
echo 8192 > /proc/sys/net/ipv4/tcp_max_syn_backlog
echo 8192 > /proc/sys/net/core/somaxconn
echo "4096 524288 16777216" > /proc/sys/net/ipv4/tcp_rmem
echo "4096 65536 16777216" > /proc/sys/net/ipv4/tcp_wmem

exit 0
```
