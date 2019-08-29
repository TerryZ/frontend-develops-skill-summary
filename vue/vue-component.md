# <div align="center">Vue - Component 组件开发</div>

## 目录

- [循环组件](#循环组件)
- [获得元素的尺寸](#获得元素的尺寸)
- [mixins 混入](#mixins-混入)
- [render 渲染函数](#render-渲染函数)
- [单选与复选](#单选与复选)

<br><br><br><br><br><br>

## 循环组件

```vue
<user v-for="(user, index) in users" :key="`user-${user.id}`" >
```
在使用自定义组件，并需要生成多个时，有个很重要的属性需要关注：`key`

使用中可以把它理解成组件的唯一 id，建议在绑定时，绑定一些不会重复的自定义值，注意不要使用 `v-for` 中的 `index` 属性进行绑定，这个 `index` 就是数组的下标，在频繁增加、移除时，数组下标会出现重复。

期望的结果是移除最后一个组件，并新增一个新的组件；此时，被移除的组件下标是0，新增的组件下标也是0，Vue 会认为现有的组件内容需要更新，而不是去新增一个新的组件，这样常常会出现莫明其妙的问题

<br><br>

## 获得元素的尺寸

以获得元素的高度（height）为例

```vue
<div class="someclass" ref="box"></div>
```

获得尺寸的几种方式

```js
let box = this.$refs.box, height = 0;

//100(number)
height = box.offsetHeight;
//100(number)
height = box.getBoundingClientRect().height;
//'100px'(string) - 获得样式值
height = window.getComputedStyle(box).height;
//'100px'(string) - 使用此法，往往获得不到内容
height = box.style.height;
```

**需要注意的是，Vue 的 ref 属性提供的对象并非响应式，为保证数据的准确有效性，请在需要获得尺寸时，再获得 ref 属性的对象进行获取数据**

<br><br>

## mixins 混入

元素抽象，可将 project, component 或 plugin 内部的公共元素进行抽象，使用通过 mixins 进入混入即可

```js
import mixins from './mixins';


export default{
    mixins: [mixins],
    ...
};
```

**注意**：在执行混入操作的元素合并时，若目标元素与本地元素名称重合，则以本地元素优先使用

<br><br>

## $attrs 和 $listeners

在开发多层级组件时（父 => 子 => ...），对于组件指定的 Props 数据以及 v-on 的事件响应处理默认情况下，仅在组件最外层响应，子、孙组件里，无法获得传入的数据及事件响应。

<br><br>

## 依赖注入(provide/inject)


```js
//parent component
export default {
    data: {
        return {
            name: 'michael'
        };
    },
    provide: {
        return {
            parentName: this.name
        }
    }
};

//children component
export default {
    data: {
        name: 'sam'
    },
    inject: ['parentName'],
    mounted() {
        let fullname = `${this.name} - ${this.parentName}`;
    }
}
```

需要注意的是 provide/inject 并非响应式，这是 Vue 在设计上有意为之的结果

<br><br>

## render 渲染函数

### v-show

### v-model

假设在 data 中已定义名为 user 的字符串变量

- 表单元素

模板

```vue
<input type="text" v-model="user"></input>
```

render

```js
h('input', {
    attrs:{
        type: 'text'
    },
    domProps:{
        value: this.user
    },
    on: {
        input: val => this.user = val
    }
})
```

- 自定义组件

模板

```vue
<custom-component v-model="user"></custom-component>
```

render

```js
h('custom-component', {
    attrs:{
        value: this.user
    },
    on: {
        input: val => this.user = val
    }
})
```

### .sync

```js
// same as <custom-component :user.sync="user"></custom-component>
h('custom-component', {
    attrs:{
        user: this.user
    },
    on: {
        'update:user': val => this.user = val
    }
})
```

### slot


<br><br>

## 单选与复选

通常在表单中设置单选或复选框如下（仅举例单选框）

```vue
<template>
  <div>
    <input type="radio" name="options" value="A" v-model="picked"> A
    <input type="radio" name="options" value="B" v-model="picked"> B
    <input type="radio" name="options" value="C" v-model="picked"> C
  </div>
</template>
<script>
export default {
  data(){
    return {
      picked: ''
    }
  }
}
</script>
```

但有时我们需要根据不同使用需求来决定选择类型，于是代码修改如下

```vue
<template>
  <div>
    <input :type="inputType" name="options" value="A" v-model="picked"> A
    <input :type="inputType" name="options" value="B" v-model="picked"> B
    <input :type="inputType" name="options" value="C" v-model="picked"> C
  </div>
</template>
<script>
export default {
  data(){
    return {
      type: 1
      picked: ''
    }
  },
  computed: {
    inputType(){
      return this.type ? 'checkbox' : 'radio'
    }
  }
}
</script>
```

可以看到 input 类型会根据变量值的变化而改变，这种处理模式本也常见，在开发环境一切相安无事，将功能通过产品模式发布到生产环境后，发现该功能在 ie 浏览器下功能不正常，并有相应的错误信息

> duplicate data property in object literal not allowed in strict mode

根据调试跟踪，最终发现是因为在 `strict` 严格模式下，对象的属性不允许被重复定义，出现错误的代码为 template 被编译为 render 函数后的内容，具体代码如下

```js
h('input', {
  ...
  domProps: {
    "value": _vm.otherValue,
    "value": (_vm.picked)
  }
  ...
})

```

此处为 render 渲染函数中提供的 `createElement` 对 radio 的构建代码，其中 domProps 出现了两个 value 属性，导致了 ie 浏览器的报错。在现代浏览器中，会自动兼容这种重复属性的问题，不会出现错误提示，而在 ie 浏览器或是较低版本的浏览器中会出现该错误提示，而这样的问题会带来的后果就是在 ie 浏览器或是较旧手机的 webview 中显示该页面会出现白屏的情况，较难被排查出来

该问题在 vue 项目中也有相关 issues：[domProps has twice the "value" key #6917](https://github.com/vuejs/vue/issues/6917)，根据 Evan You 的描述，问题的原因在于 input 的 type 属性是动态生成的，若是静态指定的模式不会出现该问题，比如

```vue
<template>
  <div>
    <input type="checkbox" name="options" value="A" v-model="picked" v-if="type"> A
    <input type="radio" name="options" value="A" v-model="picked" v-else> A
  </div>
</template>
```

这样或能解决问题，但代码写起来太过冗余，我个人的处理方案是将之独立提取为一个 component，大致代码如下

```js
export default {
  name: 'Input-El',
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    ...
    type: Number,
    checked: [Array, String]
  },
  render (h) {
    return h('input', {
      class: 'custom-control-input',
      attrs: {
        ...
        type: this.type ? 'checkbox' : 'radio', // 根据类型设置 type 属性
        value: this.optionId,// 静态生成 value 属性，不再跟 v-model 争夺 value 属性
        name: 'vote-options'
      },
      on: {
        // 在 change 事件中触发 change 事件回调
        change: e => {
          this.$emit('change', this.optionId)
        }
      }
    })
  }
}
```

> 关于 model 参数的配置详情请参考官网： [model option in component](https://cn.vuejs.org/v2/api/#model)

如此改造后，界面上即可正常使用参数传递，而不用书写冗长的代码，以下是最终的使用情况

```vue
<template>
  <div>
    <!-- before -->
    <input type="checkbox" name="options" value="A" v-model="picked" v-if="type"> A
    <input type="radio" name="options" value="A" v-model="picked" v-else> A
    <!-- after -->
    <input-el :type="type" v-model="picked" :option-id="option.id"> A
  </div>
</template>
```
