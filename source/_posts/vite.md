---
title: Vite
date: 2025-03-12 18:28:00
tags:
- 前端学习计划
- 前端
- vite
categories: 
- 笔记
---

vite 

<!-- more -->

理解 create-vite 和 vite的区别

分包

```js
build: {
    rollupOptions: {
      output: {
        manualChunks(id) {
          if (id.includes('node_modules')) {
            // 将所有 node_modules 中的模块打包到一个名为 vendor 的包中
            return 'vendor';
          }
        }
      }
    }
  }
```

vitecdnplugin

跨域其实就是自己骗了浏览器 中间加了一个服务器 

