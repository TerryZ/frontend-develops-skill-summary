
# <div align="center">Linux - Base 常用命令</div>

## 目录

- [NodeJS安装](#nodejs安装)

<br><br><br><br><br><br>

## NodeJS安装

在 [nodejs](https://nodejs.org/en/download/) 官网查看 nodejs linux x64 版本的下载链接

以当前 `8.11.1` 版本为例，获得的链接为 `https://nodejs.org/dist/v8.11.1/node-v8.11.1-linux-x64.tar.xz`

```bash
cd /usr/local/
wget https://nodejs.org/dist/v8.11.1/node-v8.11.1-linux-x64.tar.xz
#解包成tar格式
xz -d node-v8.11.1-linux-x64.tar.xz
#解压tar包到目录
tar -xvf node-v8.11.1-linux-x64.tar
```

编辑 profile 环境变量配置文件
```bash
vim /etc/profile
```

增加以下内容

```
#set for nodejs
export NODE_HOME=/usr/local/node-v8.11.1-linux-x64
export PATH=$NODE_HOME/bin:$PATH
```

安装淘宝 `npm` 镜像
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

创建软连接

```bash
ln -s /usr/local/node-v8.11.1-linux-x64/bin/node /usr/local/node
ln -s /usr/local/node-v8.11.1-linux-x64/bin/npm /usr/local/npm
ln -s /usr/local/node-v8.11.1-linux-x64/bin/cnpm /usr/local/cnpm
```

完成后，退出当前控制台，重新进入后，环境变量即可刷新，运行查看版本号命令来测试安装情况

```bash
node -v
npm -v
cnpm -v
```

<br><br>


