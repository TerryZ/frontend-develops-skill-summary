# <div align="center">Vue - Component 组件开发</div>

## 目录

- [循环组件](#循环组件)
- [获得元素的尺寸](#获得元素的尺寸)
- [mixins 混入](#mixins-混入)
- [render 渲染函数](#render-渲染函数)

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

**v-show**

假设在 data 中已定义 user 的字符串变量

- 表单元素

```js
h('input', {
    attrs:{
        type: 'input'
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

**v-model**

**.sync**

**slot**


<br><br>
