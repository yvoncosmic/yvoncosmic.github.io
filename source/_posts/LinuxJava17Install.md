---
title: Linux安装java17流程
date: 2022-06-26 12:11:36
tags: 亿点心得
---

## 一、前言

最近心血来潮想在ECS上架设MC服务器和小伙伴一起玩耍回味童年的味道，那么就必须要安装java环境。

Java版本后缀眼花缭乱，到底该安装哪个才能运行我的世界核心包，就是那个.jar的文件？

首先需了解以下几个概念，这个问题就很好解决了：

> - JDK(Java Development Kit)Java开发工具：包含：包含JRE，以及增加编译器和调试器等用于程序开发的文件。
>
> - JRE(Java Runtime Environment)Java环境：包含：Java虚拟机、库函数、运行Java应用程序所必须的文件。
> - JVM(Java Vitrual Machine)Java虚拟机：JVM就是一个虚拟的用于执行bytecode字节码的“虚拟计算机。

jdk是 Java 语言的软件开发工具包，主要用于移动设备、嵌入式设备上的java应用程序。jdk是整个java开发的核心，它包含了JAVA的运行环境，JAVA工具和JAVA

基础的类库。

就单独看名字也知道jdk最大，其次是jre，最后是jvm，jdk是java开发需要的工具包，它包括了jre，而jre是java程序运行时需要的运行环境，它包含了jvm，而jvm是java[虚拟机](https://so.csdn.net/so/search?q=虚拟机&spm=1001.2101.3001.7020)，通过jvm编译字节码。

**总结，虽然不是很懂，但是直接JDK版就完事了**



## 二、安装过程

### 2.1 Linux 手动安装Java17

1、下载安装包

`wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz`

java17和java8一样，都是LTS长期支持版本，因此这里直接下载java17

2、解压安装包，修改自动下载的包名为jdk-17，注意包的名称，可能会有版本更迭。

```bash
tar zxf jdk-17_linux-x64_bin.tar.gz
rm -rf jdk-17_linux-x64_bin.tar.gz
mv jdk-17.0.3.1 jdk-17
```

3、移动文件夹到/opt下

```bash
mv jdk-17 /opt
```

4、将java添加到环境变量末尾

```bash
vi /etc/profile
```

centos7

```bash
export JAVA_HOME=/opt/jdk-17
export PATH=/opt/php/bin:/usr/local/jdk-17/bin:$PATH
```

ubuntu20.04

```bash
JAVA_HOME=/opt/jdk-17
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME PATH
```

5、重新加载环境变量

```bash
source /etc/profile
```

6、验证java是否安装成功

```bash
java -version
```

显示下面内容，说明安装成功

> java version "17" 2021-09-14 LTS
> Java(TM) SE Runtime Environment (build 17+35-LTS-2724)
> Java HotSpot(TM) 64-Bit Server VM (build 17+35-LTS-2724, mixed mode, sharing)



### 2.2 Linux自动安装Java17

```bash
apt-get install openjdk-17-jdk
```

出现如下输出即为安装成功

> root@iZbp1gqpip1640l5lh3uynZ:~# java -version
> openjdk version "17.0.3" 2022-04-19
> OpenJDK Runtime Environment (build 17.0.3+7-Ubuntu-0ubuntu0.20.04.1)
> OpenJDK 64-Bit Server VM (build 17.0.3+7-Ubuntu-0ubuntu0.20.04.1, mixed mode, sharing)