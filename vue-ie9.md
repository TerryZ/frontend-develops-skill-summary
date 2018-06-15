# <div align="center">Vue - ie9 兼容</div>

**背景情况**
- vue2.5.11
- vue-cli 使用模板 `webpack-simple`

## 目录

- [ES6兼容](#es6兼容)
- [Number对象](#number对象)
- [requestAnimationFrame对象](#requestanimationframe对象)
- [http网络请求(跨域)](#http网络请求跨域)

<br><br><br><br><br><br>

## ES6兼容

在使用 Vue 做项目开发时，使用的 JS 规范就尽可使用新版本，而 ES6/ES2015 算是目前可以使用较新且较稳定的规范，文档比较全，国内还有 [阮一峰 《ECMAScript 6 入门》](http://es6.ruanyifeng.com/) 做了大师的文档翻译，开发环境可谓相当完善

但是，在 ie9 的环境上，es6 的部分新对象、表达式，并不支持，解决方案是使用 `babel-polyfill` 组件，它可以将 es6 的代码翻译成低版本浏览器可以识别的 es5 代码

```bash
npm i babel-polyfill --save-dev
```

安装完成后，在项目的主入口文件 `main.js` 的首行就可以直接引用

```js
import 'babel-polyfill';
```

在项目使用 `vue-cli` 生成的代码中，根目录有一个 `.babelrc` 文件，这是项目使用 babel 的配置文件。在默认生成的模板内容中，增加 `"useBuiltIns": "entry"` 的设置内容，这是一个指定哪些内容需要被 polyfill(兼容) 的设置

useBuiltIns 有三个设置选项

- `false` - 不做任何操作
- `entry` - 根据浏览器版本的支持，将 polyfill 需求拆分引入，仅引入有浏览器不支持的polyfill
- `usage` - 检测代码中 `ES6/7/8` 等的使用情况，仅仅加载代码中用到的 polyfill

这里推荐设置为 `entry` ，完整的 `.babelrc` 内容如下：
```js
{
  "presets": [
    [
      "env",
      {
        "modules": false,
        "useBuiltIns": "entry"
      }
    ],
    "stage-3"
  ]
}

```

加入这些代码后，工程里的所有内容都可以完全兼容到 ie9 的浏览器上



<br><br>

## Number对象

即使在使用 `babel-polyfill` 做代码翻译后，发现还是有一些 es6 的新特性并没有解决，比如说 `Number` 对象的 `parseInt` 和 `parseFloat` 方法

es6 将全局方法 `parseInt()` 和 `parseFloat()` ，移植到 `Number` 对象上面，行为完全保持不变。这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

解决这个问题不需要引入包来解决，同样在项目主入口文件 `main.js` 加入以下代码（代码尽可能靠前，最好是在引用 `babel-polyfill` 之后 ）

```js
if (Number.parseInt === undefined) Number.parseInt = window.parseInt;
if (Number.parseFloat === undefined) Number.parseFloat = window.parseFloat;
```

## requestAnimationFrame对象

## http网络请求(跨域)

<br><br>
