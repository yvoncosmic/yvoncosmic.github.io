---
title: Hexo + Github pages 搭建个人博客
date: 2022-07-01 09:40:32
tags: 亿点心得
---

### 一、主要工具简介

**1.1、GitHub Pages**

GitHub推出的免费功能，它允许用户的任何一个Repo的gh-pages分支上的代码可以经由HTTP访问到。类似提供了静态文件服务。你可以用Github Pages搭建博客，也可以把项目的文档和主页放在上面。请注意，善待Github资源！

**1.2、git(主要使用git bash)。**

git–程序员的时光机，保存文件，为你随时恢复你想要的版本。
本次搭建博客过程中使用git进行版本控制，并将本地仓库托管到GitHub进行管理。

**1.3、Node.js(主要使用其工具npm下载hexo)**

Node.js 就是运行在服务端的 JavaScript，Node.js 的包管理器 npm，是全球最大的开源库生态系统。
npm相当于软件管家，搜索、下载和删除包。

**1.4、Hexo**

Hexo是一个基于nodejs 的静态博客网站生成器。





### 二、大致流程

2.1、进行环境配置，在本地安装git、Node.js和hexo;

2.2、使用hexo本地编辑文章并实现本地预览；

2.3、注册[GitHub](https://github.com/)账户并创建远程仓库；

2.4、使用git bash部署到GitHub供人浏览观看；

2.5、主题美化。





### 三、安装步骤

**3.1、安装git**

官网地址：[下载安装](https://gitforwindows.org/)



**3.2、安装Node.js**

Node.js中文官网地址：[下载安装](https://nodejs.org/zh-cn/)

①安装后，在nodejs安装文件夹下新建两个空白文件夹【node_global】【node_cache】

②创建完两个空文件夹之后，打开cmd命令窗口，输入

```bash
npm config set prefix "D:\Environment\nodejs\node_global"  //设置使用npm全局安装时下载文件的位置
npm config set cache "D:\Environment\nodejs\node_cache"    //设置缓存位置
npm config set registry https://registry.npm.taobao.org/   //更换镜像源，加快下载速度
npm config list                                            //显示配置信息
```

> 以下两步可以选择不做，为优化下载位置的操作。

③在【系统变量】下新建【NODE_PATH】，输入【D:\Program Files\nodejs\node_global\node_modules】//设置下载位置

使用 npm -g 安装的时候，默认的模块D:\Program Files\nodejs\node_modules 目录将会改变为D:\Program Files\nodejs\node_global\node_modules目录

④将【系统变量】下的【Path】添加【D:\Program Files\nodejs\node_global】

> 否则可能报错：‘vue’不是内部或外部命令，也不是可运行的程序或批处理文件。

</br>

**3.3、安装hexo**

①安装核心框架hexo-cli，在cmd下执行

```bash
npm install hexo-cli -g
```

②创建一个文件夹用于存放hexo，然后右键该文件夹使用Git bash here，意思是直接在此目录下打开Git命令行工具，之后我们只用指令生成的hexo文件就会直接存放在这个文件夹内。

③安装hexo

```bash
npm install hexo --save
```

④检查安装是否成功

```bash
hexo -v
```

⑤在新建文件夹下使用git bash here初始化hexo，这一步会生成hexo默认文件

```bash
hexo init
```

⑥现在已经可以部署本地静态页面了

```bash
hexo clean 
hexo g //生成Hexo页面
hexo s //启动服务
```

本地浏览器访问**localhost:4000**就可以看到你的hexo博客初始化界面了。



### 四、本地撰写文章并预览

**4.1、新建文章**

```bash
hexo new post '我的第一篇文章' //自定义文章标题
```

</br>

**4.2、编辑文章**

hexo会自动生成一个md文件，可以自由的编辑你的md内容了

头部文件如下，可以修改基本的文章参数

```bash
---
title: postName #文章页面上的显示名称
date: 2022-07-01 10:00:16 #文章生成时间，一般不改，当然也可以任意修改
categories: 默认分类 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意冒号:后面有个空格
description: 摘要
---
```

</br>

**4.3、本地部署静态页面**

```bash
hexo cl && hexo g && hexo s
```



### 五、部署至github pages

①注册github账号

在github上创建并设置个人远程库，注册登录略过，不会的请自行百度。这里有一点必须注意，存储库名称必须为`用户名.github.io`

然后打开Git Bash**设置用户**。目的时告诉远程仓库谁上传的。

```bash
git config --global user.name "GitHub的用户名"
git config --global user.email "注册GitHub使用的邮箱"
```

然后通过下面代码即可查看用户信息配置。

```bash
git config --global --list
```

②git bash下生成密钥

```bash
ssh-keygen -t rsa -C "XXXXXXXXX@XXX.com"
```

③查看`id_rsa.pub`文件，并全部复制

```bash
cat ~/.ssh/id_rsa.pub
```

④在`Github个人设置`中添加`ssh key`

[![jMHEFA.png](https://s1.ax1x.com/2022/07/01/jMHEFA.png)](https://imgtu.com/i/jMHEFA)

⑤修改hexo根目录下的文件`_config.yml`中的deploy，**请将用户名修改为自己的用户名**。注意自己的github的hexo项目主分支名称，一般是main或者master。

```bash
deploy:
 type: git
 repo: git@github.com:yvoncosmic/yvoncosmic.github.io.git
 branch: main
```

⑥部署至github pages

```bash
hexo cl && hexo g && hexo d #一键推送
```

⑦查看blog：[https://yvoncosmic.github.io](https://yvoncosmic.github.io)

⑧依次执行以下命令提交网站相关的文件（hexo存放网站的原始文件）

hexo为github副分支，有别于main主分支，我用来存放hexo源文件

```bash
git add .
git commit -m "init"
git push origin hexo
```



### 六、博客美化

**6.1、主题安装**

选用了一款 Material Design 风格的主题Fluid，官方Github页面：https://github.com/fluid-dev/hexo-theme-fluid

根据用户文档安装即可

</br>

**6.2、添加鼠标点击效果**

这是一个点击鼠标显示红心并浮现社会主义核心价值观的效果，挺有意思。[参考文章](https://blog.51cto.com/u_13640625/3033188)

①添加love-click.js放在../hexo/themes/next/source/js/目录下：

```javascript
/* 爱心特效 */

!function (e, t, a) {
  function r() {
    for (var e = 0; e < s.length; e++) s[e].alpha <= 0 ? (t.body.removeChild(s[e].el), s.splice(e, 1)) : (s[e].y--, s[e].scale += .004, s[e].alpha -= .013, s[e].el.style.cssText = "left:" + s[e].x + "px;top:" + s[e].y + "px;opacity:" + s[e].alpha + ";transform:scale(" + s[e].scale + "," + s[e].scale + ") rotate(45deg);background:" + s[e].color + ";z-index:99999");
    requestAnimationFrame(r)
  }

  function n() {
    var t = "function" == typeof e.onclick && e.onclick;
    e.onclick = function (e) {
      t && t(), o(e)
    }
  }

  function o(e) {
    var a = t.createElement("div");
    a.className = "heart", s.push({
      el: a,
      x: e.clientX - 5,
      y: e.clientY - 5,
      scale: 1,
      alpha: 1,
      color: c()
    }), t.body.appendChild(a)
  }

  function i(e) {
    var a = t.createElement("style");
    a.type = "text/css";
    try {
      a.appendChild(t.createTextNode(e))
    } catch (t) {
      a.styleSheet.cssText = e
    }
    t.getElementsByTagName("head")[0].appendChild(a)
  }

  function c() {
    return "rgb(" + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + ")"
  }
  var s = [];
  e.requestAnimationFrame = e.requestAnimationFrame || e.webkitRequestAnimationFrame || e.mozRequestAnimationFrame || e.oRequestAnimationFrame || e.msRequestAnimationFrame || function (e) {
    setTimeout(e, 1e3 / 60)
  }, i(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"), n(), r()
}(window, document);

/* 社会主体核心价值观效果 */
var a_idx = 0;
jQuery(document).ready(function ($) {
  $("body").click(function (e) {
    var a = new Array("富强", "民主", "文明", "和谐", "自由", "平等", "公正", "法治", "爱国", "敬业", "诚信", "友善");
    var $i = $("<span/>").text(a[a_idx]);
    a_idx = (a_idx + 1) % a.length;
    var x = e.pageX,
      y = e.pageY;
    $i.css({
      "z-index": 100000000,
      "top": y - 20,
      "left": x,
      "position": "absolute",
      "font-weight": "bold",
      "color": "#ff6651"
    });
    $("body").append($i);
    $i.animate({
      "top": y - 180,
      "opacity": 0
    }, 1500, function () {
      $i.remove();
    });
  });
});
```

②修改我们的Fluid主题配置文件`_config.fluid.yml`，调用该js文件即可，向文件中`custom_js`参数中添加如下内容：

```javascript
custom_js: - /js/love-click.js # 鼠标点击特效
```

注意：如果有多个js文件需要调用，那么需要隔行逐一添加，这个是用户文档要求的，不仔细看容易踩坑。例如：

```javascript
custom_js: 
  - /js/love-click.js # 鼠标点击特效
  - /js/snow.js #雪花
```

</br>

**6.3、背景添加动态线条**

这是一个我很喜欢的特效，线条会慢慢向你的鼠标指向位置聚拢。[参考文章](https://cloud.tencent.com/developer/article/1943700)

像上一步一样，主题配置文件允许你直接调用js文件的url链接，那么我们直接像这样添加一个url，调用它即可。

```javascript
custom_js: 
  - /js/love-click.js # 鼠标点击特效
  - /js/snow.js #雪花
  - //cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js # 背景动态线条
```

</br>

**6.4、底部网站运行时间**

找到`D:\hexo\themes\fluid\layout\_partials`下的`footer.ejs`文件

直接在div盒子里添加如下代码，[参考文章](https://blog.csdn.net/qq_39720594/article/details/105411030)

```javascript
<!-- 运行时间 -->
  <span id="sitetime"></span>
  <script language=javascript>
  function siteTime(){
    window.setTimeout("siteTime()", 1000);
    var seconds = 1000;
    var minutes = seconds * 60;
    var hours = minutes * 60;
    var days = hours * 24;
    var years = days * 365;
    var today = new Date();
    var todayYear = today.getFullYear();
    var todayMonth = today.getMonth()+1;
    var todayDate = today.getDate();
    var todayHour = today.getHours();
    var todayMinute = today.getMinutes();
    var todaySecond = today.getSeconds();
    /* 
    Date.UTC() -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳)
    year - 作为date对象的年份，为4位年份值
    month - 0-11之间的整数，做为date对象的月份
    day - 1-31之间的整数，做为date对象的天数
    hours - 0(午夜24点)-23之间的整数，做为date对象的小时数
    minutes - 0-59之间的整数，做为date对象的分钟数
    seconds - 0-59之间的整数，做为date对象的秒数
    microseconds - 0-999之间的整数，做为date对象的毫秒数
        */
    var t1 = Date.UTC(2021,12,16,22,30,00); //北京时间
    var t2 = Date.UTC(todayYear,todayMonth,todayDate,todayHour,todayMinute,todaySecond);
    var diff = t2-t1;
    var diffYears = Math.floor(diff/years);
    var diffDays = Math.floor((diff/days)-diffYears*365);
    var diffHours = Math.floor((diff-(diffYears*365+diffDays)*days)/hours);
    var diffMinutes = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours)/minutes);
    var diffSeconds = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours-diffMinutes*minutes)/seconds);
    document.getElementById("sitetime").innerHTML=" 已运行"+/*diffYears+" 年 "+*/diffDays+" 天 "+diffHours+" 小时 "+diffMinutes+" 分钟 "+diffSeconds+" 秒";
  }
  siteTime();
</script>
```

