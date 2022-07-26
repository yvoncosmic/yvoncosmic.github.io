---
title: frp反代，让你的服务运行在公网上
date: 2022-07-26 09:09:55
tags: 亿点心得
---

### 一、frp 是什么？

frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。

**为什么使用 frp？**

通过在具有公网 IP 的节点上部署 frp 服务端，可以轻松地将内网服务穿透到公网，同时提供诸多专业的功能特性，这包括：

- 客户端服务端通信支持 TCP、KCP 以及 Websocket 等多种协议。
- 采用 TCP 连接流式复用，在单个连接间承载更多请求，节省连接建立时间。
- 代理组间的负载均衡。
- 端口复用，多个服务通过同一个服务端端口暴露。
- 多个原生支持的客户端插件（静态文件查看，HTTP、SOCK5 代理等），便于独立使用 frp 客户端完成某些工作。
- 高度扩展性的服务端插件系统，方便结合自身需求进行功能扩展。
- 服务端和客户端 UI 页面。

### 二、准备工作

**1、准备**

- 有公网ip的vps作为服务端，linux，windows都可，甚至是我不认识的darwin，提供反代服务
- 提供web服务的客户端，需要进行端口代理的机器（废话）

**2、下载服务端和客户端文件**

frp下载地址：https://github.com/fatedier/frp/releases 下载最新版本，正常类型amd64即可，环境不同注意区分。

解压包中包含客户端和服务端，如果你和我一样是linux+windows就是需要下这两个，然后对应取出服务端和客户端文件

### 三、安装使用

**1、服务端部署**

解压得到的服务端文件，丢进服务器目录任意位置，编辑配置文件`frps.ini`

```ini
[common]
bind_port = 7000
vhost_http_port = 8000
vhost_https_port = 90

#面板账号
dashboard_user  = admin  
#面板密码
dashboard_pwd = admin
#面板服务端口，启动成功后可通过浏览器访问如http://ip:7500
dashboard_port = 7500  
```

**2、服务端运行frps服务**

目录下`.\frps -c .\fprs.ini`运行即可，建议配合`screen`常驻服务

**3、客户端部署**

解压得到的客户端文件，编辑 `frpc.ini`

```ini
[common]
server_addr = XXX.XXX.XXX.XXX
server_port = 7000

[rdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 3389

[Minecraft]
type = tcp
local_ip = 127.0.0.1
local_port = 25565
remote_port = 25565
```

<font color=#c7254e>server_addr</font> 为 服务端IP

<font color=#c7254e>server_port</font> 为 服务端端口 即上文中的 7000

<font color=#c7254e>type</font> 为 代理类型

<font color=#c7254e>local_ip</font> 为 要进行内网穿透的内网服务器地址

<font color=#c7254e>local_port</font> 为 要进行内网穿透的内网服务器的应用端口

<font color=#c7254e>remote_port</font> 为 服务端监听的端口

> 注：rdp:3389为微软远程桌面默认端口，mc:25565为我的世界默认端口
>
> 其它服务可根据格式自定义修改

**4、客户端运行**

在目录下cmd运行`.\frps -c .\fprs.ini`

**5、服务商防火墙端口放行**

> tcp:7000 #server_port
>
> tcp:7500 #dashboard
>
> tcp:3389 #rdp
>
> tcp:25565 #minecraft
>
> tcp:... #...

### 四、开机自启

Windows开机自启脚本，省略一项步骤（~~归根结底还是公网ip香~~）

**1、在frpc目录下新建frpstart.bat，内容如下:**

```bat
@echo off
:home
frpc -c frpc.ini
goto home
```

**2、打开开始菜单，输入 “任务计划程序” 将会自动搜索，接着打开它。**

**3、点击右侧“创建任务”**

[![jxRJXQ.png](https://s1.ax1x.com/2022/07/26/jxRJXQ.png)](https://imgtu.com/i/jxRJXQ)

**4、创建任务，自定义参数**

[![jxRG6g.png](https://s1.ax1x.com/2022/07/26/jxRG6g.png)](https://imgtu.com/i/jxRG6g)

> 1名称
>
> 2可选
>
> 3可选
>
> 4运行frpc时隐藏bash命令行

**5、编辑触发器**

[![jxROAI.png](https://s1.ax1x.com/2022/07/26/jxROAI.png)](https://imgtu.com/i/jxROAI)

**6、编辑操作**

选择第一步中新建的bat脚本，例E:\yuhui\Downloads\frp_0.44.0_windows_amd64\frpcstart.bat

[![jxIbDS.png](https://s1.ax1x.com/2022/07/26/jxIbDS.png)](https://imgtu.com/i/jxIbDS)

在程序或脚本一栏选择第一步创建的 start.bat，下面的 “起始于” 填写 frpcstart.bat 的上级目录路径（即不要包含 frpcstart.bat）

例如：E:\yuhui\Downloads\frp_0.44.0_windows_amd64~~\frpcstart.bat~~

**亲测不填这个没法自启**



### 五、测试

**放行服务商腾讯云25565端口**，

本地启动我的世界服务端核心，mc客户端连接ip:25565，完美连接。

此处的我的世界联机默认端口25565是要带的，毕竟是反代的端口

[<img src="https://s1.ax1x.com/2022/07/26/jx5Tw4.md.png" alt="jx5Tw4.md.png"  />](https://imgtu.com/i/jx5Tw4)

[![jxfnit.png](https://s1.ax1x.com/2022/07/26/jxfnit.png)](https://imgtu.com/i/jxfnit)



### 六、管理

访问`ip:7500`可以清楚的看到所有正在反代中的端口

[![jxTjlq.png](https://s1.ax1x.com/2022/07/26/jxTjlq.png)](https://imgtu.com/i/jxTjlq)
