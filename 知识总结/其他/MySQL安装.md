**最新最全Windows下安装Mysql**

最近发现mysql安装和以前不一样了，这里给出详细教程

**一、下载MySQL**

首先，去数据库的官网[http://www.mysql.com](http://www.mysql.com/)下载MySQL。

然后在首页点击downloads，拉到最下面，点击MySQL Community (GPL) Downloads »(或者你们打开这https://dev.mysql.com/downloads/)

![mysql01](D:\个人资料\知识总结\图片\mysql\mysql01.png)

点击MySQL Community Server

![image-20200427133409468](D:\个人资料\知识总结\图片\mysql\image-20200427133409468.png)

按以下步骤

![image-20200427133614365](D:\个人资料\知识总结\图片\mysql\image-20200427133614365.png)

![image-20200427134125531](D:\个人资料\知识总结\图片\mysql\image-20200427134125531.png)



**二、安装过程**

**1.配置环境变量**

变量名：MYSQL_HOME

变量值：D:\software\mysql-8.0.19-winx64(1)\mysql-8.0.19-winx64

![mysql1](D:\个人资料\知识总结\图片\mysql\mysql1.jpg)

**2.生成data文件**

**以管理员身份运行cmd**

进入D:\software\mysql-8.0.19-winx64 (1)\mysql-8.0.19-winx64\bin>下

执行命令：mysqld --initialize-insecure --user=mysql  在D:\software\mysql-8.0.19-winx64 (1)\mysql-8.0.19-winx64\bin目录下生成data目录



**3.安装MySQL**

继续执行命令：mysqld -install

如果成功就会显示 Service successfully installed



**4.启动服务**

继续执行命令：net start MySQL

如果成功就会显示

MySQL 服务正在启动 ...

MySQL  服务已经启动成功。



**5.登录MySQL**

登录mysql:(因为之前没设置密码，所以密码为空，不用输入密码，直接回车即可）

执行命令：mysql -u root -p



**6.查询用户密码**

查询用户密码命令：

mysql> select host,user,authentication_string from mysql.user;



**7.设置（或修改）root用户密码**

mysql> use mysql

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';



**8.退出**

mysql> quit

Bye



**9.再次登录**

![1588122890(1)](D:\个人资料\知识总结\图片\mysql\1588122890(1).jpg)

MySQL安装就到此结束了

MySQL安装如果有其他问题的，可以看更全面的教程：

https://www.cnblogs.com/zhangkanghui/p/9613844.html

