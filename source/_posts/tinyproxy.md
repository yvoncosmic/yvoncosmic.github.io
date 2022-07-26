---
title: 轻量代理服务TinyProxy部署
date: 2022-07-26 08:30:14
tags: 亿点心得
---

### 一、TinyProxy介绍

TinyProxy 是一个轻量级的开源 web 代理守护进程，其设计目标是快而小。它适用于需要完整 HTTP 代理特性，但系统资源又不足以运行大型代理的场景，比如嵌入式部署。它对小规模网络非常有用，这样的场合下大型代理会使系统资源紧张，或有安全风险。

Tinyproxy 的一个关键特性是其缓冲连接的理念。从效果上看， TinyProxy 对服务器的响应进行了高速缓冲，然后按照客户端能够处理的最高速度进行响应。该特性极大的降低了网络延滞带来的问题。



### 二、准备

- 具备公网IP的服务器（Ubuntu 22.04 LTS，腾讯云）
- 本地计算机（windows 10）



### 三、安装

**1.在服务器安装程序TinyProxy**

`apt update`

`apt install tinyproxy`

**2.在服务器打开配置文件**

`vim /etc/tinyproxy/tinyproxy.conf`

**3.修改该配置文件**

①定义端口

> \#定义监听端口，默认端口为8888，当然你可以更改为你喜欢的端口。(防扫描?)
>
> #若果端口号小于1024，则需要使用root启动tinyproxy。
>
> Port 8888

②设置允许连接的ip

> \#定义允许连接的IP，默认只允许本地计算机连接。
>
> #若前面加#屏蔽此参数，则允许所有人连接。国内服务商给的内网ip大多为动态ip，经常变动，建议#注释掉该项。
>
> #Allow 127.0.0.1

**4.启动/停止/状态/重启**

```bash
systemctl start tinyproxy.service
systemctl stop tinyproxy.service
systemctl status tinyproxy.service
systemctl restart tinyproxy.service
```

**附：查看日志**

```
tailf /var/log/tinyproxy/tinyproxy.log
```



### 四、使用

**1.windows全局**

在设置→网络→代理中设置你的ip和服务端口即可

[<img src="https://s1.ax1x.com/2022/07/26/jxcWAP.jpg" alt="jxcWAP.jpg" style="zoom: 80%;" />](https://imgtu.com/i/jxcWAP)



**2.chrome插件**

[SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=zh)

[<img src="https://s1.ax1x.com/2022/07/26/jxcTXj.png" alt="jxcTXj.png" style="zoom: 80%;" />](https://imgtu.com/i/jxcTXj)



**3.配置好后查本机ip如下，代理成功**

[<img src="https://s1.ax1x.com/2022/07/26/jxcftf.png" alt="jxcftf.png" style="zoom: 80%;" />](https://imgtu.com/i/jxcftf)
