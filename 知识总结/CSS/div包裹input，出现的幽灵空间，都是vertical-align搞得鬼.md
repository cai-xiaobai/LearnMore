## div包裹input，出现的幽灵空间，都是vertical-align搞得鬼

最近在做表单时发现了div和input框的位置有偏差，出现了幽灵空间，查阅过程找到大牛的文章（需要花比较长时间阅读）。

分享一下：https://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/



究其原因就是：

​	input 是内联元素，有默认的 vertical-align，因为这个影响到了baseline，就会出现下面的情况

![image-20200928095337961](D:\个人资料\LearnMore\知识总结\CSS\vertical1.png)

快速的解决办法就是：

​	1.div 的 line-height:0 或 font-size:0

​	2.input 的 vertical-align:top 或 bottom 或 super 或 text-top 或 text-bottom

​	3.或者使用定位