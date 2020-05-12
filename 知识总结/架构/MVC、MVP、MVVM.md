MVC、MVP、MVVM

M：Model层，存储数据

V：View层，展示数据



MVC

C：Controller控制层，接受用户所有的操作，并根据写好的代码进行相应的操作

Controller控制层触发View层时，并不会更新View层中的数据，View层中的数据是通过监听Model层数据变化而自动更新的，与Controller层无关。

<img src="D:\个人资料\知识总结\图片\MVC流程图.jpg" alt="MVC流程图" style="zoom:40%;" />

缺点：MVC框架大部分逻辑集中在Controller层，这带给Controller层很大压力，而已经有独立处理事件能力的View层 却没有用到。还有，Controller层和View层之间是一一对应，断绝了View层复用，产生很多冗余代码。



MVP

P：Presenter层

View层不能直接访问Model层，必须通过Presenter层提供的接口，然后Presenter层再去访问Model层。因为View层与Model层之间没有关系，所以View层可以抽离出来做成组件，复用性比MVC模型好。

<img src="D:\个人资料\知识总结\图片\MVP.jpg" alt="MVP" style="zoom:40%;" />

缺点：Presenter层比较复杂，维护起来会有一定的问题，而且没有绑定数据，所有数据都需要Presenter层进行"手动同步"



MVVM

VM：ViewModel层，View层和Model层之间的修改都会同步到对方。

<img src="D:\个人资料\知识总结\图片\MVVM.jpg" alt="MVVM" style="zoom:40%;" />

MVVM模型中数据绑定方法一般有以下3种：

1.数据劫持

2.发布-订阅模式

3.脏值检查