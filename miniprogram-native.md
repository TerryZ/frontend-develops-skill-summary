# mimiprogram - Native 小程序原生

## 目录
- [setData](#setData)


<br><br><br><br><br><br>

## setData

setData 是一个将 data 中定义的数据进行修改并刷新到视图的函数。但需要注意的是，它是一个异步函数，如果刷新的数据内容较多，需要注意后续的东西要在 setData 的回调中进行处理，否则会有数据不准确的问题

```js
this.setData({
    a: this.data.a,
    b: this.data.b
},()=>{
    //do your stuff
});
```

**性能优化**

由于 setData 会修改数据并刷新视图，所以在执行 setData 应尽可能只更新必要更新的数据，以免浪费渲染性能，造成界面卡顿的感觉；在更新 Object 或 Array 若能明确需要更新的位置，应更新指定位置的内容

```js
this.setData({
    'list[1].name': 'zhangsan'
});
```

路径变量

```js
let path = 'list[' + i+ '].name'; 
this.setData({
    [path]: 'zhangsan'
});
```

<br><br>

## wxml 中数据转换成字符串

<br><br>

## navigateTo与redirectTo

`wx.navigateTo` 与 `wx.redirectTo` 以功能来说，同样是跳转到新页面的功能函数，它们之间有几点区别

- `wx.navigateTo` 保留当前页面，而 `wx.redirectTo` 会关闭当前页面
- 同是在目标页面的 url 中增加与 Web Url类似的链接参数，并可在目标页面的 `onLoad(options)` 中获取

<br><br>
