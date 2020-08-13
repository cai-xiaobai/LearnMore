## 傻瓜式玩阿里云服务ECS

[TOC]

纯小白没有服务器的知识也能配置着玩。

因为学生的关系在阿里云白嫖了一个云服务器，想着不玩白不玩，刚好做完手头上的工作，开干

### 找到打开你的云服务器

登录阿里云以后，点开左上角的目录，选择你的云服务器。（老实说这么多术词，好像对小白有点不友善，可能是我太菜）

![1.0](D:\个人资料\LearnMore\知识总结\云服务器\1.0.png)

实例就是你的云服务了！

![1.1](D:\个人资料\LearnMore\知识总结\云服务器\1.1.png)

网上有很多让你用windows的远程连接，去连接你的服务器，其实阿里云都帮你做好了。点击这里的远程连接

![1.2](D:\个人资料\LearnMore\知识总结\云服务器\1.2.png)
选择第一个
![1.3](D:\个人资料\LearnMore\知识总结\云服务器\1.3.png)

看到有新网页，恭喜你找到你的服务器啦

### 登录服务器并配置宝塔

接下来便是登录你的服务器啦

![1.4](D:\个人资料\LearnMore\知识总结\云服务器\1.4.png)

在回到刚刚点击远程连接的页面，这里会有管理

![1.5](D:\个人资料\LearnMore\知识总结\云服务器\1.5.png)

进到页面点击重置实例密码，改好密码，然后回去输入密码

![1.6](D:\个人资料\LearnMore\知识总结\云服务器\1.6.png)

到这你就成功和你的服务器会师了

![1](D:\个人资料\LearnMore\知识总结\云服务器\1.png)

接下来根据你的不同版本的服务器，输入命令行安装

使用 SSH 连接工具，如[堡塔SSH终端](https://download.bt.cn/ssh/BT-Term.exe)连接到您的 Linux 服务器后，[挂载磁盘](https://www.bt.cn/bbs/thread-5166-1-1.html)，根据系统执行相应命令开始安装（大约2分钟完成面板安装）：

Centos安装脚本 

```shell
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh d3640e
```

Ubuntu/Deepin安装脚本 

```shell
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh d3640e
```

Debian安装脚本 

```shell
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh d3640e
```

Fedora安装脚本 

```shell
wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh d3640e
```

[安装的详细教程点这！](https://www.bt.cn/bbs/thread-19376-1-1.html)

接下来一路安装就好了

![2](D:\个人资料\LearnMore\知识总结\云服务器\2.png)

然后输入你的服务器ip加8888端口号进入宝塔面板

![3](D:\个人资料\LearnMore\知识总结\云服务器\3.png)

目前我就玩到这里了，等我学点再慢慢玩，下面给一些想继续深入的链接，各位看官接好



### 相关深入链接

宝塔面板手册：https://www.kancloud.cn/chudong/bt2017/

安装完宝塔面板，无法访问面板，部分功能不能使用的解决办法：https://blog.csdn.net/youths/article/details/92581183

阿里云说明文档：https://help.aliyun.com/learn/getting-started.html

Jenkins快速安装：https://www.daniao.org/6358.html

宝塔

登录：oxycpdcp

密码：d25c49f1