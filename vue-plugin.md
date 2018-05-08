# <div align="center">Vue - Plugin 插件开发</div>

## 目录

- [插件打包](#插件打包)
- [图片资源打包](#图片资源打包)

<br><br><br><br><br><br>

## 插件打包



<br><br>

## 图片资源打包

插件中使用到的图片资源，在打包后，根据模板的默认配置，会将图片资源输出到 `dist` 目录中，此时就有图片引用路径问题。在样式内容中会发现原来设置的

`background-image:url('../image/a.jpg')` 

会转换成

`background-image:url('/dist/a.jpg')`

实际的完整路径即是

`http://xxx.com/dist/a.jpg`

我们知道插件在安装后，会统一安装在 `node_modules` 目录中，这样的目录显然是不正确的

一种折中的办法，就是将图片资源转换成 `base64` 编码，不生成实体图片

`webpack-simple` 中默认使用 `file-loader` 来处理图片，这里换成 `url-loader`，两者的区别在于后者是前者的功能封装，还增加了将图片编码为 `base64` 的功能，所以可以放心使用

*安装 `url-loader`*
```bash
npm i url-loader --save-dev
```

`webpack` 配置修改(`webpack-simple` 模板)
```js
module.exports = {
  ...
  module: {
    rules: [
      {...},
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'url-loader',
        options: {
		  limit: 30000,
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  ...
}
```
上面的配置中，将原 `file-loader` 更换了 `url-loader`，并增加 `limit` 参数，该参数设置了图片容量在小于指定容量(上例设置为30kb)时，不会转换成 `base64`
