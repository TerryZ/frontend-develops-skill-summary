# javascript - array 数组操作

## 目录
- [移除指定下标项目](#移除指定下标项目)
- [数组复制](#数组复制)



<br><br><br><br><br><br>

## 移除指定下标项目
```js
var arr = [1,2,3,4,5];
arr.splice(2,1);//移除第三个项目,下标从 0 开始
//[1,2,4,5]
```

## 数组复制
```js
var arr = [1,2,3,4,5];
var newarr = arr.concat();
newarr[0] = 9;
console.log(arr);//[1,2,3,4,5]
console.log(newarr);//[9,2,3,4,5]
```
复制后的对象修改不影响原数组对象
