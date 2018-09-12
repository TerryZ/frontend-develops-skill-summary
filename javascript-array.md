# javascript - array 数组操作

## 目录
- [移除指定下标项目](#移除指定下标项目)
- [数组复制](#数组复制)
- [测试数组中的内容是否有满足条件的内容](#数组复制)
- [遍历数组](#遍历数组)



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

## 测试数组中的内容是否有满足条件的内容
```js
var arr=[3,5,77,2,10];
var result = arr.some(function(item){
  return item > 10;
});
console.log(result);//true
```
`some` 函数用于检查数组中是否有内容满足条件，只要有一项满足，即会返回 `true`

## 遍历数组

ES5
```js
var arr = [1,2,3,4,5,6,7];
for(var i;i<arr.length;i++){
    console.log(arr[i]);
}
```
ES6
```js
arr.forEach((value, index, array) => {
    console.log(value);
});
```
