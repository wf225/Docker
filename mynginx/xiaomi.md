### 删除本地镜像
docker rmi [选项] <镜像1> [<镜像2> ...]

### 删除容器
docker rm  trusting_newton

### 进入容器
```
$ sudo docker run -idt ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$sudo docker attach nostalgic_hypatia
root@243c32535da7:/#
```

### xiaomi
```
docker ps -a

docker build -t ubuntu:xiaomi .

docker run -t -i ubuntu:xiaomi /bin/bash

docker attach nostalgic_hypatia

apt-get update
apt-get install unzip
unzip sdk_package.zip

export PATH=$PATH:/usr/xiaomi/sdk_package/toolchain/bin

```

### 编译 main.c
```
#include <stdio.h>

int main()
{
    printf("Hello, world!\n");
    return 0;
}
```

编译、链接：
```
arm-xiaomi-linux-uclibcgnueabi-gcc -o hello hello.c
```
把生成的hello文件复制到小米路由器中，运行后显示：Hello, world!