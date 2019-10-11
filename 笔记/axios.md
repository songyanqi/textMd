使用 axios  的优点：
axios 是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端，它本身具有以下特征：

从浏览器中创建 XMLHttpRequest
从 node.js 发出 http 请求
支持 Promise API
拦截请求和响应
转换请求和响应数据
取消请求
自动转换JSON数据
客户端支持防止CSRF/XSRF


兼容性处理：

项目中发现，在安卓4.3及以下的手机不支持axios的使用，主要就是无法使用promise。加上以下polyfill就可以了。

项目中安装es6-promise
cnpm install es6-promise --save-dev

在axios.min.js开头加上
require('es6-promise').polyfill();