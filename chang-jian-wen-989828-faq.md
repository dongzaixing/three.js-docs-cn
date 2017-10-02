# 常见问题

#### 那种导入格式/导出器支持得最好?

TODO

#### 为什么例子中会有meta viewport标签?

```html
<meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
```

这个标签控制viewport的尺寸和缩放比例, 在移动端浏览器中页面可能呈现和viewport不同的大小.

[http://developer.apple.com/library/safari/\#documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html](http://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html)

[https://developer.mozilla.org/en/Mobile/Viewport\_meta\_tag](https://developer.mozilla.org/en/Mobile/Viewport_meta_tag)

#### 在创建resize的时候, 如何保证场景的缩放比例不变?

我们希望所有物体, 不管到摄像机的距离, 即使是窗口被resize. 解决这个问题的关键是设置一个等比例缩放的公式:

```js
visible_height = 2 * Math.tan( ( Math.PI / 180 ) * camera.fov / 2 ) * distance_from_camera;
```

如果我们增加window的高度一个特定的比例, 然后可见高度会增加同样的比例.

#### 为什么部分物体不可见?

这可能是因**face culling**.面的朝向\(面法线\)决定它的正反面, 在一个闭合的几何模型中,  背对摄像机的面是被面对摄像机的面遮挡住的, 所以无需渲染, 但如果面法线搞反了的话, 正常的面将不会被渲染, 解决方法是反转面法线, 或者设置材质的material.side为THREE.DoubleSide设置材质的双面显示.

```js
material.side = THREE.DoubleSide 
```







