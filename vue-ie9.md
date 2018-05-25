# <div align="center">Vue - ie9 兼容</div>

**背景情况**
- vue2.5.11
- vue-cli 使用模板 `webpack-simple`

## 目录

- [es6兼容](#es6兼容)
- [Number对象](#Number对象)
- [http网络请求(跨域)](#http网络请求(跨域))

<br><br><br><br><br><br>

## es6兼容

在使用 Vue 做项目开发时，使用的 JS 规范就尽可能往较新的版本上靠，而 ES6/ES2015 算是目前可以使用较新且较稳定的规范，文档比较全，国内还有 [阮一峰 - ECMAScript 6 入门](http://es6.ruanyifeng.com/) 做了完全的翻译文档，开发环境可谓相当完善

但是，在 ie9 的环境上，es6 的部分新对象、表达式，并不支持，解决方案是使用 `babel-polyfill` 组件，它可以将 es6 的代码翻译成 低版本浏览器可以识别的 es5 代码

```bash
npm i babel-polyfill --save-dev
```

安装完成后，在项目的主入口文件 `main.js` 的首行就可以直接引用

```js
import 'babel-polyfill';
```

加入该代码后，工程里的所有内容都可以完全兼容到 ie9 的浏览器上



<br><br>

## Number对象

## http网络请求(跨域)

<br><br>
