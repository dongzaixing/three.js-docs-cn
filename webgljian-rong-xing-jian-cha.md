# WebGL兼容性检查

即使现在越来越多浏览器支持WebGL, 但还是有些设备的浏览器不支持. 下面的方法可以检查浏览器的支持情况并且展示一些信息提示用户.

在使用three.js之前添加这段脚本[https://github.com/mrdoob/three.js/blob/master/examples/js/Detector.js](https://github.com/mrdoob/three.js/blob/master/examples/js/Detector.js)

```js
if (Detector.webgl) {
    // Initiate function or other initializations here
    animate();
} else {
    var warning = Detector.getWebGLErrorMessage();
    document.getElementById('container').appendChild(warning);
}
```



