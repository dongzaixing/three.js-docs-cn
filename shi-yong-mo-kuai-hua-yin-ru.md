# 使用模块化引入

通过script标签引入three.js是个很快捷的好方法, 但也有很多缺点, 例如:

* 必须手动拉取或引入一个文件作为项目的源码
* 更新库的版本是个手动操作的过程
* 在每次检查库diff的时候会出现很多杂乱的行

使用像npm这样的依赖管理器运行你简单的下载和引入想要的版本.

## 使用npm安装

three.js发布在[npm](https://www.npmjs.com/package/three)服务器上, 意味着可以直接用`npm install three` 安装并且直接引入

## 引入模块

假设你在用webpack或者browserify对项目进行打包. 这两个打包工具运行你用`require('modules')` 打包所有依赖包. 

现在可以用下面代码这样书写:

```js
var THREE = require('three');

var scene = new THREE.Scene();
...
```

也可以用ES6 模块语法:

```js
import * as THREE from 'three';

const scene = new THREE.Scene();
...
```

或者你只想引入three.js中的部分模块:

```js
import { Scene } from 'three';

const scene = new Scene();
...
```

## 警告

现在是不能直接引入`"examples/js"`  目录中的文件. 因为一些文件是依赖于全局命名空间THREE. [Transform \`examples/js\` to support modules \#9562](https://github.com/mrdoob/three.js/issues/9562) 查看更多信息























