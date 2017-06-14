# Docker

## 使用镜像

### 1. 检查安装后的 Docker 版本
```
$ docker --version

$ docker-compose --version

$ docker-machine --version
```

### 2. 运行一个 Nginx 服务器
```sh
$ docker run -d -p 80:80 --name webserver nginx

$ docker stop webserver
$ docker rm webserver
```

### 3. [构建镜像](https://yeasy.gitbooks.io/docker_practice/content/image/build.html)
```
$ docker images #列出镜像
$ docker images -a #中间层镜像

$ docker build -t nginx:v3 .

$ docker exec -it webserver bash #进入容器
```

### 删除本地镜像
```
docker rmi [选项] <镜像1> [<镜像2> ...]
```
注意 `docker rm` 命令是删除容器，不要混淆。

## 操作容器

### [启动容器](https://yeasy.gitbooks.io/docker_practice/content/container/run.html)
下面的命令输出一个 “Hello World”，之后终止容器。
```
$ sudo docker run ubuntu:14.04 /bin/echo 'Hello world'
```

下面的命令则启动一个 bash 终端，允许用户进行交互。
```
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
```
其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开。

### 后台(background)运行
```
$ sudo docker run ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"

$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

此时容器会在后台运行并不会把输出的结果(STDOUT)打印到宿主机上面(输出结果可以用docker logs 查看)。  
注： 容器是否会长久运行，是和docker run指定的命令有关，和 -d 参数无关。  

使用 -d 参数启动后会返回一个唯一的 id，也可以通过 docker ps 命令来查看容器信息。
```
$ sudo docker ps
```

### 终止容器
可以使用 `docker stop` 来终止一个运行中的容器。  
终止状态的容器可以用 `docker ps -a` 命令看到。

### [进入容器](https://yeasy.gitbooks.io/docker_practice/content/container/enter.html)
在使用 `-d` 参数时，容器启动后会进入后台。 某些时候需要进入容器进行操作，有很多种方法，包括使用 `docker attach` 命令或 `nsenter` 工具等。

### [导出和导入容器](https://yeasy.gitbooks.io/docker_practice/content/container/import_export.html)
#### 导出容器
如果要导出本地某个容器，可以使用 docker export 命令。
```
$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:14.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ sudo docker export 7691a814370e > ubuntu.tar
```

#### 导入容器快照
可以使用 docker import 从容器快照文件中再导入为镜像，例如
```
$ cat ubuntu.tar | sudo docker import - test/ubuntu:v1.0
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
```

### 删除容器
可以使用 `docker rm` 来删除一个处于终止状态的容器。
```
$sudo docker rm  trusting_newton
```