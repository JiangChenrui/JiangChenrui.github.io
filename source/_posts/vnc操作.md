---
title: vnc操作
date: 2019-03-21 15:42:15
categories: 服务器
tags: [服务器, 远程桌面]
description: <center>vnc操作记录</center>
---

[参考博客](https://blog.csdn.net/yingyujianmo/article/details/45201097)  

## 查看vnc进程

```shell
ps -ef | grep vnc
```

## 杀掉vnc进程  

```shell
vncserver -kill :9
```

## 使用指定分辨率启动vnc  

```shell
vncserver -geometry 1920x1080 :43
```

## 查看vnc帮助  

```shell
vnc -help
```
