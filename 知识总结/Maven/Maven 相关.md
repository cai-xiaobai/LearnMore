# Maven 相关

### 使用命令向导一步步创建项目

1）在硬盘上创建一个空的目录，用来存放Maven项目，如E:\temp\demoMaven。

2）打开 CMD 窗口，用 cd 命令，切换到 demoMaven 目录

比如一般有如下目录。

3）在 CMD 窗口中输入“mvn archetype:generate”，按 Enter 键。

联网初始化一段时间后（一般不少于 5 分钟），会一步步提示输入 groupId、artifactId、version、packageName 等信息。最后创建成功，而且可以在 E:\temp\demoMaven 空目录下发现一个同 artifactId 一样的目录，这就是创建的项目目录。



和项目相关的信息包括 groupId（组 Id）、artifactId（构件 Id）、packageName（包名）、version（版本）。

其实 packageName 和 version 好理解。程序员写的类，肯定要放在一个标准包下或标准包的子包下，packageName 指标准包；version 是当前代码的版本号。

groupid表示项目的包名,artifactid表示项目名 

这里的 groupId 和 artifactId 同部门名称和组名称一样，用来唯一确定一个项目（软件、功能）。有些地方会把这两个描述的信息合起来叫“坐标”。





- src\main\java，用来存放项目的 [Java](http://c.biancheng.net/java/) 源代码。
- src\main\resources，用来存放项目相关的资源文件（比如配置文件）。
- src\test\java，用来存放项目的测试 Java 源代码。
- src\test\resource，用来存放运行测试代码时所依赖的资源文件。


当然，还有一个 pom.xml 文件，该文件配置 Maven 管理的所有内容。



1. 输入“mvn clean”，按 Enter 键清空以前编译安装过的历史结果。
2. 输入“mvn compile”，按 Enter 键编译源代码。
3. 输入“mvn test”，按 Enter 键运行测试案例进行测试。
4. 输入“mvn install”，按 Enter 键，将当前代码打成 jar 包，安装到 Maven 的本地管理目录下，其他 Maven 工程只要指定坐标就可以使用。





使用IDEA创建Maven

https://blog.csdn.net/weixin_39209728/article/details/85853516 



按照Tomcat

https://www.cnblogs.com/weixinyu98/p/9822048.html