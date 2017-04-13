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

## 阻止默认的tap颜色

```
-webkit-tap-highlight-color: rgba(0,0,0,0);
```
## 关于移动端video的标签

video标签是一个比较坑的地方，相关内容并没有统一。 主要是ios的video标签，是不能够进行自动播放，主动点击播放的方案如下：

```
<div class="video-wrapper" onclick="document.getElementById('vId').play()">
    <a href="javascript:void(0);" class="play-btn"></a>
    <video src="" id="vId"></video>
</div>
//这种方案能够实现，点击一个自定义的播放按钮，然后进行播放！ 只有video的父标签注册点击事件，在IOS下调用play事件才好使！！
```
当然，在微信下的话有更好的解决方案，其他终端待测试...

```
document.getElementById('audio').play();
//必须在微信Weixin JSAPI的WeixinJSBridgeReady才能生效
document.addEventListener("WeixinJSBridgeReady", function () {
    document.getElementById('audio').play();
}, false);
```

另外一个坑的地方是取消ios默认的播放按钮样式：

```
video::-webkit-media-controls-start-playback-button {
     display: none;
}
```

默认IOS的video标签是点击完成之后就会全屏播放的，IOS10中添加了，新的inline播放方式，设置如下：

```html
<video id="player" width="480" height="320" webkit-playsinline playsinline>
```
