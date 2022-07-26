---
title: Ubuntu下青龙面板+京东如何薅羊毛
date: 2022-07-06 16:22:17
tags: 亿点心得
---

### 一、前言

青龙面板是一个多功能的可视化面板，利用青龙面板可以自动执行京东领京豆，做东东农场任务签到浇水免费领水果，京喜牧场养小鸡收集鸡蛋，京东极速版金币，京东赚赚领金币，东东萌宠喂养等等。同时还能自动领取京东红包、京喜红包、京东极速版红包……

### 二、安装Docker

国内：

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

国外：

```bash
curl -fsSL https://get.docker.com | bash -s docker
```

启动Docker并设置自启：

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

拉取镜像，如果你是群辉之类的 NAS 安装青龙镜像，在 Dockers 官方镜像列表中拉取最新版的青龙镜像，直接使用 Dockers 搜索镜像`qinglong`即可。

```bash
docker pull whyour/qinglong:latest
```

部署面板

```bash
docker run -dit \
-v $PWD/ql/config:/ql/config \
-v $PWD/ql/log:/ql/log \
-v $PWD/ql/db:/ql/db \
-p 5700:5700 \
--name qinglong \
--hostname qinglong \
--restart always \
whyour/qinglong:latest
```

> 放行TCP端口：5700
>
> 注：这个端口可以修改，防止别人扫默认端口5700。现在不改也没问题，也可以后期修改。

### 三、进入面板

部署完成后，直接访问`IP:5700`访问青龙面板的安装界面。

接着按流程提示操作即可

### 四、拉取任务库

点击右上角新建任务，将下面的拉库指令一条一条填进去即可，这里分享的一共有9条。

定时规则可以默认填写`0 0 0 * * *`，每天0点更新，也可以自己定义。

[![jaB6MT.png](https://s1.ax1x.com/2022/07/06/jaB6MT.png)](https://imgtu.com/i/jaB6MT)

下面分享一些大佬们整合的拉库链接，可以全部添加，也可以只选一条好用的库添加。

因为全部添加的话它们之间会有检测重复任务的脚本，当你运行时它们会互相删除。实测9个库拉出1500条任务，脚本互删之后只剩200左右。

所以我只选择了最后一条使用，库名是`yyds`，[项目地址](https://github.com/okyyds/yydspure/tree/master)

```bash
ql repo https://github.com/KingRan/KR.git "jd_|jx_|jdCookie" "activity|backUp" "^jd[^_]|USER|utils|function|sign|sendNotify|ql|JDJR"

ql repo https://github.com/curtinlv/JD-Script.git

ql repo https://github.com/Zy143L/wskey.git "wskey"

ql repo ql repo https://github.com/smiek2121/scripts.git "jd_|gua_" "" "ZooFaker_Necklace.js|JDJRValidator_Pure.js|sign_graphics_validate.js|cleancart_activity.js|jdCookie.js|sendNotify.js"

ql repo https://github.com/Yun-City/City.git "jd_|jx_|gua_|jddj_|getJDCookie" "activity|backUp" "^jd[^_]|USER|function|utils|sendnotify|ZooFaker_Necklace|jd_Cookie|JDJRValidator_|sign_graphics_validate|ql|magic|cleancart_activity"

ql repo https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify"

ql repo https://github.com/gys619/Absinthe.git "jd_|jx_|jddj_|gua_|getJDCookie|wskey" "activity|backUp" "^jd[^_]|USER|utils|ZooFaker_Necklace|JDJRValidator_|sign_graphics_validate|jddj_cookie|function|ql|magic|JDJR|JD" "main"

ql repo https://github.com/zero205/JD_tencent_scf.git "jd_|jx_|jdCookie" "backUp|icon" "^jd[^_]|USER|sendNotify|sign_graphics_validate|JDJR|JDSign|ql" "main"

ql repo https://github.com/whyour/hundun.git "quanx" "tokens|caiyun|didi|donate|fold|Env"

ql repo https://github.com/okyyds/yydspure.git "jd_|jx_|gua_|jddj_|jdCookie" "activity|backUp" "^jd[^_]|USER|function|utils|sendNotify|ZooFaker_Necklace.js|JDJRValidator_|sign_graphics_validate|ql|JDSignValidator" "master"
```

添加完后运行这一条，等待它自动拉取即可，如果是国内服务器的话会稍微久一些。

### 五、配置京东cookie

安装好面板后，你还需要给面板你的京东cookie以用来登录你的账号

这里我使用的是安卓软件[Alook](https://www.alookweb.com/)来登录[京东移动端](https://m.jd.com/)获取我的cookie，用过电脑浏览器好像不怎么好使，因为我有清电脑端ck的习惯，一退出好像ck就失效了。

用[Alook](https://www.alookweb.com/)登录[京东移动端](https://m.jd.com/)后在 **工具箱→开发者工具→Cookies** 即可找到你的账号的cookie，简单方便

然后回到青龙面板，选择 **环境变量→新建变量** ，名称为 **JD_COOKIE** ，值为刚复制的Cookie，自动拆分保持为**是**。点击确定，添加完毕。

**Tips:**

> - 多账号只需要在值里面换行输入下一个Cookie，不需要再创建一个变量;
>
> - Cookie值只需要其中的 *pt_key* 与 *pt_pin* 就可以了，不懂的话全部复制粘贴上去也没问题;
>
> - 不要频繁的去执行任务，避免黑号;
> - 京东账号建议绑定微信，活动抽奖抽到微信红包会自动提现到微信钱包;
> - 根据脚本提示开通需要的小游戏、活动，需要安装京东，京东极速版，京喜。

### 六、一些依赖可供参考

一些任务需要安装依赖才能跑的动，青龙面板里的 **依赖管理** 功能可以直接安装，将以下所有依赖添加安装即可

**1.NodeJs下**

```bash
crypto-js
prettytable
dotenv
jsdom
date-fns
tough-cookie
tslib
ws@7.4.3
ts-md5
jsdom -g
jieba
fs
form-data
json5
global-agent
png-js
@types/node
require
typescript
js-base64
axios
```

**2.Python3下**

```bash
requests
canvas
ping3
jieba
```

**3.Linux下**

```bash
bizCode
bizMsg
lxml
```

### 七、更换默认端口

**1、执行命令，查询容器ID**

```bash
docker ps -a
```

[![jag21s.png](https://s1.ax1x.com/2022/07/06/jag21s.png)](https://imgtu.com/i/jag21s)

**2、停用docker**

```bash
systemctl stop docker
```

**3、然后根据路径找到这个ID，docker路径为/var/lib/docker/containers**

[![jagRcn.png](https://s1.ax1x.com/2022/07/06/jagRcn.png)](https://imgtu.com/i/jagRcn)

**4、进入容器文件夹，找到hostconfig.json**

[![jagoAU.png](https://s1.ax1x.com/2022/07/06/jagoAU.png)](https://imgtu.com/i/jagoAU)

**5、**

**双击打开文件，修改端口号，将选中的地方将端口号修改，然后ctrl+s保存即可**

[![ja2JbV.md.png](https://s1.ax1x.com/2022/07/06/ja2JbV.md.png)](https://imgtu.com/i/ja2JbV)

**shell修改：vim打开该文件，修改端口号，将蓝色框内端口号修改，然后:wq!保存即可**

[![ja2kjI.md.png](https://s1.ax1x.com/2022/07/06/ja2kjI.md.png)](https://imgtu.com/i/ja2kjI)

 **6、重启docker 和服务**

```bash
#启动docker
systemctl start docker

#启动青龙
docker start qinglong #容器ID
```



### 七、总结

感谢以下几位，文章内容主要借鉴他们：

https://vccv.cc/article/qinglong-jd.html

https://blog.csdn.net/qq_41846013/article/details/122585183

https://blog.csdn.net/Magic_Zsir/article/details/124201351

https://blog.csdn.net/sinat_39613288/article/details/122460430

最后简单分享波收益吧，无非就是薅点小羊毛罢了，合理利用服务器闲置资源何乐而不为呢。

两个号一起一个月的京豆大概二十块吧，加上jd农场种植会送实物，还有极速版的金币也有二十，挺可观的。



[<img src="https://s1.ax1x.com/2022/07/06/ja6orV.jpg" alt="ja6orV.jpg" style="zoom: 33%;" />](https://imgtu.com/i/ja6orV)
[<img src="https://s1.ax1x.com/2022/07/06/ja6IK0.jpg" alt="ja6IK0.jpg" style="zoom:33%;" />](https://imgtu.com/i/ja6IK0)
