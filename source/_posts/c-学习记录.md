---
title: c++学习记录
date: 2020-08-05 22:51:30
categories: [c++]
top: 10
tags:
- c++
description: <center>本文主要记录c++学习过程中遇到的一些问题和一些不清楚的点进行了归纳和总结，包括数组的操作、类、文件的读写等</center>
---

## 指针常量和常量指针

常量指针指向的内容不可以修改，指针的指向可以修改；指针常量的指向不可以修改，指针指向的值可以修改。

```c++
void const_pointer() {
    int a = 10;
    int b = 20;
    /*
    常量指针：
    指针指向的内容不可以修改，指针的指向可以修改
    */
    const int* p = &a;
    // *p = 20 error
    p = &b;
    cout << "p is " << *p << endl;
    /*
    指针常量
    指针的指向不可以修改，指针指向的值可以修改
    */
    int* const p2 = &a;
    *p2 = 20;
    cout << "p2 is " << *p2 << endl;
}
```
## 数组

### 二维数组定义和分配内存

c++中有三种方法来动态申请多维数组

1)  c中的malloc/free
2)  c++中的new/delete
3)  STL容器中的vector

#### 第一种： malloc/free

```c++
/**
 * c++动态申请多维数组
 * 第一种 malloc/free
 * @param len 一维数组大小
 * @param m 二维数组行数
 * @param n 二维数组列数
 */
void dynamicCreate1Array(int len, int m, int n) {
    // 一维数组
    int *p;
    p = (int*)malloc(sizeof(int)*len);    // 动态开辟m大小的数组
    cout << "malloc/free" << endl;
    PrintArray(p, len);
    // 释放空间
    free(p);

    // 二维数组
    int **p2;
    p2 = (int**)malloc(sizeof(int*)*m);         //开辟行
    for (int i = 0; i < m; ++i) {
        p2[i] = (int*)malloc(sizeof(int*)*n);   //开辟列
    }
    PrintArray(p2, m, n);
    // 释放空间
    for (int i = 0; i < m; ++i)
        free(*(p2+i));
}
```

#### 第二种：new/delete

```c++
/**
 * c++动态申请多维数组
 * 第二种 new/delete
 * @param len 一维数组大小
 * @param m 二维数组行数
 * @param n 二维数组列数
 */
void dynamicCreate2Array(int len, int m, int n) {
    // 开辟空间
    int *p = new int[len];      // 只分配空间未进行初始话
    int *p1 = new int[len]();    // 分配空间并进行初始化
    cout << "new/delete" << endl;
    PrintArray(p, len);
    PrintArray(p1, len);
    // 释放空间
    delete[] p;
    delete[] p1;

    int **p2 = new int*[m];     // 开辟行
    for (int i = 0; i < m; ++i)
        p2[i] = new int[n]();     // 开辟列
    PrintArray(p2, m, n);
    // 释放空间
    for (int i = 0; i < m; ++i)
        delete[] p2[i];
    delete[] p2;
}
```

#### 第三种：STL容器中的vector

```c++
/**
 * c++动态申请多维数组
 * 第三种 
 * @param len 一维数组大小
 * @param m 二维数组行数
 * @param n 二维数组列数
 */
void dynamicCreate3Array(int len, int m, int n) {
    vector<int> p(len);
    cout << "vector" << endl;
    cout << "输出一维数组" << endl;
    for (int i = 0; i < p.size(); ++i)
        cout << p[i] << ' ';
    cout << endl;

    vector<vector<int> > p2(m, vector<int>(n));
    cout << "输出二维数组" << endl;
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j)
            cout << p2[i][j] << ' ';
        cout << endl;
    }
}
```

## 引用

引用的本质是指针常量

## 类

### 类的权限

类的权限有3种

1. public 类内类外都可以访问
2. protected 类内可以访问，类外不可以访问，可以继承
3. private 类内可以访问，类外不可以访问，不可以继承

### struct和class的区别

struct默认权限为公共，class默认权限为私有。

### 构造函数和析构函数

* 构造函数
    * 函数名与类名相同
    * 构造函数可以有参数，可以发生重载
    * 创建对象时，构造函数会自动调用，而且只调用一次
* 析构函数
    * 没有返回值，不写void
    * 函数名与类名相同，在名称前加~
    * 析构函数不可以有参数，不可以发生重载
    * 对象在销毁前会自动调用析构函数，而且只会调用一次

### 深拷贝与浅拷贝

* 浅拷贝：简单的赋值拷贝操作（系统默认拷贝构造方式为浅拷贝）
* 深拷贝：在堆区重新申请空间，进行拷贝操作

## 文件读写

### c++文件操作有三类

1. ofstream:写操作
2. ifstream:读操作
3. fstream: 读写操作

### 读/写文件的步骤

打开头文件->创建流对象->打开文件->写/读数据->关闭文件

### 打开文件的方式

| 模式 | 含义 |
| ------- | ------- |
| in | 打开文件用于读取 |
| out | 打开文件用于写入 |
| ate | 打开文件并移到末尾 |
| app | 打开文件用于追加 |
| trunc | 若文件已存在，打开文件并截  取流（清除原有数据) |
| binary | 以二进制流方式打开文件 |

### 读取文本文件的四种方法

```c++
void test02() {
    ifstream ifs;

    ifs.open("test.txt", ios::in);
    if (!ifs.is_open()) {
        cout << "文件打开失败" << endl;
        return;
    }
    // 读数据
    // 第一种
    char buf[1024] = {0};
    while (ifs >> buf) {
        cout << buf << endl;
    }

    // 第二种
    char buf[1024] = {0};
    while (ifs.getline(buf, sizeof(buf))) {
        cout << buf << endl;
    }

    // 第三种
    string buf;
    while (getline(ifs, buf)) {
        cout << buf << endl;
    }

    // 第四种
    char c;
    while ((c = ifs.get()) != EOF) { // EOF end of file
        cout << c;
    }

    ifs.close();
}
```

### 二进制文件的读写

```c++
class Person {
   public:
    char m_name[64];
    int m_age;
};

// 写文件
void test03() {
    ofstream ofs;

    ofs.open("person.txt", ios::out | ios::binary);
    Person p = {"张三", 18};
    ofs.write((const char *)&p, sizeof(Person));

    ofs.close();
}

// 读文件
void test04() {
    ifstream ifs;

    ifs.open("person.txt", ios::in | ios::binary);
    if (!ifs.is_open()) {
        cout << "文件打开失败" << endl;
        return;
    }

    Person p;
    ifs.read((char *)&p, sizeof(Person));
    cout << "姓名：" << p.m_name << endl;
    cout << "年龄：" << p.m_age << endl;

    ifs.close();
}
```
