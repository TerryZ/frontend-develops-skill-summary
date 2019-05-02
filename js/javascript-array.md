# javascript - Array 数组操作

## 目录
- [清空数组](#清空数组)
- [数组查找项目](#数组查找项目)
- [数组查找项目下标](#数组查找项目下标)
- [数组是否包含某个值](#数组是否包含某个值)
- [数组对象匹配](#数组对象匹配)
- [移除指定下标项目](#移除指定下标项目)
- [数组复制](#数组复制)
- [测试数组中的内容是否有满足条件的内容](#测试数组中的内容是否有满足条件的内容)
- [测试数组中所有项目是否均满足条件](#测试数组中所有项目是否均满足条件)
- [遍历数组](#遍历数组)
- [数组转换](#数组转换)
- [过滤数组](#过滤数组)


<br><br><br><br><br><br>

## 清空数组

清空数组有 5 种方式：

1. `arr = []`
2. `arr.length = 0`
3. `arr.splice(0, arr.length)`
4. `while (arr.length) { arr.pop(); }`
5. `while (arr.length) { arr.shift(); }`

根据 5 种方式的 [benchmark](http://jsben.ch/hyj65) 测试得出的性能排名结论为

1. length=0
2. new init
3. splice
4. pop
5. shift

所以最推荐的清空数组的方式为：

```js
arr.length = 0;
```

> 在 Vue 中使用，请不要使用 `arr.length = 0` 来清空数组，原因请 [点击查看](https://github.com/TerryZ/frontend-develops-skill-summary/blob/master/vue-base.md#Array-%E6%93%8D%E4%BD%9C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)。所以在 Vue 中清空数据，直接赋值为空数组即可

```js
arr = []
```

<br><br>

## 数组查找项目

<br><br>

## 数组查找项目下标

<br><br>

## 数组是否包含某个值

<br><br>

## 数组对象匹配

```js
[1,2,3] === [1,2,3]//false
```

在需要判断两个数组对象是否相等时，以上的方式得到的结果并不是我们所期待的。在使用上，并不建议对数组对象进行完整匹配，尤其是数组的内容、结构较复杂时（比如：`[{a:1,b:2}, 3424, [1,2,3]]`），但简单的数组数据内容（[1,2,3] 或 ['a','b','c']）可以使用以下的方式进行数组对象的完整匹配：

```js
const arr1 = [1,2,3], arr2 = [1,2,3];
if(arr1.sort().join('') === arr2.sort().join('')){
    //do something...
}
```

在实际的多数场景中，并不建议对数组对象做完整性匹配

<br><br>

## 移除指定下标项目

```js
const arr = [1, 2, 3, 4, 5];
arr.splice(2, 1);//移除第三个项目,下标从 0 开始
//[1,2,4,5]

// 使用 filter 也可以达到同样的效果
const new_arr = arr.filter((val,index) => index !== 2);
```

<br><br>

## 数组复制
```js
const arr = [1, 2, 3, 4, 5], newarr = arr.concat();
newarr[0] = 9;
console.log(arr);//[1,2,3,4,5]
console.log(newarr);//[9,2,3,4,5]
```
复制后的对象修改不影响原数组对象，但需要注意的是使用 `concat` 和 `slice` 复制的数组仅为浅拷贝，若数组中的项目为对象，则依然是引用的方式

<br><br>

## 测试数组中的内容是否有满足条件的内容
```js
const arr=[3, 5, 77, 2, 10];
const result = arr.some(item => item > 10);
console.log(result);//true
```
`some` 函数用于检查数组中是否有内容满足条件，只要有一项满足，即会返回 `true`

<br><br>

## 测试数组中所有项目是否均满足条件

```js
const arr = [3,5,77,2,10];
const result = arr.every(item => item > 0);
console.log(result);//true
```
`every` 函数用于检查数组中是否所有内容满足条件，只要有一项不满足，即会返回 `false`，当全部满足条件时，返回 `true` 值

<br><br>

## 遍历数组

ES5
```js
const arr = [1,2,3,4,5,6,7];
for(let i=0;i<arr.length;i++){
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
const arr = [1, 2, 3, 4, 5, 6], arr1 = ['A', 'B', 'C', 'D', 'E', 'F'];
const arrNew = arr.map((value, index, array) => value + arr1[index]);
console.log(arrNew);//['1A', '2B', '3C', '4D', '5E', '6F'];
```

<br><br>

## 过滤数组

```js
const arr = [1, 2, 30, 4, 15, 6];
const arrNew = arr.filter((value, index, array) => value < 10);
console.log(arrNew);//[1, 2, 4, 6]
```

不满足过滤条件的项目将不会被返回到新的数组中
