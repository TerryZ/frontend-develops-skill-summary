<center>jQuery - event 事件处理</center>
========================================

## 目录

- [一些建议](#一些建议)
- [鼠标移入移出](#鼠标移入移出)
- [使用命名空间绑定事件](#使用命名空间绑定事件)

<br><br><br><br><br><br><br>

## 一些建议

- 用.bind()的代价是非常大的，它会把相同的一个事件处理程序hook到所有匹配的DOM元素上
- 不要再用.live()了，它已经不再被推荐了，而且还有许多问题
- .delegate()会提供很好的方法来提高效率，同时我们可以添加一事件处理方法到动态添加的元素上。
- 以上的3种情况，都可以使用.on来很好的进行替换

<br><br>

## 鼠标移入移出

```js
$(div).mouseenter(function(){
  //mouse move in
}).mouseleave(function(){
  //mouse move out
});
```
`mouseenter` 和 `mouseleave` 两个配套的事件，解决了鼠标移入目标元素和移出目标元素的事件，而实际上我们可以使用 `hover` 来更简单的替换这一套事件

```js
$(div).hover(function(){
  //mouse move in
},function(){
  //mouse move out
});
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
