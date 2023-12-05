---
title: 几天的Three.js 学习记录（第一篇章）
date: 2023-12-5 11:10:12
tags:
- Three.js
- JavaScript
categories: 
- 学习笔记
---

### 1. Three.js 文件包下载和目录简介

下载 Three.js 文件包，解压并查看目录结构。示例代码：

```shell
# 下载 Three.js 文件包
wget https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js
# 解压文件
unzip three.min.js
# 查看目录结构
ls -l
```

### 2. 学习环境 — 编辑器和本地静态服务

选择合适的代码编辑器（如Visual Studio Code）和本地静态服务器（如Live Server插件）搭建 Three.js 学习环境。

### 3. 开发和学习环境，引入 Three.js 库

在 HTML 文件中引入 Three.js 库，并创建基本的 Three.js 场景。示例代码：

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Three.js Scene</title>
  <script src="path/to/three.min.js"></script>
</head>
<body>
  <script>
    // 创建 Three.js 场景
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);
  </script>
</body>
</html>
```

### 4. 第一个 3D 案例 — 创建 3D 场景

创建最简单的 Three.js 3D 场景，包括场景、相机和渲染器，并渲染一个基本几何体。示例代码：

```javascript
// 创建 Three.js 场景
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// 创建一个立方体并添加到场景中
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

camera.position.z = 5;

function animate() {
  requestAnimationFrame(animate);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
animate();
```

### 5. 第一个 3D 案例 — 透视投影相机

学习如何使用透视投影相机创建 Three.js 场景。示例代码：

```javascript
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
```

### 6. 第一个 3D 案例 — 渲染器

创建 Three.js 渲染器，并将其绑定到 HTML 元素上。示例代码：


```javascript
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

### 7. Three.js 三维坐标系 — 加强对三维空间的认识

探索 Three.js 的三维坐标系，加深对三维空间的理解。

### 8. 光源对物体表面的影响

学习如何使用光源在 Three.js 中影响物体的表面。

### 9. 相机控件轨道控制器 OrbitControls

引入 OrbitControls，让用户控制相机以便更好地观察场景。

### 10. 平行光与环境光

了解平行光和环境光在 Three.js 中的作用。

### 11. 动画渲染循环

使用 `requestAnimationFrame` 创建 Three.js 动画渲染循环。示例代码：

```javascript
function animate() {
  requestAnimationFrame(animate);
  // 在此处更新场景对象或相机
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
animate();
```

### 12. Canvas 画布布局和全屏

学习如何设置 Three.js Canvas 画布布局并实现全屏显示效果。示例代码：

```javascript
// 设置画布尺寸与窗口一致
renderer.setSize(window.innerWidth, window.innerHeight);

// 监听窗口大小变化事件
window.addEventListener('resize', function () {
  const width = window.innerWidth;
  const height = window.innerHeight;
  renderer.setSize(width, height);
  camera.aspect = width / height;
  camera.updateProjectionMatrix();
});
```

### 13. Stats 查看 Three.js 渲染帧率

使用 Stats.js 监控 Three.js 渲染性能。示例代码：

```javascript
const stats = new Stats();
document.body.appendChild(stats.dom);
function animate() {
  requestAnimationFrame(animate);
  stats.update();
  // 其他渲染代码...
}
animate();
```

### 14. 阵列立方体和相机适配体验

创建多个立方体并调整相机以适配整个场景。示例代码：

```javascript
// 生成多个立方体
for (let i = 0; i < 100; i++) {
  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshBasicMaterial({ color: Math.random() * 0xffffff });
  const cube = new THREE.Mesh(geometry, material);
  cube.position.set(Math.random() * 200 - 100, Math.random() * 200 - 100, Math.random() * 200 - 100);
  scene.add(cube);
}

// 调整相机以适配整个场景
const box = new THREE.Box3().setFromObject(scene);
const center = box.getCenter(new THREE.Vector3());
const size = box.getSize(new THREE.Vector3());
camera.position.copy(center);
camera.position.x += size.x * 1.2;
camera.position.y += size.y * 1.2;
camera.position.z += size.z * 1.2;
```

### 15. Three.js 常见简单几何体简介

了解和使用 Three.js 中常见的简单几何体，例如球体、立方体和圆柱体等。示例代码：

```javascript
const geometry = new THREE.SphereGeometry(50, 32, 32); // 创建一个球体
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 }); // 创建材质
const sphere = new THREE.Mesh(geometry, material); // 创建网格对象
scene.add(sphere); // 添加到场景中
```

### 16. 高光网格材质 MeshPhongMaterial

了解并使用 Three.js 中的高光网格材质 `MeshPhongMaterial`。示例代码：

```
javascriptCopy code
const material = new THREE.MeshPhongMaterial({ color: 0xff0000 }); // 创建高光网格材质
const mesh = new THREE.Mesh(geometry, material); // 创建网格对象
scene.add(mesh); // 添加到场景中
```

### 17. WebGL 渲染器设置(锯齿模糊、背景颜色)

了解并设置 WebGL 渲染器的属性，如锯齿模糊和背景颜色。示例代码：

```javascript
renderer.setPixelRatio(window.devicePixelRatio); // 设置像素比
renderer.setClearColor(0xeeeeee, 1); // 设置背景颜色
renderer.antialias = true; // 启用抗锯齿
```

### 18. gui.js 库(可视化改变三维场景)

使用 `dat.GUI` 库创建可视化控制面板，控制 Three.js 场景中的参数。示例代码：

```javascript
const gui = new dat.GUI(); // 创建 GUI
const params = { color: 0xff0000 }; // 定义参数
gui.addColor(params, 'color').onChange((value) => {
  // 当颜色参数变化时执行的操作
  material.color.set(value);
});
```
