# <div align="center">javascript - Size and position 尺寸和定位</div>

## 目录

- [屏幕当前滚动条位置](#屏幕当前滚动条位置)
- [屏幕可视区域](#屏幕可视区域)
- [全文档区域](#全文档区域)




<br><br><br><br><br><br><br>


## 屏幕当前滚动条位置

`window.pageXOffset` - 屏幕当前水平滚动条位置
`window.pageYOffset` - 屏幕当前垂直滚动条位置

为了跨浏览器兼容性，如果你使用了 `window.scrollX` 与 `window.scrollY`，请使用 `window.pageXOffset` 与 `window.pageYOffset` 代替 。另外，旧版本的 IE（<9）两个属性都不支持，必须通过其他的非标准属性来解决此问题。完整的兼容性代码如下

```js
var x = (window.pageXOffset !== undefined)
        ? window.pageXOffset
        : (document.documentElement || document.body.parentNode || document.body).scrollLeft
var y = (window.pageYOffset !== undefined)
        ? window.pageYOffset
        : (document.documentElement || document.body.parentNode || document.body).scrollTop
```

## 屏幕可视区域

**包含滚动条**

- `window.innerHeight` 获得屏幕可视区域高度
- `window.innerWidth` 获得屏幕可视区域宽度

`window.innerHeight` 与 `window.innerWidth` 的 ie 版本兼容为 9+，那么完整的兼容代码为

```js
function getInnerSizeWithScroll () {
  if (window.innerWidth) {
    return {
      width : window.innerWidth,
      height: window.innerHeight
    }
  } else if (document.documentElement.offsetWidth == document.documentElement.clientWidth) {
    return {
      width : document.documentElement.offsetWidth,
      height: document.documentElement.offsetHeight
    }
  } else {
    return {
      width : document.documentElement.clientWidth + getScrollWith(),
      height: document.documentElement.clientHeight + getScrollWith()
    }
  }
}
```

**不包含滚动条**

`document.documentElement.clientHeight` - 获得屏幕可视区域高度（不含滚动条的实际可用高度）
`document.documentElement.clientWidth` - 获得屏幕可视区域宽度（不含滚动条的实际可用宽度）

<br><br>

## 全文档区域

全文档区域即为网页所有内容的高度和宽度

`document.body.clientWidth` - 文档内容实际宽度
`document.body.clientHeight` - 文档内容实际高度

