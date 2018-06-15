# <div align="center">Vue - ie9 兼容</div>

## 目录

- [前言](#前言)
- [ES6兼容](#es6兼容)
- [Number对象](#number对象)
- [requestAnimationFrame方法](#requestanimationframe方法)
- [http网络请求(跨域)](#http网络请求跨域)

<br><br><br><br><br><br>

## 前言

**背景情况**
- vue - 2.5.11
- vue-cli 使用模板 `webpack-simple`

Vue 官方对于 ie 浏览器版本兼容情况的描述是 ie9+，即是 ie9 及更高的版本。经过测试，Vue 的核心框架 `vuejs` 本身，以及生态的官方核心插件（VueRouter、Vuex等）均可以在 ie9 上正常使用。

Vue 的作者尤雨溪对于 [Vue 的学习建议](https://github.com/TerryZ/js-develop-skill-summary/blob/master/vue-base.md#vue2x-%E5%AD%A6%E4%B9%A0%E9%A1%BA%E5%BA%8F%E5%BB%BA%E8%AE%AE) 中有提及为了将项目更好的生态化/工程化，要尽可能学习及使用新的 ECMAScript 规范。然而所有版本的 ie，即使是最后的版本 ie11，对于 es6 规范也支持得并不全，如此我们需要对所有原生不支持 ES6 特性的浏览器做兼容性处理。

<br><br>

## ES6兼容

在使用 Vue 做项目开发时，使用的 JS 规范就尽可使用新版本，而 ES6/ES2015 算是目前可以使用较新且较稳定的规范，文档比较全，国内还有 [阮一峰 《ECMAScript 6 入门》](http://es6.ruanyifeng.com/) 做了大量的文档翻译，开发环境可谓相当完善

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

加入这些代码后，工程里的大部分内容已可兼容到 ie9 版本



<br><br>

## Number对象

即使在使用 `babel-polyfill` 做代码翻译后，发现还是有一些 es6 的新特性并没有解决，比如说 `Number` 对象的 `parseInt` 和 `parseFloat` 方法

es6 将全局方法 `parseInt()` 和 `parseFloat()` ，移植到 `Number` 对象上面，行为完全保持不变。这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

解决这个问题不需要引入包来解决，同样在项目主入口文件 `main.js` 加入以下代码（代码尽可能靠前，最好是在引用 `babel-polyfill` 之后 ）

```js
if (Number.parseInt === undefined) Number.parseInt = window.parseInt;
if (Number.parseFloat === undefined) Number.parseFloat = window.parseFloat;
```

<br><br>

## requestAnimationFrame方法

`window.requestAnimationFrame` 是浏览器用于定时循环操作的一个接口，类似于 `setTimeout`，主要用途是按帧对网页进行重绘。

`requestAnimationFrame` 的优势，在于充分利用显示器的刷新机制，比较节省系统资源。显示器有固定的刷新频率（60Hz或75Hz），也就是说，每秒最多只能重绘60次或75次，`requestAnimationFrame` 的基本思想就是与这个刷新频率保持同步，利用这个刷新频率进行页面重绘。此外，使用这个API，一旦页面不处于浏览器的当前标签，就会自动停止刷新。这就节省了CPU、GPU和电力。

不过有一点需要注意，`requestAnimationFrame` 是在主线程上完成。这意味着，如果主线程非常繁忙，`requestAnimationFrame` 的动画效果会大打折扣。

`window.requestAnimationFrame()` 方法告诉浏览器您希望执行动画并请求浏览器在下一次重绘之前调用指定的函数来更新动画。该方法使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用。

有部分第三方组件就使用了这个方法，例如部分文件上传、图片处理类的组件；那么在这类型的组件在 ie9 下使用时，会报出

```
SCRIPT5007: Expected object.
```

`window.requestAnimationFrame()` 的最低兼容 ie 版本为 10，那么在 ie9 上做兼容就需要制作 requestAnimationFrame polyfill

```js
(function() {
    var lastTime = 0;
    var vendors = ['ms', 'moz', 'webkit', 'o'];
    for(var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
        window.requestAnimationFrame = window[vendors[x]+'RequestAnimationFrame'];
        window.cancelAnimationFrame = window[vendors[x]+'CancelAnimationFrame'] 
                                   || window[vendors[x]+'CancelRequestAnimationFrame'];
    }
 
    if (!window.requestAnimationFrame)
        window.requestAnimationFrame = function(callback, element) {
            var currTime = new Date().getTime();
            var timeToCall = Math.max(0, 16 - (currTime - lastTime));
            var id = window.setTimeout(function() { callback(currTime + timeToCall); }, 
              timeToCall);
            lastTime = currTime + timeToCall;
            return id;
        };
 
    if (!window.cancelAnimationFrame)
        window.cancelAnimationFrame = function(id) {
            clearTimeout(id);
        };
}());
```
Gist：[requestAnimationFrame polyfill](https://gist.github.com/paulirish/1579671)

<br><br>

## http网络请求(跨域)

<br><br>
