# H5--
H5-塌坑之旅

## ios rotate3d问题

在 ios 中使用transform: rotate3d(0, 0, 1, -360deg) 不work，加上-webkit-也不work;

```
transform: rotate3d(0, 0, 1, -360deg);
-webkit-transform: rotate3d(0, 0, 1, -360deg);  

```
可以使用transform: rotateZ(-360deg)代替
