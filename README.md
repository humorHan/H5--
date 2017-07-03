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

另外一个坑的地方是取消ios默认的播放按钮样式：

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

     1. setInterval相比setTimeout计时更加准确。
     2. 在实现一般动画时，由于能自动将动画函数插入执行队列，实现起来更方便直观，不用重复设置计时器。
 setInterval的缺点：
     假设我们利用setInterval设置每5ms将move函数插入执行队列中，move函数由于计算量比较大，运行时间为6ms，计时器将move函数插入执行队列后，马          上又开始计时，那么move函数还未结束，计时器将会把第二个move函数插入执行队列中，导致move函数阻塞了执行队列。此时如果move函数中没有显式取消其自      身计时器的话，甚至可能会出现死循环。因此如果插入执行队列的函数计算量大的话（或者周期太小），就不适合选用setInterval。
setTimeout的优点：

     setTimeout实现动画不会阻塞执行队列。因为setTimeout本身就是一次性的，在实现动画时，我们在move函数的结尾处需要再设置一次计时器。因此无论          setTimeout设置了多少毫秒，假设为5ms，那么在两次move函数之间都一定会间隔开至少5ms。
setTimeout的缺点：
     在实现一般动画时，需要在函数最后再设置一次计时器。

```
 
