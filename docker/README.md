# Docker

## 1. 检查 Docker 版本

```
$ docker --version
Docker version 18.09, build c97c6d6

$ docker-compose --version
docker-compose version 1.23.2, build 8dd22a9

$ docker-machine --version
docker-machine version 0.16.0, build 9ba6da9
```

## 2. 运行一个 Nginx 服务器

1. Start a Dockerized web server. If the image is not found locally, Docker pulls it from Docker Hub.

```sh
$ docker run --detach --publish=80:80 --name=webserver nginx
```

2. In a web browser, go to http://localhost/ to view the nginx homepage. 
![](images/images-mac-example-nginx.png)

3. View the details on the container while your web server is running (with docker container ls or docker ps):

```
$ docker container ls
CONTAINER ID   IMAGE   COMMAND                  CREATED              STATUS              PORTS                         NAMES
56f433965490   nginx   "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   webserver
```

4. Stop and remove containers and images with the following commands. Use the “all” flag (--all or -a) to view stopped containers.

```
$ docker container ls
$ docker container stop webserver
$ docker container ls -a
$ docker container rm webserver
$ docker image ls
$ docker image rm nginx
```

### Docker 官网: [Get Started with Docker](https://docs.docker.com/get-started/)

## 3. 使用镜像

### [获取镜像](https://yeasy.gitbooks.io/docker_practice/content/image/pull.html)

[Docker Hub](https://hub.docker.com/search/?q=&type=image) 上有大量的高质量的镜像可以用，这里我们就说一下怎么获取这些镜像。
从 Docker 镜像仓库获取镜像的命令是 docker pull。其命令格式为：

```
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

### [列出镜像](https://yeasy.gitbooks.io/docker_practice/content/image/list.html)

```
docker image ls
```

### [删除镜像](https://yeasy.gitbooks.io/docker_practice/content/image/rm.html)

```
docker image rm [选项] <镜像1> [<镜像2> ...]
```

使用 docker image ls -q 来配合使用 docker image rm，这样可以成批的删除希望删除的镜像。  
比如，我们需要删除所有仓库名为 redis 的镜像：

```
docker image rm $(docker image ls -q redis)
```

## 3. 操作容器

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


```
$ docker exec -it webserver bash #进入容器
```

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

可以使用 `docker stop` 来终止一个运行中的容器。终止状态的容器可以用 `docker ps -a` 命令看到。  

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

## 4. [使用 Dockerfile 定制镜像](https://yeasy.gitbooks.io/docker_practice/content/image/build.html)

在 Dockerfile 文件所在目录执行：  

```
$ docker build [选项] <上下文路径/URL/->

$ docker build -t nginx:v3 .
```

新的镜像定制好后，我们可以来运行这个镜像。

```
docker run --name web3 -d -p 83:80 nginx:v3
```

打开 http://localhost:83 查看运行结果。