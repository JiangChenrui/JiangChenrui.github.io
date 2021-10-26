---
title: linux问题集锦
date: 2019-07-16 09:55:05
categories: [linux]
top: 1
tags:
- linux
- 操作系统
description: <center>记录使用linux系统过程中的操作指令</center>
---

## 命令行更新vscode

下载
```shell
wget https://vscode-update.azurewebsites.net/latest/linux-deb-x64/stable -O /tmp/code_latest_amd64.deb
```
安装
```shell
sudo dpkg -i /tmp/code_latest_amd64.deb
```

## [nohup使用](https://wsgzao.github.io/post/nohup/)

### nohup和&的区别

- &：是指在后台运行，当用户退出（挂起）的时候，命令会自动跟着结束
- nohup：不挂断的运行，没有后台运行的功能，用nohup运行命令可以是命令永久执行下去

### 输出重定向

```shell
nohup command > out.txt 2>&1 &
```
将程序command的输出重定向到out.txt文件，即输出内容不打印到屏幕上，而是输出到out.txt文件中。
2>&1是将标准错误（2）重定向到标准输出（&1），标准输出（&1）再被重定向输入到out.file文件中。

### 关闭nohup启动的程序

使用`jobs -l`查看当前nohup启动的程序，使用`kill`指令关闭

## linux资源查看工具top

在自动输入top后显示的信息如下
![Top](linux问题集锦/top.png)

同样可以使用htop来查看
![htop](linux问题集锦/htop.png)

## linux杀死僵尸进程

在终端输入

```shell
ps -ef | grep defunct | more
```

查看僵尸进程的详细信息，如图：
![僵尸进程](linux问题集锦/僵尸进程.png)
其中第二列为进程PID，第三列为父进程PID，对所有进程的父进程执行kill -9 进程号的操作来杀死僵尸进程

## 将图片地址提取到txt

```shell
ls -R /data/*.jpg > file.txt
cat *.txt > all.txt  // 将所有txt拼接
```

## 批量删除

```shell
find . -name "*.pyc" | xargs rm -f
```
