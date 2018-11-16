# mimiprogram - Optmize 性能优化

## 目录
- [setData](#setData)


<br><br><br><br><br><br>

## setData


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

合并多次设置，执行一次 setData 就会进行视图刷新，在一次功能任务中若执行了多次 setData 就会多次刷新视图，造成性能浪费

```js
//bad
this.setData({a:1});
this.setData({b:2});
this.setData({c:3});
//good
this.setData({
    a: 1,
    b: 2,
    c: 3
});
```

<br><br>
