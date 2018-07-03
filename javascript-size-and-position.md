# <div align="center">javascript - Size and position 尺寸和定位</div>

## 目录

- [屏幕当前滚动条位置](#屏幕当前滚动条位置)
- [对于宽、高度的获取，及精准数值的获取方式（含小数点）](#user-content-对于宽高度的获取及精准数值的获取方式含小数点)
- [判断当前页面是否存在滚动条](#判断当前页面是否存在滚动条)




<br><br><br><br><br><br><br>


## 屏幕当前滚动条位置

`window.pageXOffset` - 屏幕当前水平滚动条位置
`window.pageYOffset` - 屏幕当前垂直滚动条位置

为了跨浏览器兼容性，如果你使用了 `window.scrollX` 与 `window.scrollY`，请使用 `window.pageXOffset` 与 `window.pageYOffset` 代替 。另外，旧版本的 IE（<9）两个属性都不支持，必须通过其他的非标准属性来解决此问题。完整的兼容性代码如下

```javascript
var x = (window.pageXOffset !== undefined) ? 
        window.pageXOffset : 
        (document.documentElement || document.body.parentNode || document.body).scrollLeft;
var y = (window.pageYOffset !== undefined) ? 
        window.pageYOffset : 
        (document.documentElement || document.body.parentNode || document.body).scrollTop;
```
