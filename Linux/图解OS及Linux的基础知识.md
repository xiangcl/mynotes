---
title: 图解OS及Linux的基础知识
tags:
  - Linux
  - OS
  - 图解
categories: Linux
permalink: linux/chart-linux-os
---


>试着尽量用图示来表示个人对内容的理解，不足之处，还望不吝指教。



> ### 一.CPU

**1.cpu与指令集**

- **CPU**分为运算器和控制器
- **CPU指令**
	- 特权指令
		- 拥有管理权限，（一般情况下，只有OS才有权限运行特权指令）
	- 普通指令
		- 拥有普通功能，一般应用程序运行

- 不同的运算由不同的运算器完成运算（由指令集提供运算）
- **程序员**：一般面向操作系统编程
- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123c98ab644141d3000341)

**2.人与机器**

>感觉没什么好解释的，一切尽在图中

- **图示说明**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123cacab644141d3000342)




> ### 二.OS

**1.OS的目的与功能**

- **OS**：Operating System
- **System Call**
	- 简称为：Syscall （系统调用）
- **OS**的通用目的与功能：
- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123cb9ab644141d3000343)


**2.编程的层次**

- **硬件规格**：hardware specifiacation
	- 不同厂商的硬件规格千差万别，API也各不相同，写起来极为不便；
- **系统调用**：数量很少，但是很精巧；
- **库调用**：library call
	- 把底层的功能整合出来，提供成离最终目标更近的功能；对所有的计算机功能来说，所有的功能都是通过调用实现；（通常都是c，c++库）

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123cccab64414339000393)


**3.指令环**

- 由内到外依次是**环0、环1、环2、环3**；
- **环0**是特权指令，一般只有操作系统有权限运行；
- **环1、环2**出于历史原因，没有使用；
- **环3**是普通指令，一般应用程序使用；
	- **特殊情况**：例如：``mkdir /home/test``
	- **Note**：没办法直接完成，需要向内核申请权限；

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123cdcab644141d3000344)

**4.程序的运行模式**

- **用户空间**：user space（us）
- **内核空间**：system space

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123ce9ab644141d3000345)




>### 三.UI：User Interface

`对OS来说：UI是用户接口、对用户来说UI是前端；`

- **GUI**：Graphic User Interface （图形用户接口）
- **CLI**：Command Line Interface （命令行接口）

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123d10ab644141d3000347)




> ### 四.ABI与API

- **ABI**：Application Binary Interface（应用程序二进制接口）
	- 描述了应用程序（或者其他类型）和操作系统之间或其他应用程序的低级接口。

- **API**: Application Programming Interface（应用程序编程接口）
	- 是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123d35ab644141d3000348)



> ### 五.主流的CPU架构

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123d3fab64414339000396)

> ### 六.流行的OS分支

- **图示说明：**

![图片标题](https://leanote.com/api/file/getImage?fileId=57123d4bab644141d300034c)







