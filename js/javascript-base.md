# <div align="center">Javascript - base 基本使用</center>

## 目录

- [表达式赋值](#表达式赋值)
- [Boolean值](#user-content-boolean值)
- [switch 中容易被忽视的细节](#user-content-switch-中容易被忽视的细节)
- [阻止页面离开](#阻止页面离开)
- [带颜色的日志输出](#带颜色的日志输出)
- [Base64 编码解码](#Base64-编码解码)

<br><br><br><br><br><br>

## 表达式赋值

或表达式赋值

> 表达式左边的值，若是空或是 `undefined` 等情况，则使用右边的内容进行赋值

```js
function (p) {
  const param = p || { a: 1 }
}
//等效于
function (p) {
  let param
  if (!p) param = { a: 1 }
}
```

与表达式赋值

> 表达式左边的值非空时，使用右边的值进行赋值

```js
function (p) {
  const param = p && { a: 1 }
}
//等效于
function (p) {
  let param
  if (p) param = { a: 1 }
}
```

<br><br>

## Boolean值

我们经常需要使用 `boolean` 类型的变量对程序状态进行控制或判断，除去原生 `boolean` 类型的 `true` 和 `false` 之外，还有很多其它类型的数据值，也可以作为 `boolean` 的结果进行判断

0, -0, null, "", undefined 或 NaN 等数据值，在判断时，与 `false` 等效

非 0 或 -0 的数字值，非空的字符串，包含 "true" 和 "false"的内容，与 `true` 等效

<br><br>

## `switch` 中容易被忽视的细节

通常我们使用条件判断是使用 `if` ，但如果需要判断的情况较多，或是对每一个枚举的值都需要做不同处理，就会用到 `switch` 语句，不同条件执行不同代码块，首先来看一段代码

```js
const num = '5'
switch (num) {
    case 5:
        console.log('result is ' + num)
        break
    default:
        console.log('this is default branch')
}
// this is default branch
```

执行的代码块好像和期待的结果不一样，代码走到了 `default` 分支。使用 `if` 语句来试试

```js
const num = '5';
if(num == 5)
    console.log('match');
else
    console.log('no match');
//match
```

使用 `if` 判断结果是正确的！那么，使用严格比较

```js
const num = '5';
if(num === 5)
    console.log('match');
else
    console.log('no match');
//no match
```

这样结果就和 `switch` 一致了，说明 `switch` 中对于判断是使用的严格判断，那么修改 `switch` 中的判断条件，结果就满足期望了

```js
const num = '5';
switch(num){
    case '5':
        console.log('result is ' + num);
        break;
    default:
        console.log('this is default branch');
}
//result is 5
```

<br><br>

## 阻止页面离开

在部分数据编辑的页面里，需要控制用户在关闭前作出提示，并允许用户选择确定和取消，选择取消则保持当前页面不会被关闭

```js
window.onbeforeunload = function(){
    return confirm("Do you really want to leave?");
};
```
confirm 中的主按钮点击后返回 `true`，取消按钮 返回 `false` 值  
在部分浏览器里 confirm 中指定的文本不会被显示，而显示的是浏览器固定显示的文本内容

<br><br>

## 带颜色的日志输出

在常规使用控制台对象 `console` 进行日志输出时，只有几种模式可以带颜色

- warn - 黄色
- error - 红色

实际上控制台也可以自带义输出日志的字体颜色

```js
console.log('%cThis is my log.', 'color: blue');
```

`%c` 指定了在它之后的文本都是带颜色的，而使用的具体颜色，就是第二参数指定的颜色内容

文本中部分内容使用颜色

```js
console.log('This is my %clog.', 'color: blue;font-weight: bold;');
```

`log.` 四个字符，就会使用蓝色及粗体进行打印

<br><br>

## Base64 编码解码

将文本内容使用 base64 编码进行加密

> ie10+ 以上的浏览器原生支持

```js
//binary to ascii(将字符串转进行 base64 编码)
window.btoa('abcdefghijklmnopqrstuvxyz');
//result: YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnh5eg==

//ascii to binary(将加密串解密为原字符串内容)
window.atob('YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnh5eg==');
//result: abcdefghijklmnopqrstuvxyz

```
