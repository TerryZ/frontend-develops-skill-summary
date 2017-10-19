# jQuery - Size and position 尺寸和定位

## 目录

- [浏览器区域的宽度高度](#浏览器区域的宽度高度)
- [判断当前页面是否存在滚动条](#判断当前页面是否存在滚动条)




<br><br><br><br><br><br><br>


### 浏览器区域的宽度高度

页面完整高度、宽度，即文档高度和宽度
```js
$(document).width();
$(document).height();
```

当前浏览器可视区域宽度、高度
```js
$(window).width();
$(window).height();
```

屏幕当前位置距离文档顶端的距离
```js
$(window).scrollTop();
```


### 判断当前页面是否存在滚动条
```js
if($(document).height() > $(window).height()){
  //do something
}
```
