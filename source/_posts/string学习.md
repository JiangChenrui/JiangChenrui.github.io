---
title: string学习
date: 2020-08-11 16:24:25
categories: [c++]
tags: 
- c++
- string
description: <center>本文主要对c++中string字符串的操作如赋值、拼接、查找、替换和截取等进行了实现并和char*进行了对比</center>
---

## string和char*的区别

* char*是一个指针
* string是一个类，类内部封装了char*，管理这个字符串，是一个char*型容器。

特点：
&emsp;&emsp;string类内部封装了很多成员方法，如查找find，拷贝copy，删除delete，替换replace，插入insert。
&emsp;&emsp;string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责。

## 字符串赋值

```c++
string str1;
str1 = "hello world";
cout << "str1 = " << str1 << endl;

string str2;
str2 = str1;
cout << "str2 = " << str2 << endl;

string str3;
str3 = 'a';
cout << "str3 = " << str3 << endl;

string str4;
str4.assign("hello C++");
cout << "str4 = " << str4 << endl;

string str5;
str5.assign("hello C++", 5);
cout << "str5 = " << str5 << endl;

string str6;
str6.assign(10, 'a');
cout << "str6 = " << str6 << endl;
```

## 字符串拼接

```c++
string str1 = "我";
str1 += "爱玩游戏";
cout << "str1 = " << str1 << endl;

str1 += ':';
cout << "str1 = " << str1 << endl;

string str2 = " LOL";
str1 += str2;
cout << "str1 = " << str1 << endl;

string str3 = "I";
str3.append(" love ");
cout << "str3 = " << str3 << endl;

str3.append("game abcde", 4);
cout << "str3 = " << str3 << endl;

str3.append(str2);
cout << "str3 = " << str3 << endl;

str3.append("LOL DNF", 3, 4);
cout << "str3 = " << str3 << endl;
```

## 字符串查找和替换

```c++
// 查找
string str1 = "abcdefg";
int pos = str1.find("de");
if (pos != -1)
    cout << "pos = " << pos << endl;
else
    cout << "未找到" << endl;

// rfind
pos = str1.rfind("de");
if (pos != -1)
    cout << "pos = " << pos << endl;
else
    cout << "未找到" << endl;

// rfind和find的区别
// rfind是从右向左找，find是从左向右

// 替换
// 从1号位置器3个字符替换位"1111"
str1.replace(1, 3, "test1211");
cout << "str1 = " << str1 << endl;
```

## 字符串存取

```c++
string str = "hello";
cout << "str = " << str << endl;

for (int i = 0; i < str.size(); ++i) {
    cout << str[i] << ' ';
}
cout << endl;

for (int i = 0; i < str.size(); ++i) {
    cout << str.at(i) << ' ';
}
cout << endl;

// 修改
str[0] = 'x';
cout << "str = " << str << endl;

str.at(1) = 'x';
cout << "str = " << str << endl;
```

## 字符串插入和删除

```c++
string str = "hello";
cout << "str = " << str << endl;

// 插入
str.insert(1, "111");
cout << "str = " << str << endl;

// 删除
str.erase(1, 3);
cout << "str = " << str << endl;
```

## 字符串获取子串

```c++
string str = "abcdef";
string subStr = str.substr(1, 3);
cout << "subStr = " << subStr << endl;

string email = "zhangsan@sina.com";
// 从邮件地址中 获取 用户信息
int pos = email.find("@");
string usrName = email.substr(0, pos);
cout << usrName << endl;
```