# 如何更新物体

如果使用以下代码添加到场景中, 所有物体默认会自动更新他们的变换矩阵:

```js
var object = new THREE.Object3D;
scene.add( object );
```

或者是他们被设置为一个物体的子物体, 然后被添加到场景中:

```js
var object1 = new THREE.Object3D;
var object2 = new THREE.Object3D;

object1.add( object2 );
scene.add( object1 ); //object1 and object2 will automatically update their matrices
```

然而, 如果你知道物体是静态的, 可以手动关闭他的变换矩阵更新.

```js
object.matrixAutoUpdate  = false;
object.updateMatrix();
```

## 几何体

#### BufferGeometry

BufferGeometry保存信息\(比如顶点位置, 面索引, 发现, 颜色和UV, 以及其他自定义属性\)到缓存中, 这里用到了类型数组\(typed arrays\). 这样的性能高于标准的Geometry, 但难度会更大一些.

关于更新BufferGeometry, 最重要的事是理解如何修改buffer\(这点非常昂贵, 基本等于创建一个新的几何体\). 你可以更新buffer的内容任意值.

这意味着如果你知道BufferGeometry的某个属性值会增加, 假设是顶点数量, 你必须提前预分配好足够大的缓存存放新顶点的信息. 当然, 也会设置一个对于BufferGeometry的最大尺寸.  无法创建一个无限扩展的BufferGeometry.

使用这个绘制线条渲染时对其扩展的例子. 我们在渲染的时候给500个顶点分配buffer空间. 使用 BufferGeometry.drawRange.

```js
var MAX_POINTS = 500;

// geometry
var geometry = new THREE.BufferGeometry();

// attributes
var positions = new Float32Array( MAX_POINTS * 3 ); // 3 vertices per point
geometry.addAttribute( 'position', new THREE.BufferAttribute( positions, 3 ) );

// draw range
var drawCount = 2; // draw the first 2 points, only
geometry.setDrawRange( 0, drawCount );

// material
var material = new THREE.LineBasicMaterial( { color: 0xff0000, linewidth: 2 } );

// line
var line = new THREE.Line( geometry,  material );
scene.add( line );
```

下面随机添加一些点到线上面:

```js
var positions = line.geometry.attributes.position.array;

var x, y, z, index;
x = y = z = index = 0;

for ( var i = 0, l = MAX_POINTS; i < l; i ++ ) {

    positions[ index ++ ] = x;
    positions[ index ++ ] = y;
    positions[ index ++ ] = z;

    x += ( Math.random() - 0.5 ) * 30;
    y += ( Math.random() - 0.5 ) * 30;
    z += ( Math.random() - 0.5 ) * 30;

}
```





















