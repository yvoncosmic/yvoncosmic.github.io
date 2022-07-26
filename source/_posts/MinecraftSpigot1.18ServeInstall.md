---
title: 我的世界Spigot1.18简易部署流程
date: 2022-06-26 16:49:34
tags: 亿点心得
---

### 一、环境准备

1、Java环境，MC不同的版本也有对应不同的java版本，例如这里的使用的spigot1.18需要java17

2、服务端核心，下载地址https://getbukkit.org/download/spigot

### 二、部署过程

1、将上一步得到的核心文件spigot-1.xx.jar，上传至你的服务器目录运行即可。

注意：运行后会生成很多服务端文件，所以可以新建一个文件夹用于存放服务端文件。

[![jA4v9I.png](https://s1.ax1x.com/2022/06/26/jA4v9I.png)](https://imgtu.com/i/jA4v9I)

2、在服务端目录下运行以下指令，其中“spigot-1.18.jar”请自行更换为自己的核心名称。

```bash
java -Xmx1024M -Xms1024M -jar spigot-1.18.jar nogui
```

- **-Xms** 是指程序启动时初始内存大小（此值可以设置成与-Xmx相同，以避免每次GC完成后 JVM 内存重新分配）。
- **-Xmx** 指程序运行时最大可用内存大小，程序运行中内存大于这个值会 OutOfMemory。



3、首次运行核心文件，会提示你同意本地目录下生成的一份用户协议文件，我们通过vim编辑它即可。

[![jA4ONd.md.png](https://s1.ax1x.com/2022/06/26/jA4ONd.md.png)](https://imgtu.com/i/jA4ONd)

通过`vim eula.txt`修改eula该项的值为true即可，完成后ESC→:wq保存退出即可，再次运行java指令就会生成所有的游戏服务端文件，包括地图等等。



5、经过漫长的等待后，出现下面的画面即为开服成功。

[![jA47nO.png](https://s1.ax1x.com/2022/06/26/jA47nO.png)](https://imgtu.com/i/jA47nO)



6、请到安全组/防火墙开放25565我的世界默认联机端口！

> TCP: 25565



7、关闭正版认证

如果需要关闭正版认证，请编辑目录下server.properties中`online-mode=false`，不然盗版用户将无法进入！



### 三、使用MCSManager托管你的服务器，

如果你想将你的服务进程运行在xshell的话，请使用screen挂起。

或者使用一种操作更方便，更快捷，免费的管理面板MCSM: https://mcsmanager.com/

它适用于主流 Linux 系统，安装成功后，使用 systemctl start mcsm-{web, daemon} 命令即可启动。你只需要执行以下命令，即可完成MCSM的快速安装。

```
wget -qO- https://gitee.com/mcsmanager/script/raw/master/setup.sh | bash
```

安装完成后如图所示，使用 http://你的ip地址:23333 即可连接面板。之后对于服务器的启停操作，监控，文件管理会十分方便。

![](https://s3.bmp.ovh/imgs/2022/06/26/f2b834f0d4842a4c.png)

同样的，需放行端口23333(面板web服务)，24444(守护进程)

> TCP:23333
>
> TCP:24444

相关MCSM服务指令

```
重启 systemctl restart mcsm-{daemon,web}.service
禁用 systemctl disable mcsm-{daemon,web}.service
启用 systemctl enable mcsm-{daemon,web}.service
启动 systemctl start mcsm-{daemon,web}.service
停止 systemctl stop mcsm-{daemon,web}.service
```

更多使用说明，请参考：https://docs.mcsmanager.com



### 四、连接至服务器

客户端，多人游戏，填入ip地址，完成连接。

请同时告知你的小伙伴一起游玩吧~

[![jA4x3t.png](https://s1.ax1x.com/2022/06/26/jA4x3t.png)](https://imgtu.com/i/jA4x3t)

[![jA4VOO.png](https://s1.ax1x.com/2022/06/26/jA4VOO.png)](https://imgtu.com/i/jA4VOO)



### 五、补充

2022/7/22 

整了个新机器，在MCSManager尝试启动spigot1.18服务端时失败，在ssh启动却没有问题，其中MCSM报错：

`[MCSMANAGER-PTY] Process Start Error:exec: "java": executable file not found in $PATH`

`[MCSMANAGER] [ERROR] 检测到实例启动后在极短的时间内退出，原因可能是您的启动命令错误或配置文件错误。`

解决方法：安装java SDK 17即可

```
sudo apt install openjdk-17-jdk
```

安装后就可以正常运行了，原理未知?_?
