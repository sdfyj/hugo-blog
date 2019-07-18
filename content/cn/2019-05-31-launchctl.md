---
title: "launchctl定时任务"
date: 2019-05-31
categories:
  - 计算机编程
tags:
  - launchctl
  - python
slug: launchctl
---



launchctl是一个统一的服务管理框架，可以启动、停止和管理守护进程、应用程序、进程和脚本等。

launchctl是通过配置文件来指定执行周期和任务的。

mac也可以像linux系统一样，使用crontab命令来添加定时任务,不推荐。

任务目标：每天晚上十点定时执行/Users/demo/helloworld.py的python程序


#### 创建run.sh脚本
进入 helloworld.py程序所在目录
```cmd
cd /User/demo
```
创建run.sh脚本
```cmd
vi run.sh
```
添加执行helloworld.py的命令
```cmd
#!/bin/sh

LANG=zh_CN.UTF-8
#支持中文输出
#LANG=en_US.UTF-8
export LANG

# 记录一下开始时间
echo `date` >> /Users/demo/log &&
# 进入helloworld.py程序所在目录
cd /Users/demo &&
# 执行python脚本（注意前面要指定python运行环境/usr/bin/python，根据自己的情况改变）
/usr/bin/python helloworld.py
# 运行完成
echo 'finish' >> /Users/demo/log
```
然后`：wq`保存退出

#### 如何运行.sh脚本

* 直接拖到终端
* 从终端中(cd)进入到脚本所在的路径，在终端中直接运行

进入run.sh所在文件夹
```cmd
./run.sh
```
出现`permission denied`的解决办法,这种错误是因为权限问题，重新设置一下权限就可以运行
```cmd
chmod 777 run.sh
```

#### 编写plist文件
launchctl将根据plist文件的信息来启动任务。plist脚本一般存放在以下目录：

* /Library/LaunchDaemons –>只要系统启动了，哪怕用户不登陆系统也会被执行
* /Library/LaunchAgents –>当用户登陆系统后才会被执行

更多的plist存放目录：

* ~/Library/LaunchAgents 由用户自己定义的任务项
* /Library/LaunchAgents 由管理员为用户定义的任务项
* /Library/LaunchDaemons 由管理员定义的守护进程任务项
* /System/Library/LaunchAgents 由Mac OS X为用户定义的任务项
* /System/Library/LaunchDaemons 由Mac OS X定义的守护进程任务项

进入~/Library/LaunchAgents，创建一个plist文件com.demo.plist
```plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <!-- Label唯一的标识 -->
  <key>Label</key>
  <string>com.demo.plist</string>
  <!-- 指定要运行的脚本 -->
  <key>ProgramArguments</key>
  <array>
    <string>/Users/demo/run.sh</string>
  </array>
  <!-- 指定要运行的时间 -->
  <key>StartCalendarInterval</key>
  <dict>
        <key>Minute</key>
        <integer>00</integer>
        <key>Hour</key>
        <integer>22</integer>
  </dict>
<!-- 标准输出文件 -->
<key>StandardOutPath</key>
<string>/Users/demo/run.log</string>
<!-- 标准错误输出文件，错误日志 -->
<key>StandardErrorPath</key>
<string>/Users/demo/run.err</string>
</dict>
</plist>
```
#### 加载命令
```cmd
launchctl load -w com.demo.plist
```
这样任务就加载成功了。

更多的命令：
```cmd
# 加载任务, -w选项会将plist文件中无效的key覆盖掉，建议加上
$ launchctl load -w com.demo.plist
# 删除任务
$ launchctl unload -w com.demo.plist
# 查看任务列表, 使用 grep '任务部分名字' 过滤
$ launchctl list | grep 'com.demo'
# 开始任务
$ launchctl start  com.demo.plist
# 结束任务
$ launchctl stop   com.demo.plist
```

如果任务修改了，那么必须先unload，然后重新load。start可以测试任务，这个是立即执行，不管时间到了没有
执行start和unload前，任务必须先load过，否则报错。stop可以停止任务

#### 番外篇
plist支持两种方式配置执行时间：

* StartInterval: 指定脚本每间隔多长时间（单位：秒）执行一次；
* StartCalendarInterval: 可以指定脚本在多少分钟、小时、天、星期几、月时间上执行，类似如crontab的中的设置，包含下面的 key:

```cmd
Minute <integer>
The minute on which this job will be run.
Hour <integer>
The hour on which this job will be run.
Day <integer>
The day on which this job will be run.
Weekday <integer>
The weekday on which this job will be run (0 and 7 are Sunday).
Month <integer>
The month on which this job will be run.
```

plist部分参数说明：

* Label：对应的需要保证全局唯一性；
* Program：要运行的程序；
* ProgramArguments：命令语句
* StartCalendarInterval：运行的时间，单个时间点使用dict，多个时间点使用 array
* StartInterval：时间间隔，与StartCalendarInterval使用其一，单位为秒
* StandardInPath、StandardOutPath、StandardErrorPath：标准的输入输出错误文件，这里建议不要使用 .log 作为后缀，会打不开里面的信息。

定时启动任务时，如果涉及到网络，但是电脑处于睡眠状态，是执行不了的，这个时候，可以定时的启动屏幕就好了。