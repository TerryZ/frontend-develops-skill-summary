# mimiprogram - Caution 小程序开发特别注意

## 目录
- [wxs小程序脚本](#wxs小程序脚本)
- [AppData](#AppData)
- [wxml 中数据转换成字符串](#wxml-中数据转换成字符串)


<br><br><br><br><br><br>

## wxs小程序脚本

**日志不输出问题**

在 wxs 内部使用 console 输出日志，或是在 wxs 内部脚本执行错误，在浏览器的控制台中都不会打印相关的日志，给错误排查及问题跟踪带来了极大的困难

<br><br>

## AppData

微信开发者工具中的调试器，即是类似 Chrome 开发模式的调试面板，其中 AppData 是显示整个所有页面栈数据的调试面板。

<br><br>

## wxml 中数据转换成字符串

<br><br>
