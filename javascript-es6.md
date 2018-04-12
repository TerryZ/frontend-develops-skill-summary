# <div align="center">Javascript - ES6 实用技巧</center>

## 目录

- [定义变量/常量](#定义变量常量)
- [数值扩展](#数值扩展)
- [字符串扩展](#字符串扩展)
- [数组扩展](#数组扩展)
- [对象扩展](#对象扩展)
- [解构赋值](#解构赋值)
- [模块化](#模块化)

<br><br><br><br><br><br>

## 定义变量/常量

ES6 中新增加了 `let` 和 `const` 两个命令，`let` 用于定义变量，`const` 用于定义常量
两个命令与原有的 `var` 命令所不同的地方在于，`let, const` 都是块级作用域，其有效范围仅在代码块中，实例如下：

```js
//es5
if(1==1){
  var b = 'foo';
}
console.log(b);//foo

//es6
if(1==1){
  let b = 'foo';
}
console.log(b);//undefined
```

**定义常量对象**
```js
const a = {a:1,b:2};
a.b = 3;
console.log(a);//{a:1,b:3}
```
上例中，常量 a 中的内容在定义后，再进行修改依然有效，原因是对于对象类型的使用是指针式引用，常量只是指向了对象的指针，对象本身的内容却依然可以被修改，注意，数组(`Array`) 也是对象；
那么如果在定义常量时使用基础数据类型：`string`, `number`, `boolean` 等

```js
const a = 1;
a = 2;// Uncaught TypeError: Assignment to constant variable.
```

在使用中，建议使用 `let` 与 `const` 完全代替 `var` 命令

<br><br>

## 数值扩展

**转换**
`Number.parseInt` - 将字符串或数字转换为整数
`Number.parseFloat` - 将字符串或数字转换为浮点数

`Number.parseInt`, `Number.parseFloat` 与 `parseInt`, `parseFloat` 功能一致，在ES6中，推荐使用 `Number.` 的方式进行调用，这么做的目的是为了让代码的使用方式尽可能减少全局性方法，使用得语言逐步模块化

**测试函数**
```js
//测试是否整数
Number.isInteger(21)//true
Number.isInteger(1.11)//false

//测试是否NaN
Number.isNaN(Nan)//true
Number.isNaN(1)//false
```



<br><br>

## 字符串扩展

**字符串内容测试**

```js
'abcdef'.includes('c');//true
'abcdef'.includes('ye');//false
'abcdef'.startsWith('a');//true
'abcdef'.endsWitch('f');//true

//includes(), startsWith(), endsWith() 都支持第二个参数，
//类型为数字类型，意为从第 n 个字符开始，endsWith()的第二个参数有点不一样
'abcdef'.includes('c', 4);//false 从第5个字符开始查找是否有 'c' 这个字符
'abcdef'.startsWith('d', 3);//true 从第4个字符开始查找是否是以 'd' 字符为开头
'abcdef'.endsWith('d', 4);//true 前面的4个字符里，是否以 'd' 字符为结尾
```

**字符串内容重复输出**
```js
'a'.repeat(5);//aaaaa 重复输出5遍
```

**原生支持模板语言**
```js
//es5
$('#result').append(
  'There are <b>' + basket.count + '</b> ' +
  'items in your basket, ' +
  '<em>' + basket.onSale +
  '</em> are on sale!'
);
//es6
//在es6中，内容模板，可以定义在 `` 包起来的字符串中，其中的内容会保持原有格式
//另外可以在字符串中直接使用模板语言进行变量填充
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

**字符串遍历输出**
```js
//for ...of 格式为 es6 中的 Iterator 迭代器的输出方式
for(let c of 'abc'){
  console.log(c);
}
//a
//b
//c
```

**字符串补全**
```js
//参数1：[number] 目标字符串长度
//参数2：[string] 进行补全的字符串
'12345'.padStart(7, '0')//0012345 - 字符串不足7位，在头部补充不足长度的目标字符串
'12345'.padEnd(7, '0')//1234500 - 在尾部进行字符串补全
```

<br><br>

## 数组扩展

**合并数组**

```js
let a = [1, 2];
let b = [3];
let c = [2, 4];
let d = [...a, ...b, ...c];//[1, 2, 3, 2, 4] 所有内容合并，但并不会去除重复
```

**快速转换为数组**

```js
Array.of(3,4,5)//[3,4,5]
```

**数组内容测试**

```js
//判断对象是否为数组
if(Array.isArray(obj)){...}

[1,2,3].includes(5);//false，检索数据中是否有5

//找出第一个匹配表达式的结果，注意是只要匹配到一项，函数即会返回
let a = [1, 3, -4, 10].find(function(value, index, arr){
  return value < 0;
});
console.log(a);//-4

//找出第一个匹配表达式的结果下标
let a = [1, 3, -4, 10].findIndex(function(value, index, arr){
  return value < 0;
});
console.log(a);//2
```

**内容过滤**

```js
//排除负数内容
let a = [1, 3, -4, 10].filter(function(item){
  return item > 0;
});
console.log(a);//[1, 3, 10]
```

**内容实例**

`.keys()` - 获得数组中所有元素的键名（实际上就是下标索引号）

`.values()` - 获得数组中所有元素的数据

`.entries()` - 获得数组中所有数据的键名和数据

```js
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```
`.entries()`, `.keys()`, `.values()` 功能与 `Object` 中的几个同名函数功能类似，实际使用中实用性不高，对于数组的操作，直接进行遍历即可


<br><br>

## 对象扩展

**属性的简洁表示**

```js
//直接使用变量/常量的名称个为对象属性的名称
let a = 'abc';
let b = {a};//{a: 'abc'}

function f(x, y){ return {x, y};}
//等效于
function f(x, y){ return {x: x, y: y}}

let o = {
  f(){ return 1; }
}
//等效于
let o = {
  f: function(){ return 1; }
}
```

**判断对象是否为数组**

```js
if(Object.isArray(someobj)){}
```

**对象内容合并**

```js
let a = {a:1,b:2}, b = {b:3}, c = {b:4,c:5};
let d = Object.assign(a, b, c);
console.log(d);//{a:1,b:4,c:5}
console.log(a);//{a:1,b:4}
//上面的合并方式会同时更新 a 对象的内容，a 的属性如果有多次合并会被更新数据，
//但自身没有的属性，其它对象有的属性不会被添加到 a 身上；
//参数列中的对象只会影响第一个，后面的参数对象不会被修改数据

//推荐使用这种方式进行对象数据合并
let a = {a:1,b:2}, b = {b:3}, c = {b:4,c:5};
let d = Object.assign({}, a, b, c);//第一个参数增加一个空对象，在合并时让它被更新，不影响实际的对象变量内容
console.log(d);//{a:1,b:4,c:5}//与上面的方式合并结果一致，使用这种方式, a 对象的内容就不会被影响了
```
对象内容合并的方向是从参数顺序的后向前合并

**对象内容集合**

`Object.keys()` - 获得对象中所有的键名，以数组的形式返回
```js
var obj = { a:1,b:2 };
var names = Object.keys(obj);//['a', 'b']
```

`Object.values()` - 获得对象中所有的值内容，以数组的形式返回
```js
var obj = { a:1,b:2 };
var values = Object.values(obj);//[1, 2]
```

`Object.entries()` - 获得对象中所有的成员数据，以数组的形式返回，成员的内容也是数组形式
```js
var obj = { a:1,b:2 };
var values = Object.entries(obj);//[['a',1], ['b',2]]
```

其实观察可发现，`Object.keys()`, `Object.values()`, `Object.entries()`，与 `Java` 的 `MAP` 中的方法是一致的，不论是方法名还是具体的用法，这也可以帮忙理解这些功能 API

<br><br>

## 解构赋值


<br><br>

## 模块化


<br><br>
