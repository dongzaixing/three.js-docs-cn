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

关于更新BufferGeometry, 最重要的事是理解如何修改buffer\(这点非常昂贵, 基本等于创建一个新的几何体\). 但你可以修改buffer的内容.

这意味着如果你知道BufferGeometry的某个属性值会增加, 假设是顶点数量, 你必须预先分配好足够大的buffer来存放随时可能新增的顶点信息. 当然, 也会设置一个对于BufferGeometry的最大尺寸.  无法创建一个无限扩展的BufferGeometry.

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

然后会使用下面的方式随机添加一些点到线上:

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

如果你想在第一次渲染之后改变点的数量, 这样做:

```js
line.geometry.setDrawRange( 0, newValue );
```

如果你想在第一次渲染之后改变位置信息, 可以设置 needsUpdate 这个标志\(flag\):

```js
line.geometry.attributes.position.needsUpdate = true; // required after the first render
```

这里是[fiddle](http://jsfiddle.net/w67tzfhx/)上一个动画线条的demo, 你可以根据你的案例来

#### 例子:

[WebGL / custom / attributes](https://threejs.org/examples/#webgl_custom_attributes)

[WebGL / buffergeometry / custom / attributes / particles](https://threejs.org/examples/#webgl_buffergeometry_custom_attributes_particles)

#### Geometry

下面的这些flag控制几何体的更新. 设置它们可以控制对应属性是否更新, 更新是非常昂贵的, 一旦buffer改变了, 所有的flag会自动设置为false. 如果你想保持更新buffer的话就需要让它们保持为true.  注意这只针对于Geometry 而不是 BufferGeometry.

```js
var geometry = new THREE.Geometry();
geometry.verticesNeedUpdate = true;
geometry.elementsNeedUpdate = true;
geometry.morphTargetsNeedUpdate = true;
geometry.uvsNeedUpdate = true;
geometry.normalsNeedUpdate = true;
geometry.colorsNeedUpdate = true;
geometry.tangentsNeedUpdate = true;
```

在[r66](https://github.com/mrdoob/three.js/releases/tag/r66)版本之前需要单独的开启dynamic这个flag

```js
//removed after r66
geometry.dynamic = true;
```

#### 例子:

[WebGL / geometry / dynamic](https://threejs.org/examples/#webgl_geometry_dynamic)

## 材质\(Materials\)

所有统一格式的值都可以随意修改, 例如colors, textures, opacity等. 这些值都会在每一帧发送给shader.

GLstate\(Graphic Library state, 图形库状态\)相关的参数可以在任何时候被修改, 比如景深测试\(depthTest\), 合成\(blending\), 多边形偏移\(PolygonOffset\)等等.

平坦/光滑的着色会被烘焙成法线, 你需要重设法线缓存.

下面的属性不能再运行时被随意修改

* uniform的数量和类型
* 灯光的数量和类型
* 下面属性是否存在:
  * 纹理
  * 灯光雾
  * 顶点颜色
  * 皮肤
  * 渐变
  * 阴影贴图
  * alpha通道测试

修改上面这些属性需要按下面方式重建整个shader程序:

```js
material.needsUpdate = true
```

注意, 这可能导致帧率降低, 尤其是在windows上, 因为在DirectX中的shader编译比OpenGL慢.

为了提升流畅度, 你可以用一个虚假的值去模拟, 比如0强度的灯光, 白色纹理, 或者0强度的灯光雾.

你可以随意更改几何体chunk使用的材质, 然而你不能设置一个对象如何拆分为多个chunks

如果你想要在运行时修改材质的不同配置:

若材质或chunk的数量很少, 你可以预拆分\(例如人的头发/面部/身体/上衣/裤子, 或者汽车的前面/侧面/顶部/玻璃/轮胎/内部\).

如果数量很多\(每个面可能不同材质\), 考虑另一种解决方案, 例如使用属性/纹理来表现不同面的材质.

#### 例子:

[WebGL / materials / cars](https://threejs.org/examples/#webgl_materials_cars)

[WebGL / webgl\_postprocessing / dof](https://threejs.org/examples/#webgl_postprocessing_dof)

## 纹理\(Textures\)

图片, canvas, 视频和数据纹理需要用以下flag设置是否修改:

```js
texture.needsUpdate = true;
```

此时渲染目标会自动更新.

#### 例子

[WebGL / materials / video](https://threejs.org/examples/#webgl_materials_video)

[WebGL / rtt](https://threejs.org/examples/#webgl_rtt)

## 摄像机

摄像机的位置和目标会自动更新, 如果你需要改变以下属性:

* 视场角\(fov\)
* 宽高比\(aspect\)
* 近裁剪\(near clipping plane\)
* 远裁剪\(far clipping plane\)

然后你需要重新计算投射矩阵:

```js
camera.aspect = window.innerWidth / window.innerHeight;
camera.updateProjectionMatrix();
```















