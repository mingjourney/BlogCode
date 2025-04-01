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





