# <div align="center">jQuery - event 事件处理</div>



## 目录

- [一些建议](#一些建议)
- [动态元素的事件委派](#动态元素的事件委派)
- [鼠标移入移出](#鼠标移入移出)
- [使用命名空间绑定事件](#使用命名空间绑定事件)

<br><br><br><br><br><br><br>

## 一些建议

- 用 `.bind()` 的代价是非常大的，它会把相同的一个事件处理程序 hook 到所有匹配的 DOM 元素上
- 不要再用 `.live()` 了，它已经不再被推荐了，而且还有许多问题
- `.delegate()` 会提供很好的方法来提高效率，同时我们可以添加一事件处理方法到动态添加的元素上。
- 以上的3种情况，都可以使用 `.on()` 来很好的进行替换

## 动态元素的事件委派

在制作较复杂的页面或开发插件时，通常需要对一些动态增、删的元素进行事件绑定；这些动态的元素在初始化时可能存在，也可能不存在，所以我们需要对动态元素的父框架进行事件委派

事件委派有三种方式：

- ~~`live / die`~~
- `delegate / undelegate`
- `on / off`

首先 `live / die` 的方式，在 `jQuery 1.7` 版本时弃用，在 `jQuery 1.9` 时移除，并不再推荐使用。

> 顺带说一个话题，注意到很多人在常规的事件绑定中，会习惯于使用 `live` 来绑定普通事件，这是一种极其浪费性能，杀鸡用牛刀的用法；委派类的事件绑定，是将事件绑定在 `document` 本身，而 `document` 作为整个网页最根级对象，页面上的所有元素的操作都会冒泡到 `document` 身上，由此可见，若使用委派的方式做常规的事情绑定，对浏览器的性能是极大的浪费

在弃用并移除 `live / die` 后，官方文档中建议使用 `delegate / undelegate` 和 `on / off` 来替代，那么把两者进行对比时发现，实际上，`delegate / undelegate` 只是代理了 `on / off`，那么我们的结论就是需要进行事件委派时，建议使用 `on / off`来操作

html
```html
<ul id='myUl'>
</ul>
```

jquery
```js
$('#myUl').on('click', 'li', function () {
  //do something
})
```

在 `html` 的代码中可以看到 `ul` 并没有任何 `li` 的子节点，仅是一个空的结构，在后续操作为其中动态生成了 `li` 元素后，所有 `ul` 里的 `li` 元素都会响应点击事件

## 鼠标移入移出

```js
$(div).mouseenter(function () {
  // mouse move in
}).mouseleave(function () {
  // mouse move out
})
```
`mouseenter` 和 `mouseleave` 两个配套的事件，解决了鼠标移入目标元素和移出目标元素的事件，而实际上我们可以使用 `hover` 来更简单的替换这一套事件

```js
$(div).hover(function () {
  // mouse move in
},function () {
  // mouse move out
})
```

`mouseover` 对比 `mouseenter` 、 `mouseout` 对比 `mouseleave` 的本质区别就是 `mouseover` 、 `mouseout` 会被子元素的 `mouseover` 、 `mouseout` 冒泡而触发，而 `mouseenter` 和 `mouseleave` 不被子元素事件冒泡而触发

<br><br>

## 使用命名空间绑定事件

在早期的jQuery版本中，命名空间是使用 `namespace.event` 的格式，而现在应该使用 `event.namesapce` 的格式

**使用命名空间绑定事件**
```js
$(button).on('click.eButton');
$(button).on('mouseenter.eButton');
$(button).on('blur.eButton');
```

**移除所有该命名空间的相关事件**

```js
$(button).off('.eButton');
```

使用命名空间进行移除事件，所有在绑定时，使用了该命名空间的事情都会被移除

如果有以下情况的事件绑定
```js
$(button).on('click');
$(button).on('click.a');
$(button).on('click.a.eButton');
```
只想触发没使用命名空间的事情，则
```js
//请注意这里的感叹号
$(button).trigger('click!');
//若不使用感叹号，那么所有click的事件都会被触发，包含所有带和不带命名空间的事件
//触发指定命名空间的
$(button).trigger('click.a.eButton');
```
