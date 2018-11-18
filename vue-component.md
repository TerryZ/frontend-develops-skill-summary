# <div align="center">Vue - Component 组件开发</div>

## 目录

- [循环组件](#循环组件)
- [获得元素的尺寸](#获得元素的尺寸)

<br><br><br><br><br><br>

## 循环组件

```html
<user v-for="user, index in users"      
      :key="'user-' + user.id"
      >
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

## mixins

## $attrs 和 $listeners

## 依赖注入(provide/inject)

需要注意的是 provide/inject 并非响应式，这是有意为之
