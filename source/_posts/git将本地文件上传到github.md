---
title: git将本地文件上传到github
date: 2019-05-27 15:01:00
categories: git
tags:
- git操作
---

## 新建远程仓库

![Figure1](git将本地文件上传到github/Figure1.png)

## 在本地创建文件夹

初始化本地的文件夹为一个Git可以管理的仓库

```git
git init
```

## 将本地的仓库和远程仓库关联

```git
git remote add origin <git仓库地址>
```

## 将文件夹下文件添加到仓库

```git
git add .
```

可以在.gitignore中设置不上传的文件

## 将文件提交到仓库

```git
git commit -m "2019/5/27"
```

## 将本地库的内容推送到远程

```git
git push -u origin master
```

* <font face="微软雅黑">origin</font>:远程仓库名；<font face="微软雅黑">master</font>:分支
* 注意:我们第一次<font face="微软雅黑">push</font>的时候,加上<font face="微软雅黑">-u</font>参数,Git就会把本地的master分支和远程的master分支进行关联起来,我们以后的<font face="微软雅黑">push</font>操作就不再需要加上<font face="微软雅黑">-u</font>参数了