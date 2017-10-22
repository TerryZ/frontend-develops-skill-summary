# <div align="center">Javascript - base 基本使用</center>

## 目录

- [switch 中容易被忽视的细节](#user-content-switch-中容易被忽视的细节)

<br><br><br><br><br><br>

## `switch` 中容易被忽视的细节

通常我们使用条件判断是使用 `if` ，但如果需要判断的情况较多，或是对每一个枚举的值都需要做不同处理，就会用到 `switch` 语句，不同条件执行不同代码块，首先来看一段代码

```js
var num = '5';
switch(num){
    case 5:
        console.log('result is ' + num);
        break;
    default:
        console.log('this is default branch');
}
//this is default branch
```

执行的代码块好像和期待的结果不一样，代码走到了 `default` 分支。使用 `if` 语句来试试

```js
var num = '5';
if(num == 5)
    console.log('match');
else
    console.log('no match');
//match
```

使用 `if` 判断结果是正确的！那么，使用严格比较

```js
var num = '5';
if(num === 5)
    console.log('match');
else
    console.log('no match');
//no match
```

这里的结果就和 `switch` 的结果一致了，说明 `switch` 中对于判断是使用的严格判断，那么修改 `switch` 中的判断条件，结果就满足期望了

```js
var num = '5';
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
