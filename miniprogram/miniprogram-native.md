# mimiprogram - Native 小程序原生

## 目录

- [开发环境建议](#开发环境建议)
- [setData](#setData)
- [navigateTo与redirectTo](#navigateTo与redirectTo)
- [z-index](#z-index)
- [dev and product](#dev-and-product)
- [特殊字符](#特殊字符)


<br><br><br><br><br><br>

## 开发环境建议

vscode 安装插件 `miniapp`，让 vscode 可以识别小程序原生的的代码文件类型，支持小程序标签语法高亮

工作模式为编码、管理均在 vscode 上进行，查看效果时切换到微信开发者工具上进行查看，由于微信开发者工具可以实时响应文件的修改或变更，并会重新刷新小程序的预览视图

之所以微信开发者工具只使用预览功用，主要是因为作为编辑器的整体功能并不完善，甚至容易导致开发者工具崩溃自动重启，整体开发体验较差，另外开发者工具对于开发功能扩展的支持较差，插件的安装使用并不支持

## 微信小程序完整登录流程

**Check storage**

getStorage(sessionKey) -> checkSession -> fail to login

**Login**

wx.login -> receive code and request to server side ->  receive openId, sessionKey and unionId

## setData

setData 是一个将 data 中定义的数据进行修改并刷新到视图的函数。但需要注意的是，它是**异步**的，如果刷新的数据内容较多，需要注意后续的东西要在 setData 的回调中进行处理，否则会有数据不准确的问题

```js
this.setData({
  a: this.data.a,
  b: this.data.b
}, () => {
  // do your stuff
})
```

**性能优化**

由于 setData 会修改数据并刷新视图，所以在执行 setData 应尽可能只更新必要更新的数据，以免浪费渲染性能，造成界面卡顿的感觉；在更新 Object 或 Array 若能明确需要更新的位置，应更新指定位置的内容

```js
this.setData({
  'list[1].name': 'zhangsan'
})
```

路径变量

```js
const path = 'list[' + i + '].name'
this.setData({
  [path]: 'zhangsan'
})
```

## navigateTo与redirectTo

`wx.navigateTo` 与 `wx.redirectTo` 以功能来说，同样是跳转到新页面的功能函数，它们之间有几点区别

- `wx.navigateTo` 保留当前页面，而 `wx.redirectTo` 会关闭当前页面
- 同是在目标页面的 url 中增加与 Web Url类似的链接参数，并可在目标页面的 `onLoad(options)` 中获取
- `wx.navigateBack` 可以返回通过 `wx.navigateTo` 进行跳转的界面，通过 `wx.redirectTo` 进行跳转的不能返回
- 微信小程序规定 `wx.navigateTo` 只允许有五层页面路径，当已经有五层页面路径时，再进行跳转，将不会执行跳转页面功能。
- `wx.navigateBack` 可通过 `getCurrentPages()` 函数获得页面栈，并决定需要返回的层级

为了防止小程序页面跳转不了的情况常，页面切换功能应按需使用，部分操作完成后不需要保留的页面，应尽可能使用 `wx.redirectTo` 进行跳转

## z-index

对页面进行排版时，有时需要做上下层级控制。例如 A 和 B 都是一个 view 标签，A 需要 B 上层，在 A 和 B都设置了 `z-index` 值后，层级的效果并不会生效；此时需要关注 A 和 B 是否都指定了 `position` 样式属性，在都设置了 `position` 样式属性的情况下，层级效果才可以生效

**注意**：在 `position:static` 设置下无效

## dev and production

小程序针对线上版本（正式版本）、体验/开发版本以及本地开发环境中的不同环境的服务端地址自动切换功能

微信官方对于环境变量的需求十分傲慢地不予提供，该功能的缺失，使得对于当前环境的类型无法做出权威的鉴别，而导致不同环境的不同配置无法执行有效的切换。另外在本地存储方面也存在着预览版与线上版共通的问题，相对来说，如果小程序对本地存储的应用较多，该问题将更加致命，将导致预览版/开发版中调试的数据可被线上版本直接使用，而这种情况将导致开发团队在使用和测试上出现环境不准确的问题，但对于普通的终端客户影响不大

### 解决方案

在程序内部设置一个环境变量，用于区别当前环境，需要进行切换的包含服务器地址、操作行为及相关系统变量均根据该变量进行切换

如果有会用本地缓存的情况，应根据环境变量设置缓存键值前缀，例如

- 体验版：dev-user-id
- 线上版：user-id

针对缓存的操作同时建议进行简易的封装，可针对数据的存、取进行统一处理

### 不足之处

每次切换环境都需要手动修改内部环境变量，并在修改完成后需要单独为此再更新一次版本完成切换回预览版本的操作，较为繁琐，且没有有效的防呆措施

## 特殊字符

在网页上常用的特殊字符，在微信小程序的页面不能直接使用，需要使用时需要使用 text 或 view 标签进行包裹，并设置 `decode` 属性为真值，方可正常使用
```html
<text decode="{{true}}">&nbsp;</text>
```
