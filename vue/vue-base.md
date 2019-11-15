# <div align="center">Vue 核心功能</div>

## 目录

- [Vue2.x 学习顺序建议](#vue2x-学习顺序建议)
- [Vue2.x 生命周期](#vue2x-生命周期)
- [数据对象监听](#数据对象监听)
- [Component 数据输入和输出](#component-数据输入和输出)
- [数据与DOM更新完成后的回调](#数据与dom更新完成后的回调)
- [img 图片标签动态地址](#img-图片标签动态地址)
- [动画执行顺序](#动画执行顺序)
- [Array 操作注意事项](#Array-操作注意事项)
- [Vue.set 向响应式对象添加新属性并触发视图更新](#vueset-向响应式对象添加新属性并触发视图更新)


<br><br><br><br><br><br>

## Vue2.x 学习顺序建议

vuejs 作者尤雨溪对于 `Vue2` 的学习顺序建议

> 注：2.0 已经有 [中文文档](https://cn.vuejs.org/) 。如果对自己英文有信心，也可以直接阅读 [英文文档](https://vuejs.org/) ，根据自身实际情况灵活调整。

**起步**

1. 扎实的 JavaScript / HTML / CSS 基本功。这是前置条件。

2. 通读官方教程 (guide) 的基础篇。不要用任何构建工具，就只用最简单的 <script>，把教程里的例子模仿一遍，理解用法。**不推荐上来就直接用 vue-cli 构建项目，尤其是如果没有 Node/Webpack 基础。**

3. 照着官网上的示例，自己想一些类似的例子，模仿着实现来练手，加深理解。

4. 阅读官方教程进阶篇的前半部分，到『自定义指令 (Custom Directive) 』为止。着重理解 Vue 的响应式机制和组件生命周期。『渲染函数（Render Function)』如果理解吃力可以先跳过。

5. 阅读教程里关于路由和状态管理的章节，然后根据需要学习 vue-router 和 vuex。同样的，先不要管构建工具，以跟着文档里的例子理解用法为主。

6. 走完基础文档后，如果你对于基于 Node 的前端工程化不熟悉，就需要补课了。下面这些严格来说并不是 Vue 本身的内容，也不涵盖所有的前端工程化知识，但对于大型的 Vue 工程是前置条件，也是合格的『前端工程师』应当具备的知识。

**前端生态/工程化**

1. 了解 JavaScript 背后的规范，ECMAScript 的历史和目前的规范制定方式。学习 ES2015/16 的新特性，理解 ES2015 modules，适当关注 [还未成为标准的提案](https://github.com/tc39/proposals)  。

2. 学习命令行的使用。建议用 Mac。

3. 学习 Node.js 基础。**建议使用 [nvm](https://github.com/creationix/nvm) 这样的工具来管理机器上的 Node 版本，并且将 npm 的 registry 注册表配置为 [淘宝的镜像源](https://npm.taobao.org/) 。** 至少要了解 npm 的常用命令，npm scripts 如何使用，语义化版本号规则，CommonJS 模块规范（了解它和 ES2015 Modules 的异同），Node 包的解析规则，以及 Node 的常用 API。应当做到可以自己写一些基本的命令行程序。注意最新版本的 Node (6+) 已经支持绝大部分 ES2015 的特性，可以借此巩固 ES2015。

4. 了解如何使用 / 配置 Babel 来将 ES2015 编译到 ES5 用于浏览器环境。

5. 学习 Webpack。Webpack 是一个极其强大同时也复杂的工具，作为起步，理解它的『一切皆模块』的思想，并基本了解其常用配置选项和 loader 的概念/使用方法即可，比如如何搭配 Webpack 使用 Babel。学习 Webpack 的一个挑战在于其本身文档的混乱，建议多搜索搜索，应该还是有质量不错的第三方教程的。英文好的建议阅读 [Webpack 2.0 的文档](https://webpack.js.org/guides/getting-started/) ，比起 1.0 有极大的改善，但需要注意和 1.0 的不兼容之处。

**Vue 进阶**

1. 有了 Node 和 Webpack 的基础，可以通过 vue-cli 来搭建基于 Webpack ，并且支持单文件组件的项目了。建议用 webpack-simple 这个模板开始，并阅读官方教程进阶篇剩余的内容以及 [vue-loader 的文档](https://vue-loader.vuejs.org/) ，了解一些进阶配置。有兴趣的可以自己亲手从零开始搭一个项目加深理解。

2. 根据 [例子](https://github.com/vuejs/vue-hackernews-2.0) 尝试在 Webpack 模板基础上整合 vue-router 和 vuex

3. 深入理解 Virtual DOM 和『渲染函数 (Render Functions)』这一章节（可选择性使用 JSX)，理解模板和渲染函数之间的对应关系，了解其使用方法和适用场景。

4. （可选）根据需求，了解服务端渲染的使用（需要配合 Node 服务器开发的知识）。其实更重要的是理解它所解决的问题并搞清楚你是否需要它。

5. 阅读开源的 Vue 应用、组件、插件源码，自己尝试编写开源的 Vue 组件、插件。

6. 参考 [贡献指南](https://github.com/vuejs/vue/blob/dev/.github/CONTRIBUTING.md#development-setup) 阅读 Vue 的源码，理解内部实现细节。（需要了解 Flow）

7. 参与 Vue GitHub issue 的定位 -> 贡献 PR -> 加入核心团队 -> 升任 CTO -> 迎娶白富美...（误

## Vue2.x 生命周期

`Vue2.x` 的生命周期图例

![Vue2.x生命周期](https://cn.vuejs.org/images/lifecycle.png)

<br><br>

## 数据对象监听

在开发组件（component）时，通常需要接收到调用者传入的参数数据，且需要对数据的修改作出及时的响应；典型应用场景就是自定义的表格组件（TableGrid）需要实时监听表格查询参数的变化，当参数的内容发生变化时，使用新的参数进行服务端数据查询，以达到查询数据表格数据的效果

```js
<script>
export default {
  props: ['setting']//总的入参对象
  data(){
    return {
      query: {},
      pageNumber: 1
    }
  },
  watch: {
    //基础数据类型的监听
    pageNumber(newVal, oldVal){
      //do something...
    },
    'setting.params':{
      //监听到新的参数变化时，触发表格数据查询
      handler(newVal, oldVal){
        this.query = newVal
        this.doRequest()
      },
      deep: true//深度监听，监听对象类型最重要就是要打开它
    }
  }
  methods: {
    doRequest(){
      ...
    }
  }
}
</script>
```

根据 vuejs 官网的文档说明，监听数组不需要设置 deep，使用方式与普通数据的方式一致

<br><br>

## Component 数据输入和输出

使用 component 做功能模块开发时，一定会需要涉及到数据的输入和输出

- `props` 接收外部传入的参数
- `$emit` 向外层触发事件并传递数据

```js
export default {
  props: ['setting'],
  data(){
    return{
      config: this.setting //将 props 中接收到的 setting 参数落地到本地的变量
    }
  }
  methods: {
    callback(){
      this.$emit('data-change', 1) //触发外层监听的 data-change 事件，并传递数据 1
    }
  }
};
```

外层标签：
```html
<!-- 使用 v-bind 绑定数据到 setting 属性上，并监听 data-change 事件 -->
<xxx :setting="{a:1,b:2}" @data-change="doSomething">
```

<br><br>

## 数据与DOM更新完成后的回调

数据更新回调
```js
export default {
    data(){
        list: []
    },
    mounted(){
        this.list = [1,2,3];
        this.nextTick(()=>{
            console.log('数据更新完成');
        });
    }
};
```

Dom 更新回调


```vue
<template>
    <ul>
        <li v-for="item in list">{{item}}</li>
    </ul>
</template>
<script>
export default {
    data(){
        list: []
    },
    mounted(){
        this.list = [1,2,3];
        this.$nextTick(()=>{
            console.log('dom 更新完成');
        });
    }
};
</script>
```

<br><br>

## img 图片标签动态地址

首先，在使用 `<img>` 图片标签时，若图片的地址是绝对地址，不会有任何问题；但通常在项目中，我们需要在加载页面时，使用动态内容构建图片地址

```vue
<img :src="'../../assets/' + img.name + '.png'" v-for="img in list" >
```

上面的代码运行后，页面的图片全是读取不到路径的图片；实际上，在 `src` 目录下的图片资源，`webpack` 会编译打包在 `dist` 目录中，使用这种方式， `webpack` 无法识别并将图片地址转换为 '/dist/xxx.png'

为了解决路径问题，我们需要使用到 `require` 函数，那么定义一个 `method`

```js
methods:{
    getImg(name){
        return require('../../assets/' + name + '.png');
    }
}
```

并将 HTML 内容修改为

```vue
<img :src="getImg(img.name)" v-for="img in list" >
```

如此，则路径正常转换为 `/dist/xxx.png`

<br><br>

## 动画执行顺序

使用 `<transition>` 标签对希望出现动画效果的元素进行包裹，在元素的 `v-if` 或 `v-show` 的真假值切换时，即会出现指定的动画效果

- 进入

enter -> enter-active -> enter-to

- 离开

leave -> leave-active -> leave-to

<br><br>

## Array 操作注意事项

在 Vue 中，我们通常使用 Array 对象来管理一个数据列表，在使用中有部分情况需要注意：

**清空数组**

清空数组数据，以达到清空列表的效果，在原生 js 中，清空数组的方式通常有 5 种，其中以 `arr.length = 0;` 方式性能最佳；但在 Vue 中使用该方式清空数组，会发现数组内容已被清空，但绑定的 dom 内容却没有发生变化，这个问题 Evan You 专门做了解答：[What is the best way to empty an array?](https://github.com/vuejs/Discussion/issues/59)，那么结论就是在 Vue 中清空数组的最佳方式为 `arr = [];`


**扩展阅读**

- [清空数组的方式及性能评测](https://github.com/TerryZ/frontend-develops-skill-summary/blob/master/javascript-array.md#%E6%B8%85%E7%A9%BA%E6%95%B0%E7%BB%84)

<br><br>

## Vue.set 向响应式对象添加新属性并触发视图更新

一个普通的表单场景，使用 model 对象作为表单数据集的基础数据模型，但定义时只初始化了一个空对象

```vue
<template>
  name: <input type="text" v-model="model.name">
  age: <input type="text" v-model="model.age">
  address: <input type="text" v-model="model.address">

  <button type="button" @click="build">build user name</button>
  <button type="button" @click="save">save</button>
</template>

<script>
export default {
  data(){
    return {
      model: {}
    }
  },
  methods: {
    build(){
      this.model.name = 'Tom'
    },
    save(){
      //do some save stuff
    }
  }
}
</script>
```

`build user name` 按钮的事情执行了修改属性的操作，但点击按钮后与 model.name 绑定的 input 内容并未改变；通过 Vue Devtools 观察发现 model.name 被正确设置为 'Tom'，但输入框的内容并未显示

由于 model 对象初始化时仅为空对象，没有 name 属性，于是虽然给 `model.name` 指定值时，并没有更新已绑定的视图内容

```js
build(){
  this.$set(this.model, 'name', 'Tom')
}
```

在 build 方法中修改为使用 Vue.set 方式设置 name 属性，则数据得到更新，同时绑定的 Dom 内容也被更新显示

`this.$set()` API 是全局 Vue.set 的别名
