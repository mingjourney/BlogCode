---
title: nodejs
date: 2025-3-31 10:47:33  
tags:
- nodejs
categories: 
- 笔记
---

# nodejs

nodejs 和 其他服务区别比如和java 

1. nodejs是单线程、异步、非阻塞
2. nodejs运行在服务端的js
3. 统一API

nodejs原作者已经叛变去做一个语言叫deno

## 异步

异步代码没法通过return 返回值

第一步只能选择回调函数

但是多步异步就会产生“回调地狱”问题

此时需要一个东西去代替回调函数返回异步结果 引出Promise出世

Promise resolve reject做的是存数据

then做的是取数据 并且是异步的

promise三个方法 then catch finally

then return的 会被重新resolve 并且会在 下一个链式调用的then中取到

微任务宏任务

涉及 调用栈 任务队列

任务队列还有两种 ： 宏任务队列和微任务队列

大部分代码都去宏任务队列中去排队

微任务队列 如promise 的 then catch finally、queneMicroTask

```js
queneMicrotask(() => {})
```

es5 能实现 es6大部分方法 es6就是语法糖

手写promise

```js
const STATE = {
  PENDING: 0,
  FULFILLED: 1,
  REJECTED: 2,
}
// #result = null;
class MyPromise {
  #result;
  #state = STATE.PENDING;
  #callbacks = [];
  constructor (executor) { 
    executor(this.#resolve.bind(this), this.#reject.bind(this));
  }
  #resolve (value) {
    this.#result = value;
    if (this.#state !== STATE.PENDING) return;
    this.#state = STATE.FULFILLED;
    queueMicrotask(() => {
      this.#callbacks.forEach(callback => callback(this.#result));
    })
  }
  #reject (value) {
    this.#result = value;
    if (this.#state !== STATE.PENDING) return;
    this.#state = STATE.REJECTED;
  }
  then(onFulFilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      if (this.#state === STATE.PENDING) {
        this.#callbacks.push((value) => resolve(onFulFilled(value)));
      } else if (this.#state === STATE.FULFILLED) {
        queueMicrotask(() => {
          resolve(onFulFilled(this.#result));
        })
      } 
    })
  }
}

//test
const promise = new MyPromise((resolve, reject) => {
  // setTimeout(() => {
    resolve(333);
  // }, 0);
})

promise.then((result) => {
  // setTimeout(() => {
    console.log('aaa', result);
    return '111';
  // })
}).then(res => {
  console.log('bbb', res);
  return '222'
}).then(res => {
  console.log('ccc', res);
});
```



Await 可以在async申明函数中 或者在 es模块顶部用（浏览器或mjs ）

## CommonJs

commonjs就是es6出来之前nodejs提出来的模块化

exports === module.exports

但是不能写exports = {} 只能是module.exports = {}或者exports. 这和js对象是引用数据类型的原理有关

requeire函数返回的就是exports

文件名后缀以及文件夹都会自动补齐后缀或者index.js

执行原理 module export require 这三个不是全局变量

所有commonjs模块都会被包装到一个函数中,函数如下，可以打印arguments得到这些实参

```js
;(function(exports, require, module, __filename, __dirname) {
  
})();
```

默认情况下 node中模块化标准是CommonJs 要使用es模块化：

1. 使用mjs作为扩展名
2. 修改package.json type="module"

es模块导入不能省略拓展名

node中 globalThis === global === window

process.nextTick(() => {}) // tick队列

调用栈 tick队列 微任务队列 宏任务队列

path.resolve() 返回的是工作目录

path.resolve(__dirname, './1.js')

确保健壮性，各环境各工作目录运行都能保证一样的结果

### fs

```js
const path = require('node:path');
// const fs = require('node:fs');
const fs = require('node:fs/promises');
console.log(__dirname);
console.log(path.resolve(__dirname, './1.js'));
// const buffer = fs.readFileSync(path.resolve(__dirname, './1.js'));
// console.log('buffer', buffer.toString());
// fs.readFile(path.resolve(__dirname, './1.js'), (err, buffer) => {console.log('buffer1', buffer.toString())});
fs.readFile(path.resolve(__dirname, './1.js'))
  .then((buffer) => {
    console.log('buffer2', buffer);
  })
  .catch(err => console.log('err', err));
(async () => {
  try {
    const buffer = await fs.readFile(path.resolve(__dirname, './1.js'));
    console.log('buffer3', buffer.toString());
  } catch (err) {
    console.log('err', err);
  }
})();
// fs.readFile() 读取文件
(async () => {
  const buffer = await fs.readFile(path.resolve(__dirname, './1.js'))
  return fs.appendFile(path.resolve(__dirname, '3.js'), buffer);
})();
fs.readFile(path.resolve(__dirname, './1.js')).then((buffer) => {
  return fs.appendFile(path.resolve(__dirname, '2.js'), buffer);
});
// fs.mkdir() 创建目录
// fs.rmdir() 删除目录
// fs.rmdir() 删除目录

```

## 网络协议

请求首行

请求头

Accept能够接受的内容类型

Accept-Encoding能接受的编码方式

Accept-Language能接受的自然语言

User-Agent身份标识

请求体

空行



在 ESM 中，可通过 `importmap` 使得裸导入可正常工作:

```
<script type="importmap">  {    "imports": {      "lodash": "https://cdn.skypack.dev/lodash",      "ms": "https://cdn.skypack.dev/ms"    }  }</script>
```

对于 `~1.2.3` 而言，它的版本号范围是 `>=1.2.3 <1.3.0`

对于 `^1.2.3` 而言，它的版本号范围是 `>=1.2.3 <2.0.0`
