---
title: Teamspeak3服务端简易部署
date: 2022-06-24 11:02:00
tags: 亿点心得
---

最近腾讯云618活动，入手了一台轻量应用服务器，新人首单价格还行，配置如图。

![](https://s3.bmp.ovh/imgs/2022/06/24/5380d996a801d111.png)

参与买赠活动有两个福利可以选择其中一个：1.使用一个4C8G的服务器一个月 2.续费你的订单服务器一个月。我选择了后者，我觉得只是试用的话那还是算了，不如续杯。

正好拿来与小伙伴测试下传说的职业哥用的交流软件Teamspeak3，个人的部署流程如下

### 一、环境准备 ###

1、ECS云服务器一台，我的系统是Ubuntu Server 20.04 LTS 64bit，其它大同小异

2、Teamspeak官网下载对应的服务端文件：[https://www.teamspeak.com/en/downloads/#server](https://www.teamspeak.com/en/downloads/#server)

Teamspeak客户端：[https://www.teamspeak.com/en/downloads/#client](https://www.teamspeak.com/en/downloads/#client) (需自行汉化)

Teamspeak汉化客户端(盗版)：[http://www.ts1.cn/download](http://www.ts1.cn/download)

3、Xshell，Xftp或者其它命令行工具以及FTP工具

### 二、部署服务端 ###

将上步 2 中得到的服务端压缩包解压缩后用Xftp直接上传你的服务器目录，文件夹我命名为teamspeak3，方便命令行检索。

(个人习惯用root账户以及root根目录，虽然很多地方都强调建议用普通用户运行，可能是我还没吃到亏2333)

![](https://s3.bmp.ovh/imgs/2022/06/24/8fa36299d6d57ca0.png)

在终端中进入该文件夹，给予sh脚本运行权限

    chmod 777 ts3server_startscript.sh

启动服务之前，你还需要同意一份用户协议，我们直接执行该指令

    touch .ts3server_license_accepted

启动服务，这一步我直接用root用户运行了，尽管它会发出警告。建议懂得用户权限系统的运行在普通用户下即可，我是弄不明白。

    ./ts3server_startscript.sh start

接着会给你登录账号和密码，以及一枚权限密钥token，这个密钥是最重要的请保存下来，一会登录服务器要用。

有且只能使用一次来确定服务器管理员身份。后续有需要的话请使用客户端生成新的密钥，以防更换电脑带来的权限丢失。

### 三、放行端口 ###

记得在服务商或者终端放行服务需要的端口，ts3需要的端口是

> UDP: 9987
> 
> TCP: 10011
> 
> TCP: 30033

到这一步，服务端的部署基本就完成了，此时ts3就可以连上你的私人语言频道了，在客户端左上角连接里填入你的服务器ip或者域名(多一步解析)以及上一步中终端给你的信息即可。快和小伙伴畅享高质量，无延迟的语言交流吧。

![](https://s3.bmp.ovh/imgs/2022/06/24/747808fadc3cc5d2.png)

### 四、设置开机自启 ###

服务运行后，是没有开机自启的。如果遇到平时重启，主机崩溃之类的还要手动运行较麻烦。可以相应设置一个开机自启的脚本，灵活应对重启状况。

第一步，切换到root用户下，用su指令即可

第二步，在系统服务文件下新建一个teamspeak.service文件

    vim /lib/systemd/system/teamspeak.service

第三步，进入插入模式，向文本添加以下内容。注意所有涉及路径的地方请根据个人情况自行修改，否则将无法运行。
"/root/teamspeak3"为我的安装路径

    [Unit]
    Description=Teamspeak server
    After=network.target
    
    [Service]
    WorkingDirectory=/root/teamspeak3
    User=teamspeak
    Group=teamspeak
    Type=forking
    ExecStart=/root/teamspeak3/ts3server_startscript.sh start
    Inifile=ts3server.ini
    ExecStop=/root/teamspeak3/ts3server_startscript.sh stop
    PIDFile=/root/teamspeak3/ts3server.pid
    RestartSec=15
    Restart=always
    [Install]
    WantedBy=multi-user.target

第四步，重新加载服务配置文件，使新服务的服务程序配置文件生效

    systemctl daemon-reload

第五步，设置开启服务自启动即可，因为此时teamspeak已经在运行了

    systemctl enable teamspeak.service

附上三条启停的指令

1.启动 TeamSpeak：

    systemctl start teamspeak.service

2.停止 TeamSpeak：

    systemctl stop teamspeak.service

3.重启 TeamSpeak：

    systemctl restart teamspeak.service

### 五、总结 ###

个人觉得还是比yy好用不少的，主要体现在以下几点：更少的资源暂用，语音感应激活（更少收录队友杂音），启动速度更快，无开屏弹窗广告纵享丝滑。

当然如果你单纯只为了架ts3而购买服务器是大可不必的，和小伙伴一顿使用下来没浪费多少多少服务器资源，静置状态只有2%的内存占用，相当于40M？？可以说真的很轻量了。