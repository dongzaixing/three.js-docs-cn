# 创建一个场景

本节的内容主要是对three.js做一个简单的介绍. 我们将开始设置一个包含旋转立方体的场景. 下面会提供一个可以运行的例子以免你遇到问题.

## 开始之前

在开始使用three.js之前, 你需要一个显示它的地方. 保存下面的HTML代码到文件中, 然后复制[three.js](http://threejs.org/build/three.js)文件到js目录中, 最后用浏览器打开这个文件.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset=utf-8>
        <title>My first three.js app</title>
        <style>
            body { margin: 0; }
            canvas { width: 100%; height: 100% }
        </style>
    </head>
    <body>
        <script src="js/three.js"></script>
        <script>
            // Our Javascript will go here.
        </script>
    </body>
</html>
```

就这样. 所有的js代码都会放到哪个空的script标签中.

## 创建场景

实际上可以用three.js显示任何东西, 我们需要三样东西: 一个场景, 一个摄像机, 一个渲染器. 所以我们可以用摄像机渲染场景.

```js
var scene = new THREE.Scene();
var camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

var renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );
```

现在解释一下原理.  在three.js中有很多种不同的摄像. 现在我们用的是透视摄像机\(PerspectiveCamera\). 第一个属性是视场角\(field of view\).

第二个参数是宽高比例\(aspect ratio\). 也就是是屏幕宽度除以高度的比例, 如果比例不对在宽屏电视上可能会有压扁的感觉.

剩下两个参数分别是近裁剪平面\(near clipping plane\)和远裁剪平面\(far clipping\)的距离. 意思是只渲染这个范围内的模型,  这是可以提升渲染性能的参数.

接下来介绍渲染器. 这才是神起的地方. 这里使用的WebGLRenderer, three.js包含一些其他渲染器, 通常都对一些使用不支持Webgl的老浏览器的用户做出一些降级处理.

除了创建一个渲染器实例, 还要设置输出分辨率. 使用可视区域的宽高是个不错的选择, 这里设置了浏览器窗口的宽高作为输出尺寸, 如果考虑性能, 可以将其设置为宽高的1/2

如果你想讲输出1/2的分辨率, 且保证图像不变小, 可以在setSize方法的第三个参数传入false, 这回在渲染之后给&lt;canvas&gt;加上100%的css宽高属性.

最后一步是将渲染器生成的canvas节点添加到dom.

```js
var geometry = new THREE.BoxGeometry( 1, 1, 1 );
var material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
var cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;
```

要创建一个立方体模型, 需要用到BoxGeometry类. 实例化之后的geometry对象包含立方体模型点和面信息. 以后会详细介绍.

除了几何体, 还需要材质对象对其添加颜色. three.js包含一些基本材质, 这里使用 MeshBasicMaterial材质. 所有材质类实例化的时候都会传一些属性给它. 简单起见, 传了一个值为0x00ff00的颜色属性进去, 表示16进制的绿色.

然后需要一个Mesh. mesh是一个包含几何体信息和材质信息的对象, 可以直接将其插入到场景中, 并且能自由移动.

通常, 调用 scene.add\(\)会添加模型到坐标\(0, 0, 0\)的位置, 这会导致摄像机和立方体重叠, 这场把摄像机向外面移动一点避免这个问题.

## 渲染场景

如果你只是复制了上面的代码到HTML文件中将不会看到任何东西. 这是因为还没有使用渲染器的render方法. 现在将它渲染出来, 并且做一个小动画.

```js
function animate() {
    requestAnimationFrame( animate );
    renderer.render( scene, camera );
}
animate();
```

这里用requestAnimationFrame创建了一个每秒60帧的动画, 你可能会问为什么不用setInterval, 因为requestAnimationFrame是现代浏览器提供的针对于动画的新api, 有很多优势, 比如切换浏览器tab之后会暂停, 去掉不必要的重排重绘, 省电等等.

## 让立方体动起来

如果只复制上面的代码只会看到一个静态的绿色立方体, 现在添加个有趣的旋转动画

```js
cube.rotation.x += 0.1;
cube.rotation.y += 0.1;
```

这会在每一帧中运行, 不断更新立方体的角度. 任何你想在app运行之后改变的属性都需要放到animatie循环中. 

## 最后

祝贺你完成了第一个three.js应用. 虽然简单, 但千里之行始于足下.

下面是完整的代码, 运行它可以更好地理解整个流程:

```html
<html>
    <head>
        <title>My first three.js app</title>
        <style>
            body { margin: 0; }
            canvas { width: 100%; height: 100% }
        </style>
    </head>
    <body>
        <script src="js/three.js"></script>
        <script>
            var scene = new THREE.Scene();
            var camera = new THREE.PerspectiveCamera( 75, window.innerWidth/window.innerHeight, 0.1, 1000 );

            var renderer = new THREE.WebGLRenderer();
            renderer.setSize( window.innerWidth, window.innerHeight );
            document.body.appendChild( renderer.domElement );

            var geometry = new THREE.BoxGeometry( 1, 1, 1 );
            var material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
            var cube = new THREE.Mesh( geometry, material );
            scene.add( cube );

            camera.position.z = 5;

            var animate = function () {
                requestAnimationFrame( animate );

                cube.rotation.x += 0.1;
                cube.rotation.y += 0.1;

                renderer.render(scene, camera);
            };

            animate();
        </script>
    </body>
</html>
```

















