# Navicat Premium 出现的Missing required libary sqlite.dll,998问题，解决方案

准备查看数据库，双击Navicat Premium，然后就：

Missing required libary sqlite.dll,998

查询了百度后的解决方法都是，可能是与360安全卫士，腾讯电脑管家冲突有关，将360安全卫士. 腾讯电脑管家、关闭即可使用了。

**但是我根本没安装任何杀毒软件呀！！！我卸载重装一样报错**

搜查无果，决定查查为何出现，极有可能我没安装微软常用的运行库。

安装好微软库后就可以了！

下面给出最便宜，最方便的链接大家下载微软库，

**https://download.csdn.net/download/qq_43850819/12371795**



使用工具：Navicat Premium
问题：Missing required libary sqlite.dll,998
解决方案如下：

1. 查看计算机中是否有360安全卫士，或者腾讯电脑管家。
2. 如果有，则关闭。否则，重启计算机。
3. 关闭后，启动Navicat Premium，如果还是报错，下载常用的微软库。
4. 如果还是不能成功，卸载Navicat Premium，重新安装。
5. 如果还是报错，请继续全网搜吧！