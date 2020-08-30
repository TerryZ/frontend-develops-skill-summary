
# <div align="center">DevOps - Docker - Learning 学习笔记</div>

### 启动 docker 服务

刚安装完成或有时 docker daemon 没有正确启动，在运行多数命令时，会显示

`Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`

通过执行命令：`service docker start`
则 docker 服务运行正常

### 设置 docker 服务开机启动

```bash
# 设置服务开机启动
systemctl enable docker.service
# 关闭服务开机启动
systemctl disable docker.service
```

### 查询镜像

`docker search <image-name>`

### 本地已下载镜像查询

```bash
docker image ls
docker images
```

### 查询当前正在运行的镜像

`docker ps`

### 运行一个镜像

```bash
docker run <image-name>
# 后台运行一个镜像，但注意需要后续的指定端口等参数，否则会运行不成功
docker run -d <image-name>
```

### 退出容器
```bash
# 退出且终止运行
exit
# 退出容器且保留容器运行
ctrl + p + q
```

### 启动和停止容器

```bash
# 启动容器
docker start <container-id>
# 重启容器
docker restart <container-id>
# 停止正在运行的指定容器
docker stop <container-id>
# 强制停止指定容器
docker kill <container-id>
# 使容器自启动
docker run --restart=always
# 运行过的项目自启动
docker update --restart=always
```

### 查看正在运行中的容器资源占用情况

`docker stats`

### 查看容器日志

```bash
# 查看容器日志
docker logs <container-id>
# 查看容器日志并启用跟随模式，即是在查看的过程中又出现新的日志内容，就自动追加在控制台中
docker logs -f <container-id>
# 跟随模式查看容器日志并显示日志时间轴
docker logs -f -t <container-id>
# 跟随模式并显示时间轴查看容器日志，通过参数控制显示条目数
docker logs -f -t --tail all <container-id>
```

### 查看容器进程信息

`docker top <container-id>`

### 查看容器元数据

`docker inspect <container-id>`

### 进入容器

```bash
# 进入容器并生成一个新的终端
docker exec -it <container-id> /bin/bash
# 进入容器并应用正在运行的终端，不启动新的进程
docker attach <container-id>
# 退出容器
exit
```

### 复制文件

```bash
# 在宿主机器中复制指定容器内容的文件
docker cp <container-id>/some-folder/some-file /target-path
```

### 将处理后的内容提交为一个新镜像

1. 下载并运行一个镜像
1. 在容器中运行成功后，将容器中的文件内容进行修改后，达到想的效果
1. 将容器的内容提交为一个新的镜像

```bash
`docker commit -a="author-name" -m="commit-message" <container-id> new-image-name:tag`

-a 指定提交的作者名称
-m 指定提交的描述信息
new-image-name:tag 指定了新镜像的名称以及标签（版本号）
```




