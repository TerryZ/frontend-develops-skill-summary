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

<br><br>
