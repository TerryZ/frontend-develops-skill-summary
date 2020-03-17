# <div align="center">jQuery - Size and position 尺寸和定位</div>

## 目录

- [浏览器区域的宽度高度](#浏览器区域的宽度高度)
- [对于宽、高度的获取，及精准数值的获取方式（含小数点）](#user-content-对于宽高度的获取及精准数值的获取方式含小数点)
- [判断当前页面是否存在滚动条](#判断当前页面是否存在滚动条)




<br><br><br><br><br><br><br>


## 浏览器区域的宽度高度

页面完整高度、宽度，即文档高度和宽度
```js
$(document).width()
$(document).height()
```

当前浏览器可视区域宽度、高度
```js
$(window).width()
$(window).height()
```

屏幕当前位置距离文档顶端的距离
```js
$(window).scrollTop()
```

<br><br>

## 对于宽、高度的获取，及精准数值的获取方式（含小数点）

jQuery对于宽度、高度的结果是同一套规则，这里仅以宽度为主进行讲解 

对于宽度的获取，jQuery API中提供了 `width` 、`innerWidth` 、`outerWidth` 三个函数进行获取
 

- `$.fn.width()`

![width](https://terryz.github.io/image/document/width.png)


`width()` 获得的宽度，仅为元素自身的宽度，不包含内边距（padding），边框（border），外边距（margin）

 

- `$.fn.innerWidth()`

![inner-width](https://terryz.github.io/image/document/inner-width.png)


`innerWidth()` 获得的宽度，是在 `width()` 的基础上，再加上内边距（padding）的宽度


- `$.fn.outerWidth([includeMargin])`

![outer-width](https://terryz.github.io/image/document/outer-width.png)

`outerWidth()` 则是在 `innerWidth()` 的宽度上，增加了边框的宽度（border）

`outerWidth(true)`，设置了 boolean 参数是指定在元素的 `outerWidth()` 的宽度基础上，再增加外边距（margin）的宽度，通常对于元素的宽度计算，我们通常希望获得可见区域的宽度，外边距通常对元素可见区域没有影响；而在部分情况下，却需要使用外边框来撑开实际区域以处理目标的可见范围，此时则需要使用到 `outerWidth(true)`，多数情况下，并不需要获得外边距

以上对于 `width()` 、 `innerWidth()` 、`outerWidth([includeMargin])` 的描述，同理应用于 `height()` 、`innerHeight()` 、`outerHeight([includeMargin])`

### 获得精准的数值

通常情况下，使用以上的方式就可以获得目标元素的实际宽度，但有时需要对于宽度、高度、坐标等具体的数值，需要获得非常精准的数据（包含小数），例如在开发插件，开发复杂功能等情况；而上文中描述的方式，获得的数值都是整数，实际上对于数据中的小数，javascript、jquery所返回的数值中，是进行了四舍五入，但有时候所获得的数值和实际在浏览器中调试的数值进行四舍五入的值不一致，看样子是直接取整，既然如此，我们需要有办法可以获得详细的数据，自行在程序中进行结果的精准控制

在javascript中，对象的函数里有一个getBoundingClientRect()的API函数，返回值是一个 DOMRect 对象，这个对象是由该元素的 getClientRects() 方法返回的一组矩形的集合, 即：是与该元素相关的CSS 边框集合 。


如图所示，函数返回的内容，包含如下8个属性：

- bottom
- height
- left
- right
- top
- width
- x
- y

这里的8 个属性，所获得的数值俱为最精确的数值，包含小数的完整数据

那么以获得宽度为例，获得精确的宽度数值的方法为：

javascript
```js
document.getElementById('elementid').getBoundingClientRect().width
```

jQuery
```js
$('selector')[0].getBoundingClientRect().width
```
the image come from [jquery.com](jquery.com)

<br><br>

## 判断当前页面是否存在滚动条
```js
if($(document).height() > $(window).height()){
  //do something
}
```
<br><br>
