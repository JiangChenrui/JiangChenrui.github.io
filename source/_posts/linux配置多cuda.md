---
title: linux配置多cuda
date: 2019-07-29 14:33:43
categories: linux
tags: [linux]
description: <center>linux配置多版本cuda操作方法</center>
---

&emsp;&emsp;最近实习工作需要多个环境的pytorch与cuda，目前所需为pytorch-0.4.1(cuda9.0/cudnn7.5.1/python3.6)与pytorch-0.4.1(cuda8.0/cudnn5.1/python2.7)，pytorch和python版本可以使用virtualenv控制，cuda版本需要安装多个版本cuda，然后生成要使用cuda版本的软链接，将软链接地址加入到环境变量中。

* 删除原软链接

```shell
sudo rm -rf /usr/local/cuda
```

* 建立新的软链接

```shell
sudo ln -s /usr/local/cuda-8.0 /usr/lcoal/cuda
```

可以在/usr/local目录下使用`stat cuda`查看cuda对应的软链接，也可以使用`nvcc -V`查看当前cuda版本

* 修改环境变量

```shell
sudo gedit ~/.bashrc


export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda
```

`/etc/profile`文件也需要修改

```shell
sudo gedit /etc/profile

export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda
```

* 使用命令查看当前cuda软链接地址

```shell
ls -l cuda
```
