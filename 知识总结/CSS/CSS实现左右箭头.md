# CSS实现左右箭头

效果图：

![image-20200514153219089](D:\个人资料\LearnMore\知识总结\CSS\cssArrow.png)

实效果的css样式如下

```html
//CSS样式
<style>
.arrow-bg {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    -moz-border-radius: 50%;
    -webkit-border-radius: 50%;
    margin: 8% 0;
}

.left {
    position: relative;
    top: 27%;
    left: 31%;
    width: 20px;
    height: 20px;
    border-top: 3px solid #fff;
    border-right: 3px solid #fff;
    transform: rotate(-135deg);
}

.right {
    position: relative;
    top: 27%;
    left: 18%;
    width: 20px;
    height: 20px;
    border-top: 3px solid #fff;
    border-right: 3px solid #fff;
    transform: rotate(45deg);
}
</style>
//左箭头
<div class="medals arrow-bg">
	<div class="left"></div>
</div>

//右箭头
<div class="medals arrow-bg">
	<div class="right"></div>
</div>
```

