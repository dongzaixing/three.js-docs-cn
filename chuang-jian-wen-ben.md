# 创建文本

在你的three.js应用中可能经常会用到字体, 这里有几种绘制字体的方法

### 1, DOM + CSS

使用HTML通常是最简单高效的字体绘制方法. 此方法常用来做three.js例子说明

```html
<div id="info">Description</div>
```

并且使用css设置其样式

```css
#info {
  position: absolute;
  top: 10px;
  width: 100%;
  text-align: center;
  z-index: 100;
  display:block;
}
```

### 2, 使用canvas绘制文字并作为纹理贴图\(Texture\)

如果你想简单绘制文字在一个平面上, 用此方法

### 3, 用你喜欢的3D软件创建文字模型并导到three.js

如果你偏爱3D软件, 用此方法

### 4, 程序化文字几何体

单纯使用three.js实现动态3D文字几何体, 你可以创建一个geometry是THREE.TextGeometry实例的mesh到场景:

```js
new THREE.TextGeometry( text, parameters );
```

为了能正常运行, 这个TextGeometry构造函数的参数需要一个THREE.Font的实例作为参数, 以便控制字体. 在TextGeometry那一节会介绍如何操作, 每种可用参数的描述以及THREE.js中包含的JSON格式字体的列表.

### 例子

[WebGL / geometry / text](https://threejs.org/examples/#webgl_geometry_text)

[canvas / geometry / text](https://threejs.org/examples/#canvas_geometry_text)

[WebGL / shadowmap](https://threejs.org/examples/#webgl_shadowmap)

如果你想使用更多字体, 这里有个blender中使用python脚本导出针对于three.js的JSON格式字体的制作教程:

[http://www.jaanga.com/2012/03/blender-to-threejs-create-3d-text-with.html](http://www.jaanga.com/2012/03/blender-to-threejs-create-3d-text-with.html)









