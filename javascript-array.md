# javascript - Array 数组操作

## 目录
- [移除指定下标项目](#移除指定下标项目)
- [数组复制](#数组复制)
- [测试数组中的内容是否有满足条件的内容](#数组复制)
- [遍历数组](#遍历数组)
- [数组转换](#数组转换)



<br><br><br><br><br><br>

## 移除指定下标项目
```js
var arr = [1,2,3,4,5];
arr.splice(2,1);//移除第三个项目,下标从 0 开始
//[1,2,4,5]
```

<br><br>

## 数组复制
```js
var arr = [1,2,3,4,5];
var newarr = arr.concat();
newarr[0] = 9;
console.log(arr);//[1,2,3,4,5]
console.log(newarr);//[9,2,3,4,5]
```
复制后的对象修改不影响原数组对象，但需要注意的是使用 `concat` 和 `slice` 复制的数组仅为浅拷贝，若数组中的项目为对象，则依然是引用的方式

<br><br>

## 测试数组中的内容是否有满足条件的内容
```js
var arr=[3,5,77,2,10];
var result = arr.some(function(item){
  return item > 10;
});
console.log(result);//true
```
`some` 函数用于检查数组中是否有内容满足条件，只要有一项满足，即会返回 `true`

<br><br>

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

<br><br>

## 数组转换

```js
let arr = [1,2,3,4,5,6], arr1 = ['A', 'B', 'C', 'D', 'E', 'F'];
let arrNew = arr.map((value, index, array) => {
    return value + arr1[index];
});
console.log(arrNew);//['1A', '2B', '3C', '4D', '5E', '6F'];
```
