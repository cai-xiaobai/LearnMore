# Maven更新失败，Cannot resolve plugin org.apache.maven.plugins:maven-deploy-plugin:X.X

在接受新项目，使用 <font color="blue">maven</font> 更新的时候，偶尔会遇到包无法下载到本地 <font color="blue">maven</font>  仓库，就会报：（这里以2.7为例，其他也是同理）

<font color="red">Cannot resolve plugin org.apache.maven.plugins:maven-deploy-plugin:2.7</font>

解决办法：

首先打开 idea ->setting -> Build,Execution,Deployment -> Build Toos -> Maven

![image-20200925104519560](D:\个人资料\LearnMore\知识总结\Maven\error1.png)

![image-20200925104722541](D:\个人资料\LearnMore\知识总结\Maven\error2.png)

找到你的 Local repository

到本地文件夹中跟着以下地址

\\.m2\repository\org\apache\maven\plugins\maven-deploy-plugin\2.7

将此文件夹的内容清空

然后重新 执行 clean、install、更新maven仓库，右边红色波浪消失后，此时就能愉快的运行程序了



出处：https://blog.csdn.net/Frankenstein_/article/details/101288295