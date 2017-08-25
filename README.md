# H5--
H5-塌坑之旅

## ios rotate3d问题

在 ios 中使用transform: rotate3d(0, 0, 1, -360deg) 不work，加上-webkit-也不work;

```
transform: rotate3d(0, 0, 1, -360deg);
-webkit-transform: rotate3d(0, 0, 1, -360deg);  

```
可以使用transform: rotateZ(-360deg)代替

## ios 弹性滚动问题

在 ios 的 滚动的父元素也就是设置overflow:auto;的元素上面，设置如下值：

```
-webkit-overflow-scrolling: touch; /* 当手指从触摸屏上移开，会保持一段时间的滚动 */

-webkit-overflow-scrolling: auto; /* 当手指从触摸屏上移开，滚动会立即停止 */
```

采用如上方式，比较适合是body的滚动，更好的方式是不用上面的属性，因为该属性在低版本的webkit浏览器中会产生不必要的屏幕闪烁，更好的实现方式，是采用div内的滚动方式，记得阻止默认的滚动事件进行冒泡。

## ios 部分机型 给body设置overflow：hidden不好用

## 阻止默认的tap颜色

```
-webkit-tap-highlight-color: rgba(0,0,0,0);
```
## 关于移动端video的标签

video标签是一个比较坑的地方，相关内容并没有统一。 主要是ios的video标签，是不能够进行自动播放，主动点击播放的方案如下：

##### 探索办法一：
```
<div class="video-wrapper" onclick="document.getElementById('vId').play()">
    <a href="javascript:void(0);" class="play-btn"></a>
    <video src="" id="vId"></video>
</div>
//这种方案能够实现，点击一个自定义的播放按钮，然后进行播放！ 只有video的父标签注册点击事件，在IOS下调用play事件才好使！！
```
当然，在微信下的话有更好的解决方案，其他终端待测试...

##### 进阶办法二
```
document.getElementById('audio').play();
//必须在微信Weixin JSAPI的WeixinJSBridgeReady才能生效
document.addEventListener("WeixinJSBridgeReady", function () {
    document.getElementById('audio').play();
}, false);
```
##### 终极办法

```
 wx.getNetworkType({
                success: function (res) {
                    document.getElementById('video').play();
                    document.getElementById('video').pause();
                }
           });

```

另外一个坑的地方是在不用controls控件的时候，ios依然存在默认的播放按钮样式：以下方法取消即可~

```
video::-webkit-media-controls-start-playback-button {
     display: none;
}
```

默认IOS的video标签是点击完成之后就会全屏播放的，IOS10中添加了，新的inline播放方式，顺带兼容全屏设置如下：

```html
<video id="player" width="480" height="320" x5-video-player-type="h5"
               x5-video-player-fullscreen="true" webkit-playsinline="true" playsinline="true">
```
## video 微信非全屏播放模式，ios可以在设定区域播放，安卓是视频在中间播放，上下都是黑的

```
    这个问题可以设置video height="100%"--这个就可以视频在中间播放，而且可以看到视频下层的dom

```
## 而且，所有异步的操作都不能引起视频的播放  
   点击事件 然后ajax或者定时器 之后再播放视频音频是不行的~

## user-select 禁止用户选中文本
	
	-webkit-user-select: none; /* Chrome, Opera, Safari */
    -moz-user-select: none; /* Firefox 2+ */
    -ms-user-select: none; /* IE 10+ */
    user-select: none; /* Standard syntax */
    
    
## 清除手机tap事件后element 时候出现的一个高亮
	 
	*{
		-webkit-tap-highlight-color: rgba(0,0,0,0);
	}   
	 
## IOS下部分机型不支持  background-attachment: fixed;

```
   #bg{
	background: url(../img/loading.jpg) no-repeat fixed center;
   } 

```
	
## onscroll 事件在ios非webkit内核不能实时监听到 是在touchend触发的。兼容性图谱如下链接
<code>
	<a href="https://tstatic.toptest.yidianzixun.com.ks3-cn-beijing.ksyun.com/public/files/A7965370-6C40-4A32-BB41-486A7B77AD911495678400078.png">IOS上onscroll问题</a>
</code>    

## 关于setTimeout和setInterval

 首先我不推荐在webapp上使用setTimeout和setInterval，言外之意 慎用，初次之外单独说一下setTimeout和setIntervl区别
 
``` 
 setInterval的优点：
     1. setInterval相比setTimeout计时更加准确--严格按照定时时间执行【也正因如此，如果函数比较耗时，达到时间以后js执行阻塞dom渲染引起‘卡顿’】
     2. 在实现一般动画时，由于能自动将动画函数插入执行队列，实现起来更方便直观，不用重复设置计时器。
 setInterval的缺点：
     假设我们利用setInterval设置每5ms将move函数插入执行队列中，move函数由于计算量比较大，运行时间为6ms，计时器将move函数插入执行队列后，
     马上又开始计时，那么move函数还未结束，计时器将会把第二个move函数插入执行队列中，导致move函数阻塞了执行队列。此时如果move函数中没有显式取消
     其自身计时器的话，甚至可能会出现死循环。因此如果插入执行队列的函数计算量大的话（或者周期太小），就不适合选用setInterval。
     
setTimeout的优点：
     setTimeout实现动画不会阻塞执行队列。因为setTimeout本身就是一次性的，在实现动画时，我们在move函数的结尾处需要再设置一次计时器。因此无论          setTimeout设置了多少毫秒，假设为5ms，那么在两次move函数之间都一定会间隔开至少5ms【这样就可以计算完了生成新的setTimeout，从而不阻塞dom】
setTimeout的缺点：
     在实现一般动画时，需要在函数最后再设置一次计时器。

```
 
 ##  输入框在页面底部的时候，弹起的虚拟输入键盘会挡住输入框--scrollIntoView完美解决

```
着重介绍下兼容性最好的解决方案--scrollIntoView
1、scrollIntoView(alignWithTop)  滚动浏览器窗口或容器元素，以便在当前视窗的可见范围看见当前元素。如果alignWithTop为true，或者省略它，窗口会尽可能滚动到自身顶部与元素顶部平齐。-------目前各浏览器均支持
eg：document.getElementById("test").scrollIntoView(true);

扩展： scrollIntoViewIfNeeded、scrollByLines、scrollByPages由于目前只有safari和谷歌支持，所以暂时不做详细介绍

```


## IOS手机上有时候input的placeholder属性不好用（不要问我什么时候，反正玩了这么久就遇到过这一次，还是时而好用时而不好用，难道是项目太大加载问题？）

```
   IOS下某SB情况：时而placeholder好用，时而不好用，但是点击一下input就出现了placeholder的提示语，多次尝试之后怀疑系统默认认为input是非空导致（猜），然后点击之后内部触发监测input是否为空，发现是空的，然后显示了提示，这样解释也是合情合理的。（其实估计系统在渲染其他的dom等，没检测到input是否为空，我在页面首次渲染之后js设置input为空，所以可以触发对input是否为空的判断）
   
   解决办法：页面加载完成后 js设置input为空
   
```
## IOS上设置iOS图标  apple-touch-icon 图片自动处理成圆角和高光等效果。apple-touch-icon-precomposed 禁止系统自动添加效果，直接显示设计原图。
```
iOS 图标大小在iPhone 6+上是180×180，iPhone 6 是120×120  and so on...
适配iPhone 6+ <link rel="apple-touch-icon-precomposed" sizes="180x180" href="retinahd_icon.png">

```
## 微信上，安卓机型，横向布局的H5中，竖屏旋转为横屏时(未锁屏的横屏)，末尾出现空白
```
 始终也是理解不了的问题，各种调试过后，决定开大招--旋转时，动态添加需要绘制的dom元素，结果显示正常。
 注： 如果还是不OK，且你用了 onorientationchange 那么请自觉更改为原始方法--onresize吧，虽然查了没什么兼容问题，BUT 不换不行
 
```

