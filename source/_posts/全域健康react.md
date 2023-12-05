---
title: allzonehealth前端
date: 2023-04-27 19:50:12
tags:
- react
- 前端
- 字典
- 表单格式
- 正则
- 跨域
categories: 
- 笔记
---

### react开发简洁开发

``` react
const onFinish = async (values) => {
  console.log(values);
  //对象字典，以便更简洁地构造 userData
  const userDataBuilders = {
    mobile: (values) => new URLSearchParams({
      mobile: values.mobile,
      code: values.code,
      password: values.password,
      autologin: values.remember,
    }),
    email: (values) => new URLSearchParams({
      email: `${values.emailPrefix}${values.emailSuffix}`,
      code: values.code,
      password: values.password,
      autologin: values.remember,
    }),
  };
```

### 跨域问题

记录一次跨域问题，找了1个小时，原来与BaseUrl有关

<!-- more -->

这是放在src下的`setupProxy.js`文件,该路径下会自动识别

```js
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function (app) {
  app.use(
    '/api',
    createProxyMiddleware({
      target: 'http://localhost:8888',
      changeOrigin: true,
      pathRewrite: {
        '^/api': '', 
      },
    })
  );
};
```

那么axios的配置就应该是

```js
axios.defaults.baseURL = '/api/'
```

### 邮箱@之前的前缀（不包括@）

```react
 const validateEmail = async (_, value) => {
   const emailPrefixRegex = /^(?![_.])[a-zA-Z0-9._]+(?<![_.])$/;
   if (value && emailPrefixRegex.test(value)) {
      return Promise.resolve(); // 验证通过
   } else {
      return Promise.reject('请输入正确的邮箱前缀'); // 验证失败，返回错误信息
   }
 };
```

