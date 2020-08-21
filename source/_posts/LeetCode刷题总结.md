---
title: LeetCode刷题总结
date: 2019-01-23 19:14:30
categories: 算法
tags: [c++, LeetCode, 算法]
---

## 1 数组用法

```c++
//元素交换
swap(a[1], a[3]);
//sort排序
sort(a.begin(),a.end());
//数组颠倒
reverse(a.begin(), a.end());
//数组元素置为0
memset(a, 0, a.size());
//数组取值
a.push_back();
//定义二维数组
vector< vector<int> > result
```

## 2 set集合的用法

集合中没有重复元素

``` c++
//定义一个int类型的集合
set<int> s;
set<int>::iterator it;
//插入元素10（插入的数值默认从小到大排序）
s.insert(10);
//删除元素10
s.erase(10);
//清空集合
s.clear();
//集合元素的个数
s.size();
//判断集合是否为空
s.empty();
//查找集合中是否与元素10，有的话返回10，没有则返回s.end()
it = s.find(10);
//
```

mutiset：多重集合与set最大的区别是它可以插入重复元素，如果删除的话，相同的会一起删除，如果查找的话，返回该元素的迭代器的位置，若有相同，返回第一个元素的地址，其它使用和set基本类似。  

## 3 [map用法](https://www.cnblogs.com/fnlingnzb-learner/p/5833051.html)  

c++中map中的元素按key升序排列，基本格式为map<string, int> m，需要使用头文件#include<map>。  

```c++
//数据的插入
map<int, string> studentsID;
studentsID.instert(pair<int, string>(1, "student_one"));
studentsID.instert(map<int, string>::value_type(2, "student_two"));
studentID[3]="student_three";
```

## 4 字符串

```c++
//排序
sort(a.begin(), a.end());
//将所有字符转换成小写
transform(s.begin(), s.end(), s.begin(),::tolower);
//截取字符串
//标准库的string有一个substr函数用来截取子字符串。一般使用时传入两个参数，第一个是开始的坐标（第一个字符是0），第二个是截取的长度。
string name("rockderia");
string firstname(name.substr(0,4));

```

## 5 运算

异或（^）：二进制数进行运算相同为0，不同为1
与运算（&）：同时为1，才为1；
或运算（|）：同时为0，才为0；
取反（~）
左移（<<）：左边的二进制丢失，右边补0，左移最高位不包括1，左移相当于该数乘2；
右移（>>）:正数左补0，负数左补1
不同长度的数据进行位运算时，系统会自动补齐。

## 6 栈

栈具有先进后出的特性  

```c++
// 默认构造函数
stack<int> s;
// 复制构造函数
stack<int, list<int>> s1;
stack<int, list<int>> s2(s1);
// 入栈
s.push();
// 出栈，不会显示内容
s.pop();
// 提取栈顶元素
s.top();
// 判断是否非空
s.empty() // true表示未空，false表示非空
// 返回栈中数目
s.size()
```

## 7 队列  

```c++
// 定义队列
queue<int> q;
// 入队
q.push(x)
// 出队
q.pop()
// 访问队首元素
q.front()
// 访问队尾元素
q.back()
// 判断队列是否为空
q.empty()
// 访问队列中元素个数
q.size()
```

队列具有先进先出的特性  
