# 入门篇



## 初试容器

### Docker 的诞生

PyCon2013 大会之后，许多人都意识到了容器的价值和重要性，发现它能够解决困扰了云厂商多年的打包、部署、管理、运维等问题，Docker 也就迅速流行起来，成为了 GitHub 上的明星项目。然后在几个月的时间里，Docker 更是吸引了 Amazon、Google、Red Hat 等大公司的关注，这些公司利用自身的技术背景，纷纷在容器概念上大做文章，最终成就了我们今天所看到的至尊王者 Kubernetes 的出现。



### docker安装

安装Docker Engine



### docker的使用



### docker的架构

下图描述了 Docker Engine 的内部角色和工作流程

![img](https://static001.geekbang.org/resource/image/c8/fe/c8116066bdbf295a7c9fc25b87755dfe.jpg?wh=1920x1048)



在客户端通过docker命令，与后台服务Docker daemon通信，而镜像储存在远程仓库里。Docker daemon 负责从远端拉取镜像、在本地存储镜像，还有从镜像生成容器、管理容器等所有功能。

官方提供一个demo

```bash
docker run hello-world
```

它会先检查本地镜像，如果没有就从远程仓库拉取，再运行容器，最后输出运行信息：

![img](https://static001.geekbang.org/resource/image/2b/06/2b1c5561438a7bdb6243dcb450e5c006.png?wh=1920x1014)



## 容器的本质——被隔离的进程

广义上来说，容器技术是动态的容器、静态的镜像和远端的仓库这三者的组合。

### 容器到底是什么

从字面上来看，容器就是 Container，就像是现实中的集装箱，一旦打包完成之后，就可以从一个地方迁移到任意的其他地方。相比散装形式而言，集装箱隔离了箱内箱外两个世界，保持了货物的原始形态，避免了内外部相互干扰，极大地简化了商品的存储、运输、管理等工作。

容器，就是一个特殊的隔离环境，它能够让进程只看到这个环境里的有限信息，不能对外界环境施加影响。



### 为什么要隔离

- 系统安全
  - 使用容器技术，我们就可以让应用程序运行在一个有严密防护的“沙盒”（Sandbox）环境之内。避免无意的 Bug 导致信息泄漏或者其他安全事故。
- 资源分配
  - 容器技术的另一个本领就是为应用程序加上资源隔离，在系统里切分出一部分资源，让它只能使用指定的配额。避免容器内进程过度消耗系统资源。

### 与虚拟机的区别

#### 目的

容器和虚拟机的目的都是隔离资源，保证系统安全，然后是尽量提高资源的利用率。

#### 虚拟化原理

- 虚拟机虚拟化出来的是硬件，需要在上面再安装一个操作系统后才能够运行应用程序，而硬件虚拟化和操作系统都比较“重”。不过好处就是隔离程度非常高，每个虚拟机之间可以做到完全无干扰。
- docker的容器直接利用了下层的计算机硬件和操作系统，因为比虚拟机少了一层，所以自然就会节约 CPU 和内存。因为多个容器共用操作系统内核，应用程序的隔离程度就没有虚拟机那么高了。

#### 运行效率

是容器相比于虚拟机最大的优势。

![img](https://static001.geekbang.org/resource/image/26/6d/26cb446ac5ec53abde2744c431200c6d.jpg?wh=1920x869)



#### 不互相排斥

虚拟机和容器这两种技术也不是互相排斥的，它们完全可以结合起来使用，就像我们的课程里一样，用虚拟机实现与宿主机的强隔离，然后在虚拟机里使用 Docker 容器来快速运行应用程序。



### 隔离是怎么实现的

虚拟机使用的是 Hypervisor（KVM、Xen 等），那么，容器是怎么实现和下层计算机硬件和操作系统交互的呢？为什么它会具有高效轻便的隔离特性呢？

Linux 操作系统内核之中，为资源隔离提供了三种技术：namespace、cgroup、chroot，虽然这三种技术的初衷并不是为了实现容器，但它们三个结合在一起就会发生奇妙的“化学反应”。

- namespace 是 2002 年从 Linux 2.4.19 开始出现的，和编程语言里的 namespace 有点类似，它可以创建出独立的文件系统、主机名、进程号、网络等资源空间，相当于给进程盖了一间小板房，这样就实现了系统全局资源和进程局部资源的隔离。
- cgroup 是 2008 年从 Linux 2.6.24 开始出现的，它的全称是 Linux Control Group，用来实现对进程的 CPU、内存等资源的优先级和配额限制，相当于给进程的小板房加了一个天花板。
- chroot 的历史则要比前面的 namespace、cgroup 要古老得多，早在 1979 年的 UNIX V7 就已经出现了，它可以更改进程的根目录，也就是限制访问文件系统，相当于给进程的小板房铺上了地砖。



## 容器化的应用

容器就是被隔离的进程。

### 什么是容器化的应用

容器技术里的镜像也是同样的道理。因为容器是由操作系统动态创建的，那么必然就可以用一种办法**把它的初始环境给固化下来，保存成一个静态的文件**，相当于是把容器给“拍扁”了，这样就可以非常方便地存放、传输、版本化管理了。

镜像是只读的。

从功能上来看，镜像和常见的 tar、rpm、deb 等安装包一样，都打包了应用程序，但最大的不同点在于它里面**不仅有基本的可执行文件，还有应用运行时的整个系统环境。**这就让镜像具有了非常好的跨平台便携性和兼容性。

所谓的“**容器化的应用**”，或者“应用的容器化”，**就是指应用程序不再直接和操作系统打交道，而是封装成镜像，再交给容器环境去运行。**

镜像就是静态的应用容器，容器就是动态的应用镜像。

### 常用的镜像操作有哪些

![img](https://static001.geekbang.org/resource/image/27/19/27364161a8d3c1f960a91e07b5094419.jpg?wh=1920x963)

docker pull 从远端仓库拉取镜像，docker images 列出当前本地已有的镜像。

镜像的完整名字由两个部分组成，名字和标签，中间用 : 连接起来。有一个比较特殊的标签叫“latest”，它是默认的标签，如果只提供名字没有附带标签，那么就会使用这个默认的“latest”标签。

![img](https://static001.geekbang.org/resource/image/3c/00/3c6e24139acc6d791c189879a7608c00.png?wh=1818x608)

IMAGE ID是镜像唯一的标识。

IMAGE ID 还有一个好处，因为它是十六进制形式且唯一，Docker 特意为它提供了“短路”操作，在本地使用镜像的时候，我们不用像名字那样要完全写出来这一长串数字，通常只需要写出前三位就能够快速定位，在镜像数量比较少的时候用两位甚至一位数字也许就可以了。



docker rmi ，它用来删除不再使用的镜像。



### 常用的容器操作有哪些

![img](https://static001.geekbang.org/resource/image/c8/85/c8cd008e91aaff2cd91e0392b0079085.jpg?wh=1920x1747)

**docker run** 命令把这些静态的应用（镜像）运行起来，变成动态的容器了。

基本的格式是“docker run 设置参数”，再跟上“镜像名或 ID”，后面可能还会有附加的“运行命令”。比如这个命令：

```bash
docker run -h srv alpine hostname
```

几个最常用的参数。

- -it 表示开启一个交互式操作的 Shell，这样可以直接进入容器内部，就好像是登录虚拟机一样。（它实际上是“-i”和“-t”两个参数的组合形式）
- -d 表示让容器在后台运行，这在我们启动 Nginx、Redis 等服务器程序的时候非常有用。
- --name 可以为容器起一个名字，方便我们查看，不过它不是必须的，如果不用这个参数，Docker 会分配一个随机的名字。

**docker ps** 命令来查看容器的运行状态

对于正在运行中的容器，我们可以使用 **docker exec** 命令在里面执行另一个程序。它最常见的用法是使用 -it 参数打开一个 Shell，从而进入容器内部，例如：

```bash
docker exec -it red_srv sh
```

**docker stop** 命令来强制停止容器，可以使用容器名字，也可以用“CONTAINER ID”的前三位数字。

容器被停止后使用 docker ps 命令就看不到了。-a 命令查看系统里所有的容器，当然也包括已经停止运行的容器。

停止运行的容器可以用 **docker start** 再次启动运行，如果你确定不再需要它们，可以使用 **docker rm** 命令来彻底删除，只删除容器不删除镜像。

docker run 命令的时候加上一个 **--rm** 参数，这就会告诉 Docker 不保存容器，只要运行完毕就自动清除，省去了我们手工管理容器的麻烦。



## 创建容器镜像——编写Dockerfile

### 镜像的内部机制是什么

容器镜像内部并不是一个平坦的结构，而是由许多的镜像层组成的，每层都是只读不可修改的一组文件，相同的层可以在镜像之间共享，然后多个层像搭积木一样堆叠起来，再使用一种叫“Union FS 联合文件系统”的技术把它们合并在一起，就形成了容器最终看到的文件系统。

![img](https://static001.geekbang.org/resource/image/c7/3f/c750a7795ff4787c6639dd42bf0a473f.png?wh=800x600)

docker inspect 来查看镜像的分层信息

```bash
docker inspect nginx:alpine
```

它的分层信息在“RootFS”部分：

![img](https://static001.geekbang.org/resource/image/5y/b7/5yybd821a12ec1323f6ea8bb5a5c4ab7.png?wh=1920x592)

通过这张截图就可以看到，nginx:alpine 镜像里一共有 6 个 Layer。

### Dockerfile 是什么

Dockerfile 是一个纯文本，里面记录了一系列的构建指令，比如选择基础镜像、拷贝文件、运行脚本等等，每个指令都会生成一个 Layer，而 Docker 顺序执行这个文件里的所有步骤，最后就会创建出一个新的镜像出来。

来一个简单实例

```bash
# Dockerfile.busybox
FROM busybox                  # 选择基础镜像
CMD echo "hello world"        # 启动容器时默认运行的命令
```

第一条指令是 FROM，所有的 Dockerfile 都要从它开始，表示选择构建使用的基础镜像，相当于“打地基”，这里我们使用的是 busybox。

第二条指令是 CMD，它指定 docker run 启动容器时默认运行的命令，这里我们使用了 echo 命令，输出“hello world”字符串。

docker build 命令来创建出镜像。注意命令的格式，用 -f 参数指定 Dockerfile 文件名，后面必须跟一个文件路径，叫做“构建上下文”（build’s context）。

```bash
docker build -f Dockerfile.busybox .

Sending build context to Docker daemon   7.68kB
Step 1/2 : FROM busybox
 ---> d38589532d97
Step 2/2 : CMD echo "hello world"
 ---> Running in c5a762edd1c8
Removing intermediate container c5a762edd1c8
 ---> b61882f42db7
Successfully built b61882f42db7
```



### 怎样编写正确、高效的 Dockerfile

因为构建镜像的第一条指令必须是 FROM，所以基础镜像的选择非常关键。如果关注的是镜像的安全和大小，那么一般会选择 Alpine；如果关注的是应用的运行稳定性，那么可能会选择 Ubuntu、Debian、CentOS。

```bash
FROM alpine:3.15                # 选择Alpine镜像
FROM ubuntu:bionic              # 选择Ubuntu镜像
```



可以使用 COPY 命令，它的用法和 Linux 的 cp 差不多，不过拷贝的源文件必须是“构建上下文”路径里的，不能随意指定文件。也就是说，如果要从本机向镜像拷贝文件，就必须把这些文件放到一个专门的目录，然后在 docker build 里指定“构建上下文”到这个目录才行。

```bash
COPY ./a.txt  /tmp/a.txt    # 把构建上下文里的a.txt拷贝到镜像的/tmp目录
COPY /etc/hosts  /tmp       # 错误！不能使用构建上下文之外的文件
```



RUN ，可以执行任意的 Shell 命令。通常会是 Dockerfile 里最复杂的指令，会包含很多的 Shell 命令，但 Dockerfile 里一条指令只能是一行，所以有的 RUN 指令会在每行的末尾使用续行符 \，命令之间也会用 && 来连接，这样保证在逻辑上是一行。

```bash
RUN apt-get update \
    && apt-get install -y \
        build-essential \
        curl \
        make \
        unzip \
    && cd /tmp \
    && curl -fSL xxx.tar.gz -o xxx.tar.gz\
    && tar xzf xxx.tar.gz \
    && cd xxx \
    && ./config \
    && make \
    && make clean
```



可以把这些 Shell 命令集中到一个脚本文件里，用 COPY 命令拷贝进去再用 RUN 来执行。

```bash
COPY setup.sh  /tmp/                # 拷贝脚本到/tmp目录

RUN cd /tmp && chmod +x setup.sh \  # 添加执行权限
    && ./setup.sh && rm setup.sh    # 运行脚本然后再删除
```



Dockerfile可以通过ARG和ENV创建变量。ARG 创建的变量只在镜像构建过程中可见，容器运行时不可见，而 ENV 创建的变量不仅能够在构建镜像的过程中使用，在容器运行时也能够以环境变量的形式被应用程序使用。

```bash
ARG IMAGE_BASE="node"
ARG IMAGE_TAG="alpine"

ENV PATH=$PATH:/tmp
ENV DEBUG=OFF
```

EXPOSE，它用来声明容器对外服务的端口号，对现在基于 Node.js、Tomcat、Nginx、Go 等开发的微服务系统来说非常有用：

```bash
EXPOSE 443           # 默认是tcp协议
EXPOSE 53/udp        # 可以指定udp协议
```



**每个指令都会生成一个镜像层，尽量精简合并，否则太多的层会导致镜像臃肿不堪。**



### docker build 是怎么工作的

Dockerfile 必须要经过 docker build才能生效，其中的构建上下文指定的路径作为打包镜像时依赖的文件，可以通过在路径中建立一个 .dockerignore 文件来忽略不需要的文件。

.dockerignore的例子

```bash
# docker ignore
*.swp
*.sh
```

docker build 通过-f指定文件，如果不指定则默认处理名字是 Dockerfile 的文件。

docker build 通过-t 参数指定生成镜像的名称。名字应该符合规范，用:分割名称和tag，如果没有tag，默认为latest



## 镜像仓库Docker Hub

