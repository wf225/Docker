## Linux(Ubuntu)系统查看版本及内核信息
```
cat /etc/issue

lsb_release -a

cat /etc/lsb-release

cat /proc/version

uname -a
```

## Download sdk_package
http://bigota.miwifi.com/xiaoqiang/sdk/tools/package/sdk_package.zip

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

编译：g++ -static -o test test.cpp

注意这个地方有个参数-static，这个提示编译器我们需要一个静态链接的程序，否则在路由器上会有动态库依赖问题。


## Cross-compiling Nodejs

### Download node
```
apt-get install git-core
apt-get install python
apt-get install make

git clone https://github.com/nodejs/node

./configure --without-snapshot --prefix=/output
make
```

### Set ENV
```
export AR=/route/to/openwrt/folder/staging_dir/toolchain-name-of-your-architecture-something/bin/your-architecture-ar
export CC=/route/to/openwrt/folder/staging_dir/toolchain-name-of-your-architecture-something/bin/your-architecture-gcc
export CXX=/route/to/openwrt/folder/staging_dir/toolchain-name-of-your-architecture-something/bin/your-architecture-g++
export LINK=/route/to/openwrt/folder/staging_dir/toolchain-name-of-your-architecture-something/bin/your-architecture-g++
export RANLIB=/route/to/openwrt/folder/staging_dir/toolchain-name-of-your-architecture-something/bin/your-architecture-ranlib
export STAGING_DIR=/route/to/openwrt/folder/staging_dir
export LIBPATH=/route/to/openwrt/folder/staging_dir/toolchain-name-of-your-architecture-something/lib
export LDFLAGS='-Wl,-rpath-link '${LIBPATH}
```

```
export AR=/usr/xiaomi/sdk_package/toolchain/bin/arm-xiaomi-linux-uclibcgnueabi-ar
export CC=/usr/xiaomi/sdk_package/toolchain/bin/arm-xiaomi-linux-uclibcgnueabi-gcc
export CXX=/usr/xiaomi/sdk_package/toolchain/bin/arm-xiaomi-linux-uclibcgnueabi-g++
export LINK=/usr/xiaomi/sdk_package/toolchain/bin/arm-xiaomi-linux-uclibcgnueabi-g++

export RANLIB=/usr/xiaomi/sdk_package/toolchain/bin/arm-xiaomi-linux-uclibcgnueabi-ranlib
export STAGING_DIR=/usr/xiaomi/sdk_package
export LIBPATH=/usr/xiaomi/sdk_package/toolchain/lib
export LDFLAGS='-Wl,-rpath-link '${LIBPATH}
```

### Configuration file & Compile
```
./configure --without-snapshot --without-npm 

make
```

### https://cnodejs.org/topic/536a402afa61d57127060d40
export CFLAGS=-static
export CXXFLAGS=-static
export LDFLAGS=-static

./configure --without-snapshot --prefix=/output




## OpenWrt 编译系统
https://wiki.openwrt.org/zh-cn/doc/howto/buildroot.exigence  

### 安装编译openwrt须要的包：
```
apt-get update
# apt-get install git-core build-essential libssl-dev libncurses5-dev unzip
# apt-get install subversion mercurial

apt-get install libncurses5-dev zlib1g-dev gawk flex patch git-core g++ subversion mercurial
apt-get install bzip2
apt-get install wget
apt-get install openssl ncurses


##
apt-get install gawk zlib1g libncurses5 g++ flex
安装编译openwrt须要的包：
apt-get install libncurses5-dev zlib1g-dev gawk flex patch git-core g++ subversion 


# git clone git://git.openwrt.org/openwrt.git openwrt_dev

git clone git://git.openwrt.org/openwrt.git
make menuconfig
```

### 安装依赖包 OPT-2
https://github.com/wongsyrone/LinuxNotes/blob/master/06.md  

```
// Ubuntu 14.04 必选
apt-get install asciidoc bash bc binutils bzip2 fastjar flex git-core g++ build-essential util-linux gawk libgtk2.0-dev intltool jikespg zlib1g-dev genisoimage libncurses5-dev libssl-dev patch perl-modules python2.7-dev rsync ruby sdcc unzip wget gettext xsltproc libboost1.55-dev libboost1.55-tools-dev libxml-parser-perl libusb-dev bin86 bcc bzr ecj sharutils openjdk-7-jdk zip gcc-multilib quilt libglib2.0-dev gperf
// Ubuntu 14.04 可选
apt-get install subversion mercurial cvs
// ArchLinux 必选
pacman -S base-devel
pacman -S [--needed] asciidoc b43-fwcutter bash bc bin86 boost binutils bzip2 cdrkit fastjar flex gawk gettext git gtk2 intltool jdk7-openjdk libusb libxslt ncurses openssl patch perl python2 rsync ruby sdcc sharutils unzip util-linux wget zlib gcc make perl-extutils-makemaker findutils libstdc++5 lib32-libstdc++5 gperf
// 根据wiki，ArchLinux部分必选包在AUR里面
// 从 2015-12-11 开始，bcc和jikes貌似从AUR里消失了，可能是 C++ ABI 更新的缘故，不安装这两个貌似也可以
$ yaourt -S bcc jikes
// ArchLinux 可选
pacman -S subversion
```