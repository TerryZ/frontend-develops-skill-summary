
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

### 启动容器常用参数

```bash
docker run -d --name my-mysql -it -p 13306:3306

# 指定容器名称
--name my-mysql
# 在后台运行
-d
# 进入命令交互模式
-it
# 端口映射，指定宿主机与容器内部的端口映射关系
# -p <宿主端口>:<容器内部端口>
-p 13306:3306
# 有需要对同个参数设置多处的，指定多次即可，例如：-p 1080:80 -p 13306:3306
```

### 启动容器并使用数据卷

```bash
docker run -d --name my-mysql -it -p 13306:3306 -v /data/mysql:/var/lib/mysql

# 数据卷，指定宿主机与容器内容数据目录的镜像关系，且互为同步
# -v <宿主目录位置>:<容器内部目录>
# 需要注意的是，映射双方，至少要在目录结构上保持一致，否则启动失败
# 可以先通过 docker cp <container-id>:/some-path/some-file /local-path 的方式先复制到本地后，再时行映射
# 挂载方式有如下的几种方式
# -v <容器内部目录> 匿名挂载
# -v <卷名>:<容器内部目录> 具名挂载
# -v <宿主目录位置>:<容器内部目录> 指定路径挂载
# 数据卷权限
# -v <宿主目录位置>:<容器内部目录>:ro 只能在宿主机目录中修改内容，容器内部只读，更推荐使用
# -v <宿主目录位置>:<容器内部目录>:rw 在宿主机和容器内部均可读写，为权限的默认形式
-v /data/mysql:/var/lib/mysql
```

### 通过 DockerFile 定义并生成镜像并应用数据卷同步

编写一个 `dockerfile` 文件并命名为 **dockerfile1** 如下

```dockerfile
FROM centos

VOLUME ["volume-test"]

CMD /bin/bash
```

通过 **docker build** 编译成一个镜像（这里的 `.` 指定了当前的路径）

```sh
docker build -f dockerfile1 -t centos-with-volume .

# 如此则生成了一个名为 centos-with-volume 的镜像，再通过运行镜像并生成一个新容器

docker run --name cv1 -it centos-with-volume /bin/bash

# 运行成功后，可在目录列表中看到除 centos 的基本目录之外，还多了一个 volume-test 的目录。此时通过命令再运行镜像生成
# 一个新容器，并使用数据共享命令来对已生成容器中的数据卷的内容进行同步共享

docker run --name cv2 --volumes-from cv1 -it centos-with-volume /bin/bash
```

此时，**cv1** 与 **cv2** 两个容器中的 `volume-test` 目录中的数据是完全同步的，可通过对两个容器内的文件进行修改来测试效果

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
docker commit -a="author-name" -m="commit-message" <container-id> new-image-name:tag

-a 指定提交的作者名称
-m 指定提交的描述信息
new-image-name:tag 指定了新镜像的名称以及标签（版本号）
```




