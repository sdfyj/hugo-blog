---
title: "ubuntu18.04下安装时出现Could not get lock /var/lib/dpkg/lock 的解决方法"
date: 2019-11-27
categories:
  - 计算机编程
tags:
  - ubuntu
slug: ubuntu_dpkg
---


在Ubuntu中，有时候运用`sudo  apt-get install`安装软件时，会出现以下的情况
```terminal
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)

E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```
主要是因为apt还在运行。

解决方案：杀死所有的apt进程。

1：查找所有apt相关的进程，并用命令杀死。
```terminal
**@**:~$ ps afx|grep apt
 3284 pts/0    S+     0:00  \_ grep --color=auto apt
 2869 ?        Ss     0:00 /bin/sh /usr/lib/apt/apt.systemd.daily install
 2873 ?        S      0:00  \_ /bin/sh /usr/lib/apt/apt.systemd.daily lock_is_held install

**@**:~$sudo kill -9 2873
**@**:~$ sudo kill -9 2869
```
2：删除锁定文件

 锁定的文件会阻止 Linux 系统中某些文件或者数据的访问，这个概念也存在于 Windows 或者其他的操作系统中。

一旦你运行了 apt-get 或者 apt 命令，锁定文件将会创建于 /var/lib/apt/lists/、/var/lib/dpkg/、/var/cache/apt/archives/ 中。

这有助于运行中的 apt-get 或者 apt 进程能够避免被其它需要使用相同文件的用户或者系统进程所打断。当该进程执行完毕后，锁定文件将会删除。

所以：

1：移除对应目录下的锁文件：

2：强制重新配置软件包：

3：更新软件包源文件：
```terminal
**@**:~$ sudo rm /var/lib/dpkg/lock
**@**:~$ sudo dpkg --configure -a
**@**:~$ sudo apt update
```

参考文献

[关于Ubuntu中Could not get lock /var/lib/dpkg/lock解决方案](https://www.cnblogs.com/yun6853992/p/9343816.html)