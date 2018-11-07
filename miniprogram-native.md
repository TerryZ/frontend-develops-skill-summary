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


<br><br>
