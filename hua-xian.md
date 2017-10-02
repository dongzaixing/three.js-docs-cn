# 线条绘制

假设你想绘制一根线条或者一个线圈,  不是线框网格. 首先需要设置渲染器, 场景和摄像机\(看创建一个场景那一节\)

这是要用的代码:

```js
var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

var camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 500);
camera.position.set(0, 0, 100);
camera.lookAt(new THREE.Vector3(0, 0, 0));

var scene = new THREE.Scene();
```

然后定义一个材质. 对于线条我们用`LineBasicMaterial`或者是`LineDashedMaterail`

```js
//create a blue LineBasicMaterial
var material = new THREE.LineBasicMaterial({ color: 0x0000ff });
```

在材质之后我们需要一个带有顶点的几何体\(Geometry\)或缓存几何体\(BufferGeometry\). 缓存几何体性能更高, 简单起见, 这里使用Geometry.

```js
var geometry = new THREE.Geometry();
geometry.vertices.push(new THREE.Vector3(-10, 0, 0));
geometry.vertices.push(new THREE.Vector3(0, 10, 0));
geometry.vertices.push(new THREE.Vector3(10, 0, 0));
```

注意, 相邻两个点之间会绘制线条,  但首尾两个点不会绘制. 所以默认线条是不封闭的.

既然我们有两条线的点和一个材质, 我们能将他们组合起来形成一个Line的实例\(线条的mesh\).

```js
var line = new THREE.Line(geometry, material);
```

这个line可以被添加到场景中, 然后调用渲染器的render方法:

```js
scene.add(line);
renderer.render(scene, camera);
```

你现在回看到一个由两条蓝色线条组成的指向上的箭头.





