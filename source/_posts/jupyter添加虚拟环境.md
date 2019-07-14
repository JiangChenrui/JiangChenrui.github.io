---
title: jupyter添加虚拟环境
date: 2019-05-15 09:41:24
categories: 服务器
tags: [服务器, 远程桌面]
---
[参考地址](https://www.jianshu.com/p/0432155d1bef)  
[服务器端jupyter开启远程访问](https://blog.csdn.net/simple_the_best/article/details/77005400)  

## virtualenv + jupyter notebook

&emsp;&emsp;为了方便远程使用服务器，在服务器端打开了远程访问，之后需要将服务器中创建的虚拟环境添加到jupyter中。

* 1.进入虚拟环境  

   ```shell
   source pytorch3/bin/activate
   ```

* 2.安装IPykernel

   ```shell
   < python2 >
   pip install ipykernel
   < python3 >
   pip3 install ipykernel
   ```

* 3.将 Virtualenv 加入IPykernel

    ```shell
    < python2 >
    python2 -m ipykernel install --user --name=myproject
    < python3 >
    python3 -m ipykernel install --user --name=myproject
    ```

* 4.启动jupyter notebook并更改kernel
