# Javascript - Dom 操作

## 目录

- [Json数据的相关操作](#user-content-修改页面标题)

### 修改页面标题

有时我们需要在不同的情况下或不同的状态下，实时修改网页的标题，然而修改标题不需要去获取标签对象之类的一系列操作，却只需要简单的一行代码即可解决
```js
document.title = 'new title';
```
若要使用jquery的方式也很简单
```js
$(document).attr('title','new title');
```

