# <div align="center">jQuery - base 基本使用</div>

## 目录

- [判断元素是显示还是隐藏](#判断元素是显示还是隐藏)
- [Json数据的相关操作](#user-content-json数据的相关操作)
- [数组的相关操作](#数组的相关操作)
- [$.each 遍历中的 continue 和 break](#user-content-each-遍历中的-continue-和-break)
- [Javascript对象复制引用机制及$.extend复制特点](#user-content-javascript对象复制引用机制及extend复制特点)

<br><br><br><br><br><br>

## 判断元素是显示还是隐藏

```js
// 判断元素是否为隐藏状态
if ($(obj).is(':hidden')) {
  // do something
}
// 判断元素是否为显示状态
if ($(obj).is(':visible')) {
  // do something
}
```

## Json数据的相关操作

其实使用 `$.each` 也可以操作 object 对象
```js
var data = { a: 11, b: 22, c: 33 }
$.each(data,function (i, n) {
  console.log(i + ' - ' + n)
})
// a - 11
// b - 22
// c - 33
```

在操作数组时，callback的第一个参数是数组下标，在操作对象是就是Key；第二参数是数组的具体元素，操作对象时，是属性的值内容

## 数组的相关操作

数组数据反转操作

```js
$(selector).reverse().each(...)
```

这里的 `reverse()` 函数是 Javascript 数组(Array)原生的方法，功能是将数组的内容进行倒置

## $.each 遍历中的 `continue` 和 `break`

我们知道在原生 javascript 的循环遍历 `for` 中控制循环跳到下一轮和中断循环的语句是 `continue` 和 `break` 。而在使用 jquery 的过程中，`$.each` 可以是说是 `for` 是更简单好用，功能更强大的替代者，但是， `$.each` 是不支持 `continue` 和 `break` 的，想实现循环控制的功能则需要使用 `return` 表达式，这也是很多新人在循环中想在满足一定条件后，直接退出 function 并返回相应的值，却发现结果和期望不一致的原因

- return true     - `continue`
- return false    - `break`

**example**

```js
// only need top 5 data in array
var arr = [1,2,3,4,5,6,7], newarr = []
$.each(arr,function (index,row) {
  if (index > 5) return false
  newarr.push(row)
})
console.log(newarr.toString())
// 1, 2, 3, 4, 5
```

## Javascript对象复制引用机制及$.extend复制特点

首先复习一下javascript中的变量类型

**基本类型**

- number
- string
- boolean
- undefined
- null

**引用类型**

- function
- array
- date
- 正则
- 错误

基本类型变量的复制，内容修改后，不会对另一变量产生影响

```js
var a = 1;
var b =a;
a = 2;
console.log(a);//2
console.log(b);//1
```

引用类型变量若将A对象内容赋值给B对象，则B对象只是A对象的引用，此时若修改了B的内容，A的内容也会随之变化

```js
var a = {a:1,b:2};
var b = a;
b.b = 3;
console.log(a);//{a:1,b:3}
console.log(b);//{a:1,b:3}
```

以此得出的结论，在Javascript下，将一个对象类型变量赋值仅得到对象的引用。

但在某些情况下，出于对功能上的要求，必须要求变量在获得另一个对象的复制后，各自修改内容，不会互相影响；使用Javascript原生可参照以下方式：

```js
var arr = {"a":"1","b":"2"};
var arr1 = arr;
arr = {};
arr["a"] = 2;
console.log(arr);//{"a":2}
console.log(arr1);//{"a":"1","b":"2"}
```

原理是A变量是B对象变量的引用时，将A置空，再为A设置内容，就不会再是B的引用



还可以利用 `JSON.stringify` 方法和jQuery的 `$.parseJSON` 方法进行处理（略麻烦）

```js
var data = {a:1,b:2,c:3,d:[0,1,2,3]};
var str = JSON.stringify(data);
var data1 = $.parseJSON(str);
data1["e"] = 5;
data1["d"][0] = 22;
console.log(data);//{a: 1, b: 2, c: 3, d: [0,1,2,3]}
console.log(data1);//{a: 1, b: 2, c: 3, d: [22,1,2,3], e: 5}
```

这种转换的过程实际就是重新生成对象了
 

使用jQuery的 `$.extend()` 方法来扩展、复制对象

```js
var a = {a:1,b:2};
var b = $.extend({},a);
b.b = 3;
console.log(a);//{a:1,b:2}
console.log(b);//{a:1,b:3}
```

以上的方式，若A变量的属性里还有子对象，例如：
```js
{
  a: 1,
  b: 2,
  c: {
    aa: 11,
    bb: 22
  }
}
```
在进行复制时，子对象的内容依然是引用的方式，会互相影响

```js
var a = {a:1,b:2,c:{aa:11,bb:22}};
var b = $.extend({},a);
b.b = 3;
b.c.aa = 33;
console.log(a);//{a:1,b:2,c:{aa:33,bb:22}}
console.log(b);//{a:1,b:3,c:{aa:33,bb:22}}
```


使用 `$.extend()` 方法的深度复制方式，即可杜绝以上问题，使用深度复制，即使母对象中子对象有再多层级，依然会进行完整复制，并不会进行对象引用

```js
var a = {a:1,b:2,c:{aa:11,bb:22}};
var b = $.extend(true,{},a);
b.b = 3;
b.c.aa = 33;
console.log(a);//{a:1,b:2,c:{aa:11,bb:22}}
console.log(b);//{a:1,b:3,c:{aa:33,bb:22}}
```
<br><br>
