# <div align="center">Vue - Component 组件开发</div>

## 目录

- [循环组件](#循环组件)

<br><br><br><br><br><br>

## 循环组件

```html
<user v-for="user, index in users"      
      :key="'user-' + user.id"
      >
```
在使用自定义组件，并需要生成多个时，有几个很重要的属性需要关注

`key` 可以把它理解成组件的唯一 id，建议在绑定时，绑定一些不会重复的自定义值，注意不要使用 `v-for` 中的 `index` 属性进行绑定，这个 `index` 就是一个数组的下标，在频繁增加、移除时，数组下标会出现重复，出现重复时，Vue 会认为现有的组件内容需要更新，而不是去新增一个新的组件，这样常常会出现莫明其妙的问题
