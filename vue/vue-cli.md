# <div align="center">Vue - CLI 脚手架</div>

## 目录

- [环境变量](#环境变量)

<br><br><br><br><br><br>

## 环境变量

在 cli-2.x 中，环境变量的设置可以在 

```
/config/dev.env.js
/config/prod.env.js
```

文件中进行配置，而在 cli-3.x 版本中，不再有以上的配置，对于环境变量的配置，需要自行增加文件

**开发环境变量配置文件**

`.env.development`

文件内容样例

```
//声明环境
NODE_ENV=development

//环境变量
VUE_APP_API=http://localhost
```

**生产环境变量配置文件**

`.env.production`

文件内容样例

```
NODE_ENV=production

//server side base path
VUE_APP_API=/api
//file upload server path
FILE_UPLOAD_API=/api
```
