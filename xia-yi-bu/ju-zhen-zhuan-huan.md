# 矩阵转换

Three.js使用矩阵来编码transform, 包括位移\(translate\), 旋转\(rotate\)和缩放\(scale\).

每一个Object3D的实例都有一个保存其transform\(位移, 旋转, 缩放\)的矩阵.这一节介绍如何更新一个物体的变换属性.

## 方便属性和矩阵自动更新\(matrixAutoUpdate\)

有两种方法可以更新一个物体的变换属性:

1, 修改物体的transform\(位置, 四元数, 以及缩放属性\), 然后让three.js从这些属性中重新计算物体的矩阵:

```js
object.position.copy(start_position);
object.quaternion.copy(quaternion);
```

默认情况, matrixAutoUpdate属性是true, 矩阵会自动重新计算. 如果物体是静态的, 为了提升性能, 你就需要手动控制是否重新计算.

```js
object.matrixAutoUpdate = false;
```

在修改任何属性只会, 手动更新矩阵:

```js
object.updateMatrix();
```



2, 直接修改物体的矩阵.  Matrix4 这个类有不同的方法可以修改矩阵:

```js
object.matrix.setRotationFromQuaternion(quaternion);

object.matrix.setPosition(start_position);
object.matrixAutoUpdate = false;
```

注意此时matrixAutoUpdate必须设置为false, 并且不能再调用 updateMatrix. 如果调用了updateMatrix会再次计算transform.

## 物体和世界坐标矩阵

一个物体的矩阵保存了相对于其父物体的transform. 要获得物体相对于世界坐标的transform, 则需要调用 Object3D.matrixWord

当父物体或子物体的transform改变之后, 你可以通过调用updateMatrixWord\(\) 来更新字符体的matrixWord



## 旋转和四元数\(Quaternion\)

Three.js提供两种方法来表示3D旋转: 欧拉角和四元数,  欧拉角易受到"万向锁"的影响, 这会导致配置中的角度可能无法自由控制. 基于这点, 物体旋转通常保存在四元数对象中.

之前的three.js版本中包含一个useQuaternion的属性, 如果是false, 将会从欧拉角中计算物体的矩阵. 现在此方法被弃用了, 可以使用setRotationFromEuler方法来更新四元数.













