# <div align="center">Vue - Plugin 插件开发</div>

## 目录

- [插件打包与发布](#插件打包与发布)
- [图片资源打包](#图片资源打包)

<br><br><br><br><br><br>

## 插件打包与发布

插件功能开发完成后，若需要发布到公共组件库中（例如：npmjs），需要对插件进行打包并发布，简单说明一下这个过程，以插件名 `dialog` 为例

1. 创建 `dialog` 目录，并进入
2. 运行命令行，初始化项目，生成 `package.json`
```bash
npm init -y
```
3. 使用 `webpack-simple` 模板构建项目基本结构（前提为已自行安装好 `vue-cli`）
```bash
vue init webpack-simple
```
根据导航提示，设置好项目后，基本结构生成完成

4. 删除无用内容  
删除 `index.html` 和 `src` 目录下的所有文件

5. 复制插件内容到 `src` 目录中

6. 修改 `package.json` 配置内容
```js
{
  "name": "dialog",
  "description": "the dialog plguin",
  "version": "1.0.0",
  "author": "TerryZ <terry5@foxmail.com>",
  "license": "MIT",
  //删除原有的"priveate": true，发布到公共库的项目，不能设置该参数
  //增加 main 配置，设置插件在安装后的主入口文件
  "main": "dist/dialog.js",
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "dependencies": {
    "vue": "^2.5.11"
  },
  //增加插件关键字描述，非必须，按需设置
  "keywords": [
    "front-end",
    "javascript",
    "dialog",
    "vue",
    "vuejs"
  ],
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ],
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-env": "^1.6.0",
    "babel-preset-stage-3": "^6.24.1",
    "cross-env": "^5.0.5",
    "css-loader": "^0.28.7",
    "file-loader": "^1.1.4",
    "node-sass": "^4.5.3",
    "sass-loader": "^6.0.6",
    "vue-loader": "^13.0.5",
    "vue-template-compiler": "^2.4.4",
    "webpack": "^3.6.0",
    "webpack-dev-server": "^2.9.1"
  }
}
```
7. 修改 `webpack.config.js` 的 `output` 部分配置
```js
  output: {
	path: path.resolve(__dirname, './dist'),
	publicPath: '/dist/',
    //修改输出打包后的脚本文件名，该文件即是 package.json 中配置的 main 属性的对应文件
	filename: 'dialog.js',
    //增加以下库配置信息
	library: 'Dialog',
	libraryTarget: 'umd',
	umdNamedDefine: true
  }
```
8. 安装库，国内环境建议使用 `cnpm` 安装速度会快些
```bash
npm install
```
9. 编译插件
```bash
npm run build
```
10. 发布插件，确定你的插件名当前公共库中不存在，否而会发布失败
```bash
npm publish
```

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

如此调整配置后，再运行编译

```bash
npm run build
```

会发现编译到 `dist` 目录中的内容已经没有图片文件，只有编译完成的 `build.js` 和对应的 map 文件，所以在插件开发时，尽可能使用 CSS 处理样式，需要使用到图片或图标资源，尽可能也使用图片尺寸小的图片，方便打包处理
