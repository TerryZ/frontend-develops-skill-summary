# <div align="center">Vue - Http 网络请求</div>

## 目录

- [axios](#axios)
- [proxyTable/proxy](#proxytableproxy)
- [nginx反向代理](#nginx反向代理)

<br><br><br><br><br><br>

## axios

在 `vue1.x` 时，网络类的请求官方建议使用 `vue-resourse` 来处理，到 `vue2.x` 时，官方推荐使用的 `http` 请求的插件就变成了 [axios](https://github.com/axios/axios)

```html
<user v-for="user, index in users"      
      :key="'user-' + user.id"
      >
```
在使用自定义组件，并需要生成多个时，有个很重要的属性需要关注：`key`

使用中可以把它理解成组件的唯一 id，建议在绑定时，绑定一些不会重复的自定义值，注意不要使用 `v-for` 中的 `index` 属性进行绑定，这个 `index` 就是数组的下标，在频繁增加、移除时，数组下标会出现重复。

期望的结果是移除最后一个组件，并新增一个新的组件；此时，被移除的组件下标是0，新增的组件下标也是0，Vue 会认为现有的组件内容需要更新，而不是去新增一个新的组件，这样常常会出现莫明其妙的问题

<br><br>

## 跨域请求

[跨域数据请求的整体解决方案](https://github.com/TerryZ/js-develop-skill-summary/blob/master/vue-ie9.md#http%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E8%B7%A8%E5%9F%9F)
