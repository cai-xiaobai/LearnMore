## Swiper3 导航栏的轮播

最近迭代旧项目开发移动端，使用 swiper3 制作一个拥有的导航栏的轮播。

单独截取的代码可能跑不起来（主要看 js 的逻辑）

先看效果：

![image-20200929172945441](D:\个人资料\LearnMore\知识总结\CSS\Swiper\swiper3-1.png)

上代码：

记得提前引入 swiper3 的文件

swiper_v3.4.2/swiper.min.css

swiper_v3.4.2/swiper.min.js

还有 jquery 的 也是 3.0版本
js

```js
<script>
<%--  轮播  --%>
$(document).ready(function(){

    // 初始化swiper轮播插件
    var mySwiper = new Swiper ('.swiper-container', {
        direction: 'horizontal', // 横向切换选项
        onTransitionStart: function(swiper){//回调函数，swiper从当前slide开始过渡到另一个slide时执行。
            var index = swiper.activeIndex; //当前活动块的索引
            $(".nav li").eq(index).addClass("active").siblings().removeClass("active");//根据活动块的索引切换每个li的字体颜色
            $(".line-contain").css({transform:"translateX("+ 1.68*index +"rem)"});
        }
    })
    //  获取到导航条的所有li
    $(".nav li").click(function(){
        // 获取到当前点击li的索引值
        var index = $(this).index();
        mySwiper.slideTo(index, 1000, false);//切换到第一个slide，速度为1秒
        $(this).addClass("active").siblings().removeClass("active")
        $(".line-contain ").css({transform:"translateX("+ 1.68*index +"rem)"});
    })

})
</script>
```

html

```html
<div class="swiper">
        <div class="wrap">
            <div class="nav">
                <ul>
                    <li class="active">Thetitle1</li>
                    <li>Thetitle2</li>
                    <li>Thetitle3</li>
                    <li>Thetitle4</li>
                </ul>
            </div>
            <div class="line-contain">
                <div class="line"></div>
            </div>
            <div class="swiper-container">
                <div class="swiper-wrapper">
                    <div class="swiper-slide">
                        <div class="swiper-txt">
                            slide1
                        </div>
                        <img src="/phone/1.png" width="100%" />
                    </div>
                    <div class="swiper-slide">
                        <div class="swiper-slide">
                            <div class="swiper-txt">
                                slide2
                            </div>
                        </div>
						<img src="/phone/1.png" width="100%" />
                    </div>
                    <div class="swiper-slide">
                        <div class="swiper-slide">
                            <div class="swiper-txt">
                                slide3
                            </div>
                            <img src="/phone/1.png" width="100%" />
                        </div>
                    </div>
                    <div class="swiper-slide">
                        <div class="swiper-slide">
                            <div class="swiper-txt">
                                slide4
                            </div>
							<img src="/phone/1.png" width="100%" />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
```

css

```css
<style>
		html {font-size: 13.4vw; }
		.swiper { height: 7.82rem;margin: .98rem 0 .71rem 0; }
		.wrap{width: 6.9rem;height: .4rem;line-height:.4rem;font-size:.32rem;margin: 0 auto;}
		.nav{width: 100%;display: flex;}
		.nav ul{flex: 1;display: flex;align-items: center;justify-content: space-between;}
		.nav li.active{color:#1b56eb;font-size:.4rem;}
		.line-contain { width:1.68rem;display: flex;justify-content: center }
		.line { width: .32rem;height: .06rem;border-radius: .3rem;background-color: #1b56eb;margin-top: .3rem; }
		.swiper-container { margin-top: .4rem; }
		.swiper-slide .swiper-txt{ height: 1.85rem;font-size:.24rem;line-height: .4rem; }
		.swiper-slide img { margin-top: .5rem; }
	</style>
```



