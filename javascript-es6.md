# <div align="center">Javascript - ES6</center>

## 目录

- [定义变量](#定义变量)
- [数值扩展](#数值扩展)
- [字符串扩展](#字符串扩展)
- [数组扩展](#数组扩展)
- [对象扩展](#对象扩展)
- [解构赋值](#解构赋值)
- [模块化](#模块化)

<br><br><br><br><br><br>

## 定义变量

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

定义常量对象
```js
const a = {a:1,b:2};
a.b = 3;
console.log(a);//{a:1,b:3}
```
上例中，常量 a 中的内容在定义后，再进行修改依然有效，原因是对于对象类型的使用是指针式引用，常量只是指向了对象的指针，对象本身的内容却依然可以被修改，注意，数组(`Array`) 也是对象；
那么如果在定义常量时使用基础数据类型：`string`, `number`, `boolean` 等

在使用中，建议使用 `let` 与 `const` 完全代替 `var` 命令

<br><br>

## 数值扩展


<br><br>

## 字符串扩展


<br><br>

## 数组扩展


<br><br>

## 对象扩展


<br><br>

## 解构赋值


<br><br>

## 模块化


<br><br>
