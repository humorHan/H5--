## H5塌坑之旅--video/audio
###### video标签是一个比较坑的地方，相关内容并没有统一。

### IOS下video/audio 不能自动播放(如下三种办法)

    初级办法：

    给video(甚至是body)添加一个绑定事件，点击之后调用play方法

###### 缺点： 并没有从根本上解决问题，需要点击一下，影响用户体验

    进阶办法：

    document.addEventListener("WeixinJSBridgeReady", function () {

        document.getElementById('audio').play();

    }, false);

###### 缺点： 必须在微信Weixin JSAPI的WeixinJSBridgeReady才能生效

    终极办法：

    wx.getNetworkType({
        success: function (res) {
            document.getElementById('video').play();
            document.getElementById('video').pause();
        }
    });


### video的默认播放按钮问题

带控件的情况下，都有播放按钮(这个没什么问题)

不带控件的情况下，IOS仍然有播放按钮，安卓不带播放按钮。

使用如下取消即可

    video::-webkit-media-controls-start-playback-button {
         display: none;
    }

### 补充一下video的基础知识点

    <video id="player" x5-video-player-type="h5" x5-video-player-fullscreen="true" webkit-playsinline="true" playsinline="true">

    内联： webkit-playsinline="true" playsinline="true"

    同层： x5-video-player-type="h5" x5-video-player-fullscreen="true"

### 播放视频报错--提示没有找到资源

在点击播放视频回调里添加如下代码

    var video = document.getElementById("video");

    video.addEventListener("canplay", function(){

    	video.play();

    });

    //video.load();

### video 设置height的作用和区别

微信下，ios可以在设定内联播放，安卓是视频在中间播放.

比如一个320x180的视频，设置视频width: 100%(假设是320px)，

那么视频高度是180px,安卓下表现形式是在屏幕垂直居中180px高，

但是如果同时设置height: 100%,

那么视频高度是整个屏幕的高度，但是安卓下表现形式仍然是垂直方向居中高度看起来180px

    利用这一点即可实现：视频在中间播放 而且可以看到视频下层的dom做背景,代码如下：

    <video class="video" src="video/v1.mp4" preload="true" x5-video-player-type="h5" x5-video-player-fullscreen="true" webkit-playsinline="true" playsinline="true"></video>

    .video{
        width: 100%;
        height: 150%;
        position: absolute;
        top: -27%;
    }

### video/audio currentTime

    audio currentTime work

    video currentTime 微信下 全屏模式没有问题，非全屏下，安卓不好使


### X5内核监听 进入/退出全屏播放

    document.getElementById('video').addEventListener("x5videoexitfullscreen", function(){

        alert("exit fullscreen")

    })

    document.getElementById('video').addEventListener("x5videoenterfullscreen", function(){

        alert("enter fullscreen")

    })

### 去除视频黑边

可能会拉伸...其他属性值自行百度吧

```
    object-fit: fill
```
