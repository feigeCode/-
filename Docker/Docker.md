# 1、在centos7上安装docker

## 1.1、如果存在则先卸载

~~~shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
~~~

## 1.2、安装docker需要的一些工具包

~~~shell
yum install -y yum-utils

yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo #国外的镜像慢，可以换成阿里云的
~~~

## 1.3、安装最新docker engine和容器

~~~shell
yum install docker-ce docker-ce-cli containerd.io
~~~

### 安装指定版本

~~~shell
yum list docker-ce --showduplicates | sort -r #找出指定版本


docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
~~~

~~~shell
yum install docker-ce-版本号 docker-ce-cli-版本号 containerd.io #docker-ce-18.09.1
~~~

## 1.4、启动docker

~~~shell
systemctl start docker
~~~



## 1.5、运行`hello-world` 映像来验证是否正确安装了Docker Engine 。

~~~shell
docker run hello-world
# 效果
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:6a65f928fb91fcfbc963f7aa6d57c8eeb426ad9a20c7ee045538ef34847f44f1
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
~~~



# 2、docker常用命令

## 2.1、运行镜像

~~~shell
docker run -it centos /bin/bash

docker run -itd --name test-centos centos /bin/bash

docker run tomcat -itd --name tomcat1 -p 8000:8080 tomcat

docker run tomcat -itd --name tomcat2 -P tomcat
~~~

参数说明

- **-i**: 交互式操作。
- **-t**: 终端。
- **/bin/bash**：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。
- **-d：** 后台运行，不会进入容器
- **--name：**为镜像取一个名字
- **-P:**随机端口映射
- **-p:**指定端口映射
- **-v:**挂载数据卷

~~~shell
exit #退出容器
~~~

## 2.2、查找和拉取镜像

~~~shell
docker search 镜像名
docker pull 镜像名:版本号默认最新
~~~

## 2.4、查看镜像

~~~shell
docker images -aq
~~~

参数说明

- **-a:** 列出所有镜像
- **-q:** 列出所有镜像的id

## 2.4、移除镜像和移除容器

~~~shell
docker rmi -f 镜像名或镜像ID

docker rmi -f $(docker images -ap) #删除所有镜像
~~~

~~~docker
docker rm -f 镜像名或镜像ID

docker rm -f $(docker images -ap) #删除所有容器
~~~

参数说明

- **-f** 强制删除

## 2.5、停止、启动和重启一个容器

~~~shell
docker stop 容器ID #停止
~~~

~~~shell
docker start 容器ID #启动
~~~

~~~shell
docker restart 容器ID #重启
~~~

## 2.6、查看正在运行的容器

~~~shell
docker ps
~~~

参数说明

- **-a:** 列出所有正在运行的容器
- **-q:** 列出所有正在运行的容器的id

## 2.7、进入容器

在使用 **-d** 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：

- **docker attach**
- **docker exec**：退出容器终端，不会导致容器的停止。

![image-20200518231603154](.\Docker.assets\image-20200518231603154.png)



![image-20200518231943175](.\Docker.assets\image-20200518231943175.png)



## 2.8、查看 WEB 应用程序日志

~~~shell
docker logs -f [ID或者名字] #可以查看容器内部的标准输出。
~~~



## 2.9、检查 WEB 应用程序

~~~shell
docker inspect [ID或者名字] #查看 Docker 的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息
~~~

## docker命令大全

~~~shell
容器生命周期管理
run
start/stop/restart
kill
rm
pause/unpause
create
exec
容器操作
ps
inspect
top
attach
events
logs
wait
export
port
容器rootfs命令
commit
cp
diff
镜像仓库
login
pull
push
search
本地镜像管理
images
rmi
tag
build
history
save
load
import
info|version
info
version
~~~



# 3、实战

## 3.1、安装Tomcat

~~~shell
[root@VM_0_2_centos ~]# docker run -itd --name tomcat03 -p 8000:8080  tomcat
38a890f9e79d87056ada1513117ff544ebeb048b0b7651c100339fdee3389b30
[root@VM_0_2_centos ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
38a890f9e79d        tomcat              "catalina.sh run"   16 seconds ago      Up 15 seconds       0.0.0.0:8000->8080/tcp   tomcat03
[root@VM_0_2_centos ~]# docker exec 38a890f9e79d /bin/bash
[root@VM_0_2_centos ~]# docker exec -it 38a890f9e79d /bin/bash
root@38a890f9e79d:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
root@38a890f9e79d:/usr/local/tomcat# cd webapps
root@38a890f9e79d:/usr/local/tomcat/webapps# ls #是瘦身版的没有webapps
root@38a890f9e79d:/usr/local/tomcat/webapps# cd ..
root@38a890f9e79d:/usr/local/tomcat# cd webapps.dist/
root@38a890f9e79d:/usr/local/tomcat/webapps.dist# cd ..
root@38a890f9e79d:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@38a890f9e79d:/usr/local/tomcat# cd webapps
root@38a890f9e79d:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
~~~

~~~shell
# 保存容器的状态
docker commit -a="作者" -m="提交的信息" 容器ID  容器名字:[tag](版本) #提交一个容器成为一个新的副部
docker commit  -a="feige" -m="add webapps" 38a890f9e79d tomcat02:2.0.0
~~~



## 3.2、安装Nginx

~~~shell
[root@VM_0_2_centos /]# docker run -itd --name nginx01 -p 8000:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
afb6ec6fdc1c: Pull complete 
b90c53a0b692: Pull complete 
11fa52a0fdc0: Pull complete 
Digest: sha256:30dfa439718a17baafefadf16c5e7c9d0a1cde97b4fd84f63b69e13513be7097
Status: Downloaded newer image for nginx:latest
1e4daf08a806e7b54f32d01d1c5faa20daa397acdb6ea04284dc785b4b4aaf99
[root@VM_0_2_centos /]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
1e4daf08a806        nginx               "nginx -g 'daemon of…"   7 seconds ago       Up 4 seconds        0.0.0.0:8000->80/tcp   nginx01
~~~

## 3.3、安装Elasticsearch

~~~shell
[root@VM_0_2_centos /]# docker run -d --name elasticsearch01 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
# elasticsearch启动非常耗内存，我们要限制它的内存-e ES_JAVA_OPTS="-Xms64m -Xmx512m"
Unable to find image 'elasticsearch:7.6.2' locally
7.6.2: Pulling from library/elasticsearch
ab5ef0e58194: Pull complete 
c4d1ca5c8a25: Pull complete 
941a3cc8e7b8: Pull complete 
43ec483d9618: Pull complete 
c486fd200684: Pull complete 
1b960df074b2: Pull complete 
1719d48d6823: Pull complete 
Digest: sha256:1b09dbd93085a1e7bca34830e77d2981521a7210e11f11eda997add1c12711fa
Status: Downloaded newer image for elasticsearch:7.6.2
4f76f9524686ef312079d201798d3b2428484eb08c557398adc3d2d4feb6f5f7
~~~



# 4、Docker 镜像原理

一、镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境的开发软件，它包含运行某个软件所需的所有内容，

包括代码、运行时、库、环境变量和配置文件。

二、UnionFS(联合文件系统)

　　Union文件系统(UnionFS) 是一种分层、轻量级并且高性能的文件系统，他支持对文件系统的修改作为一次提交来层层的叠加，

同时可以将不同目录挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）。Union文件系统是Docker镜像

的基础。镜像可以通过分层来进行集成，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

　　特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统你那个，联合加载会把各层文件系统叠加起来，这样最终的

文件系统会包含所有底层文件和目录。

 

三、Docker 镜像加载原理

　　docker 的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system) 主要包含bootloader和kernel，bootloader 主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就存在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

　　roorfs （root file system），在bootfs之上。包含的就是典型Linux系统中的 /dev ，/proc，/bin ，/etx 等标准的目录和文件。rootfs就是各种不同的操作系统发行版。比如Ubuntu，Centos等等。

　　对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host（宿主机）的kernel，自己只需要提供rootfs就行了，由此可见对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

 

 四、Docker 镜像联合文件系统分层，Tomcat镜像示例

　　![img](.\Docker.assets\1185883-20190705114018923-574032187.png)

　　　采用这种分层结构最大的一个好处就是共享资源，比如有多个镜像都从相同的base镜像构建而来，那么宿主机只需要在磁盘上保存一份base镜像，

　　同时内存中也只需要加载一份base镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。 

　　  docker 镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作 “容器层” ，“容器层” 之下的都叫镜像层。

# 5、容器数据卷

**Docker容器产生的数据，同步到本地，将容器中的目录挂载到Linux本地。实现数据的共享**

## 指定路径挂载

~~~shell
 # docker run -it -v 本地目录:容器内需要映射到本地的目录 镜像名

[root@VM_0_2_centos home]# docker run -it -v /home/test:/home centos
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
8a29a15cefae: Pull complete 
Digest: sha256:fe8d824220415eed5477b63addf40fb06c3b049404242b31982106ac204f6700
Status: Downloaded newer image for centos:latest
[root@f00c33058ac1 /]# /bin/bash
[root@f00c33058ac1 /]# cd /home
[root@f00c33058ac1 home]# touch main.go
[root@f00c33058ac1 home]# cat main.go
~~~

本地目录

~~~shell
[root@VM_0_2_centos ~]# cd /home/test
[root@VM_0_2_centos test]# ll
total 0
-rw-r--r-- 1 root root 0 May 20 15:25 main.go
~~~

~~~shell
docker inspect f00c33058ac1 # 查看该容器的底层信息
~~~

## 安装mysql

~~~~shell
[root@VM_0_2_centos home]# docker run -it --name mysql -p 3344:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=hufei mysql:5.7
Unable to find image 'mysql:5.7' locally
5.7: Pulling from library/mysql
afb6ec6fdc1c: Already exists 
0bdc5971ba40: Pull complete 
97ae94a2c729: Pull complete 
f777521d340e: Pull complete 
1393ff7fc871: Pull complete 
a499b89994d9: Pull complete 
7ebe8eefbafe: Pull complete 
4eec965ae405: Pull complete 
a531a782d709: Pull complete 
10e94c02b508: Pull complete 
799a94b968ef: Pull complete 
Digest: sha256:5c9fd7949bc0f076429fa2c40d0e7406e095bdb5216a923257b31972a6f3ae4
~~~~



![image-20200520154651460](.\Docker.assets\image-20200520154651460.png)

## 具名和匿名挂载

### 具名挂载

~~~shell
[root@VM_0_2_centos _data]# docker run -d --name nginx03 -P -v juming_nginx:/etc/nginx nginx
acae22abda71165610b59665d42d04ae5be6040a22a4ddab9b4200d6e4729d26
[root@VM_0_2_centos _data]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
acae22abda71        nginx               "nginx -g 'daemon of…"   6 seconds ago       Up 4 seconds        0.0.0.0:32770->80/tcp   nginx03
[root@VM_0_2_centos _data]# docker volume ls
DRIVER              VOLUME NAME
local               fbb38b6991355a0f13d3b9e2aef453ac4d57a54754f8c5aa35a9a28d5f22608e
local               juming_nginx
[root@VM_0_2_centos _data]# docker volume inspect juming_nginx
[
    {
        "CreatedAt": "2020-05-20T16:27:31+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming_nginx/_data",
        "Name": "juming_nginx",
        "Options": null,
        "Scope": "local"
    }
]
[root@VM_0_2_centos _data]# cd /var/lib/docker/volumes/juming_nginx/_data/
[root@VM_0_2_centos _data]# ll
total 40
drwxr-xr-x 2 root root 4096 May 20 16:27 conf.d
-rw-r--r-- 1 root root 1007 Apr 14 22:19 fastcgi_params
-rw-r--r-- 1 root root 2837 Apr 14 22:19 koi-utf
-rw-r--r-- 1 root root 2223 Apr 14 22:19 koi-win
-rw-r--r-- 1 root root 5231 Apr 14 22:19 mime.types
lrwxrwxrwx 1 root root   22 Apr 14 22:34 modules -> /usr/lib/nginx/modules
-rw-r--r-- 1 root root  643 Apr 14 22:34 nginx.conf
-rw-r--r-- 1 root root  636 Apr 14 22:19 scgi_params
-rw-r--r-- 1 root root  664 Apr 14 22:19 uwsgi_params
-rw-r--r-- 1 root root 3610 Apr 14 22:19 win-utf
~~~



### 匿名挂载

~~~shell
[root@VM_0_2_centos home]# docker run -d -P --name  nginx02 -v /etc/nginx nginx
de68feff85a1af19929a76f6fbb8d448c27d3cf91f3ca9d6b45bda415cfa8aed
[root@VM_0_2_centos home]# docker volume ls
DRIVER              VOLUME NAME
local               fbb38b6991355a0f13d3b9e2aef453ac4d57a54754f8c5aa35a9a28d5f22608e
[root@VM_0_2_centos home]# docker volume inspect fbb38b6991355a0f13d3b9e2aef453ac4d57a54754f8c5aa35a9a28d5f22608e
[
    {
        "CreatedAt": "2020-05-20T16:21:18+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/fbb38b6991355a0f13d3b9e2aef453ac4d57a54754f8c5aa35a9a28d5f22608e/_data",
        "Name": "fbb38b6991355a0f13d3b9e2aef453ac4d57a54754f8c5aa35a9a28d5f22608e",
        "Options": null,
        "Scope": "local"
    }
]
[root@VM_0_2_centos home]# cd /var/lib/docker/volumes/fbb38b6991355a0f13d3b9e2aef453ac4d57a54754f8c5aa35a9a28d5f22608e/_data
[root@VM_0_2_centos _data]# ll
total 40
drwxr-xr-x 2 root root 4096 May 20 16:21 conf.d
-rw-r--r-- 1 root root 1007 Apr 14 22:19 fastcgi_params
-rw-r--r-- 1 root root 2837 Apr 14 22:19 koi-utf
-rw-r--r-- 1 root root 2223 Apr 14 22:19 koi-win
-rw-r--r-- 1 root root 5231 Apr 14 22:19 mime.types
lrwxrwxrwx 1 root root   22 Apr 14 22:34 modules -> /usr/lib/nginx/modules
-rw-r--r-- 1 root root  643 Apr 14 22:34 nginx.conf
-rw-r--r-- 1 root root  636 Apr 14 22:19 scgi_params
-rw-r--r-- 1 root root  664 Apr 14 22:19 uwsgi_params
-rw-r--r-- 1 root root 3610 Apr 14 22:19 win-utf
~~~

**所有的docker容器内的卷，没有指定目录的情况下都是在`/var/lib/docker/volumes/xxxxx/_data`**

## 读写权限

~~~shell
docker run -d -P --name  nginx02 -v /etc/nginx:ro nginx #只读，只可以从外部来改变
docker run -d -P --name  nginx02 -v /etc/nginx:rw nginx #可读可写
~~~

## 通过Dockerfile挂载

~~~dockerfile
FROM centos

VOLUME ["/home"] #可以写多个，匿名挂载，默认目录/var/lib/docker/volumes/xxxxx/_data

CMD echo "-----success-----"

CMD /bin/bash
~~~

**每一条命令就是一层**

~~~shell
[root@VM_0_2_centos dockerfile]# vim dockerfile
[root@VM_0_2_centos dockerfile]# cat dockerfile 
FROM centos

VOLUME ["/home"]

CMD echo "-----success-----"

CMD /bin/bash
[root@VM_0_2_centos dockerfile]# vim dockerfile
[root@VM_0_2_centos dockerfile]# docker build -f dockerfile -t contos01:1.0 .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 470671670cac
Step 2/4 : VOLUME ["/home"]
 ---> Running in 2668cf2a8044
Removing intermediate container 2668cf2a8044
 ---> 8850b606ffd1
Step 3/4 : CMD echo "-----success-----"
 ---> Running in d699380f2f75
Removing intermediate container d699380f2f75
 ---> bac5a4f7ff61
Step 4/4 : CMD /bin/bash
 ---> Running in 1217c0115f9e
Removing intermediate container 1217c0115f9e
 ---> b2522b049920
Successfully built b2522b049920
Successfully tagged contos01:1.0
[root@VM_0_2_centos dockerfile]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
contos01            1.0                 b2522b049920        11 seconds ago      237MB
f5334b0b4cdc        1.0                 f5334b0b4cdc        5 hours ago         652MB
tomcat              latest              1b6b1fe7261e        3 days ago          647MB
nginx               latest              9beeba249f3e        4 days ago          127MB
mysql               5.7                 b84d68d0a7db        4 days ago          448MB
elasticsearch       7.6.2               f29a1ee41030        7 weeks ago         791MB
centos              latest              470671670cac        4 months ago        237MB
[root@VM_0_2_centos dockerfile]# docker run -it contos01:1.0
[root@5c1dd23745e2 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@5c1dd23745e2 /]# cd home
[root@5c1dd23745e2 home]# touch main.go
[root@5c1dd23745e2 home]# ls
main.go 
~~~

**本地目录**

~~~shell
[root@VM_0_2_centos test]# cd /var/lib/docker/volumes/96874784a49826d917fa11f11cde2b0304ce97f1604eeac349359b1123b95822/_data
[root@VM_0_2_centos _data]# ll
total 0
[root@VM_0_2_centos _data]# ls
main.go
~~~

## **继承**

~~~~shell
# docker run -it --name centos02 --volumes-from 要继承的容器ID 要启动的容器 
docker run -it --name centos02 --volumes-from dcb3333fbaa9 contos01:1.0 #实现两个容器数据的共享
~~~~

# 6、Dockerfile

![图片](.\Docker.assets\u=4047443406,3367621745&fm=11&gp=0.jpg)



![image-20200520163628045](.\Docker.assets\image-20200520163628045.png)



~~~dockerfile
FROM        #基础镜像，一切从这里开始
MAINTAINER  #镜像是谁写的
RUN         #镜像构建的时候需要运行的命令
ADD         #添加内容（直接用压缩包，会自动解压）
WORKDIR     #工作目录
VOLUME      #挂载目录
EXPOSE      #暴露端口配置
CMD         #指定这个容器启动的时候需要运行的命令，只有最后一个会生效，可被代替
ENTRYPOINT  #指定这个容器启动的时候需要运行的命令,可以追加
ONBUILD     #当构建一个被继承DockerFile这个时候就会运行
COPY        #类似ADD将我们的文件拷贝到镜像中
ENV         #构建的时候设置环境变量
~~~



## 实战

### 构建自己的centos

~~~~dockerfile
FROM centos

MAINTAINER feige

ENV MYPATH /usr/local

WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "----end---"
CMD /bin/bash
~~~~

~~~shell
docker build -f dockerfile -t self_centos:1.0 . #构建镜像
~~~

### 打包springboot

~~~dockerfile
FROM java:8
COPY *.jar /app.jar # 取别名
CMD ["--server.port=8080"]
EXPOSE 8080

ENTRYPOINT ["java","-jar","/app.jar"]
~~~

~~~shell
# 把jar包和Dockerfile（官方推荐命名）放在一个目录
docker build -t spingboot01:1.0 . #构建镜像
~~~

# 7、docker网络

## 理解docker0

~~~~shell
[root@VM_0_2_centos ~]# docker run -d -P --name tomcat01 tomcat
Unable to find image 'tomcat:latest' locally
latest: Pulling from library/tomcat
376057ac6fa1: Pull complete 
5a63a0a859d8: Pull complete 
496548a8c952: Pull complete 
2adae3950d4d: Pull complete 
0a297eafb9ac: Pull complete 
09a4142c5c9d: Pull complete 
9e78d9befa39: Pull complete 
18f492f90b9c: Pull complete 
7834493ec6cd: Pull complete 
216b2be21722: Pull complete 
Digest: sha256:ce753be7b61d86f877fe5065eb20c23491f783f283f25f6914ba769fee57886b
Status: Downloaded newer image for tomcat:latest
356e82c7db7355cdc5b86999b2234c42cdc3db29565ae090ffd5bf97ecb88d6c
[root@VM_0_2_centos ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
356e82c7db73        tomcat              "catalina.sh run"   37 seconds ago      Up 35 seconds       0.0.0.0:32771->8080/tcp   tomcat01
[root@VM_0_2_centos ~]# docker exec -it 356e82c7db73 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
64: eth0@if65: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever
[root@VM_0_2_centos ~]# ping  172.18.0.2
PING 172.18.0.2 (172.18.0.2) 56(84) bytes of data.
64 bytes from 172.18.0.2: icmp_seq=1 ttl=64 time=0.235 ms
64 bytes from 172.18.0.2: icmp_seq=2 ttl=64 time=0.082 ms
# linux可以ping通docker容器
~~~~

> 原理

**1、 我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要安装了docker，就会有一个网卡docker0桥接模式，使用的技术是veth-pair技术！**

![image-20200521135224103](.\Docker.assets\image-20200521135224103.png)

重新启动一个tomcat

![image-20200521135309437](.\Docker.assets\image-20200521135309437.png)

我们发现这两个容器带来的网卡，都是一对一对的

veth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此相连

正因为有这个特性，evth-pair充当一个桥梁，连接各种虚拟网络设备的

OpenStac，docker容器之间的连接，ovs的连接，都是使用evth-pair技术

**2、tomcat02能ping通tomcat01**

![image-20200521140147018](.\Docker.assets\image-20200521140147018.png)

结论：tomcat01和tomcat02是公用的一个路由器。

所有容器不指定网络的情况下，都是docker0路由的，docker会给我们的容器非配一个默认的可用ip

![image-20200521142641328](.\Docker.assets\image-20200521142641328.png)



docker中的所有的网络接口都是虚拟的，虚拟的转发效率高！（内网传递文件！）

只要容器删除，对应的网桥一对就没了



## --link

~~~shell
# 无法ping通
[root@VM_0_2_centos ~]# docker exec -it tomcat02 ping tomcat01
ping: tomcat01: Name or service not known
~~~



~~~shell
# 通过--link就可以解决网络连通
[root@VM_0_2_centos ~]# docker run -d -P --name tomcat03 --link tomcat02 tomcat
8716ce830d9715ef4e7ebae9d7dae04e840e0f13f8026cd6e461f9e32bf17c3d
[root@VM_0_2_centos ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
8716ce830d97        tomcat              "catalina.sh run"   5 seconds ago       Up 3 seconds        0.0.0.0:32773->8080/tcp   tomcat03
ae9147a92d90        tomcat              "catalina.sh run"   30 minutes ago      Up 30 minutes       0.0.0.0:32772->8080/tcp   tomcat02
356e82c7db73        tomcat              "catalina.sh run"   39 minutes ago      Up 39 minutes       0.0.0.0:32771->8080/tcp   tomcat01
[root@VM_0_2_centos ~]# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.18.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.18.0.3): icmp_seq=1 ttl=64 time=0.277 ms
64 bytes from tomcat02 (172.18.0.3): icmp_seq=2 ttl=64 time=0.082 ms
~~~



~~~shell
# 反向不能ping通
[root@VM_0_2_centos ~]# docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
~~~

**inspect探究一下**

![image-20200521143037846](.\Docker.assets\image-20200521143037846.png)

其实这个tomcat03就是本地配置了tomcat02

~~~shell
# 查看hosts 配置，在这里发现原理
[root@VM_0_2_centos ~]# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.18.0.3	tomcat02 ae9147a92d90
172.18.0.4	8716ce830d97
~~~

--link 就是在hosts中配置增加了一个172.18.0.3	tomcat02 ae9147a92d90

现在已经不建议使用--link了

## 自定义网络

~~~shell
# 查看所有的docker网络
[root@VM_0_2_centos ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
a4a45b318b73        bridge              bridge              local
a312db629304        host                host                local
cedc582ebf9f        none                null                local
~~~

**网络模式**

bridge：桥接 docker（默认）

none：不配置网络

host：和宿主机共享网络

container：容器网络连通！（用的少，局限很大）

**测试**

~~~shell
# 我们直接启动的命令 --net bridge，而这个就是我们的docker01
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat

# docker0特点，默认，域名不能访问，--link可以打通连接
~~~

~~~shell
# 自定义一个网络
# --driver bridge
# --subnet 192.168.0.0/16 
# --gateway 192.168.0.1
[root@VM_0_2_centos ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
ed277c37b9b41464028da0175bab0359dcc1a727b7f98e473fa3224c84fbd36a
[root@VM_0_2_centos ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
a4a45b318b73        bridge              bridge              local
a312db629304        host                host                local
ed277c37b9b4        mynet               bridge              local
cedc582ebf9f        none                null                local
~~~

自己的网络就创建好了

![image-20200521144912812](.\Docker.assets\image-20200521144912812.png)

~~~shell
[root@VM_0_2_centos ~]# docker run -d -P --name tomcat01 --net mynet tomcat
248ab38bea248fcd85832c0c339775acb1b710bf5a5f22e245402146c08d271a
[root@VM_0_2_centos ~]# docker run -d -P --name tomcat02 --net mynet tomcat
7a4c5ddc3619a60dbf823d502f7d2a140146ea6983a91399f93289108ef19712
d[root@VM_0_2_centos ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
7a4c5ddc3619        tomcat              "catalina.sh run"   7 seconds ago       Up 5 seconds        0.0.0.0:32775->8080/tcp   tomcat02
248ab38bea24        tomcat              "catalina.sh run"   15 seconds ago      Up 13 seconds       0.0.0.0:32774->8080/tcp   tomcat01
# 现在可以ping通了
[root@VM_0_2_centos ~]# docker exec -it tomcat01 ping tomcat02
PING tomcat02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.111 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.082 ms
# 反向也可以ping通了
[root@VM_0_2_centos ~]# docker exec -it tomcat02 ping tomcat01
PING tomcat01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.070 ms
64 bytes from tomcat01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.078 ms

~~~

我们自定义的的网络docker都已经帮我们维护好了对应的关系，推荐我们平时这样使用网络

好处：

​	redis-不同的集群使用不同的网络，保证集群都是安全和健康的

​	mysql-不同的集群使用不同的网络，保证集群都是安全和健康的

## 网络连通

~~~shell
[root@VM_0_2_centos ~]# docker run -d -P --name tomcat03  tomcat
43c5d0b0fb0874635a39c842012ce6db99a4d36e5b57e8eb766f47b81e9a1d78
# docker0和mynet是不能连通的
[root@VM_0_2_centos ~]# docker exec -it tomcat03 ping tomcat01
ping: tomcat01: Name or service not known
# 网络和容器连接
[root@VM_0_2_centos ~]# docker network connect mynet tomcat03
# 现在可以ping通了
[root@VM_0_2_centos ~]# docker exec -it tomcat03 ping tomcat01
PING tomcat01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.131 ms
64 bytes from tomcat01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.074 ms
64 bytes from tomcat01.mynet (192.168.0.2): icmp_seq=3 ttl=64 time=0.121 ms

~~~

连通之后就是将tomcat03放到了mynet的网络下

一个容器两个ip地址

阿里云服务，公网ip  私网ip都能访问

~~~shell
docker network inspect mynet
~~~

![image-20200521150619991](.\Docker.assets\image-20200521150619991.png)

## 实战：部署redis集群

分片+高可用+负载均衡

~~~shell
for port in $(seq 1 6); do mkdir -p /mydata/redis/node-${port}/conf; touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
 done

# 通过脚本启动
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

#单独启动
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6372:6379 -p 16372:16379 --name redis-2 \
-v /mydata/redis/node-2/data:/data \
-v /mydata/redis/node-2/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.12 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6373:6379 -p 16373:16379 --name redis-3 \
-v /mydata/redis/node-3/data:/data \
-v /mydata/redis/node-3/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.13 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf


docker run -p 6374:6379 -p 16374:16379 --name redis-4 \
-v /mydata/redis/node-4/data:/data \
-v /mydata/redis/node-4/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.14 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6375:6379 -p 16375:16379 --name redis-5 \
-v /mydata/redis/node-5/data:/data \
-v /mydata/redis/node-5/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.15 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

docker run -p 6376:6379 -p 16376:16379 --name redis-6 \
-v /mydata/redis/node-6/data:/data \
-v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.16 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

# 创建集群
[root@VM_0_2_centos ~]# docker exec -it redis-1 /bin/sh
/data # ls
appendonly.aof  nodes.conf
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: 5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: 4c467610fb5552e91aa9bb045dc2805c9bf0fe7e 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 903e6fbe6546d1d8775fdcc71866522db4420f29 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: d26c60a4db2278a9e464b3c5120ef55b158f75f2 172.38.0.14:6379
   replicates 903e6fbe6546d1d8775fdcc71866522db4420f29
S: 544016953c930fabeef5b5a245b0e143d3f3b106 172.38.0.15:6379
   replicates 5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b
S: 4550790ef9da7dcefc03a79e1ec8788d418e34e6 172.38.0.16:6379
   replicates 4c467610fb5552e91aa9bb045dc2805c9bf0fe7e
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
....
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: 5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: d26c60a4db2278a9e464b3c5120ef55b158f75f2 172.38.0.14:6379
   slots: (0 slots) slave
   replicates 903e6fbe6546d1d8775fdcc71866522db4420f29
M: 903e6fbe6546d1d8775fdcc71866522db4420f29 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
M: 4c467610fb5552e91aa9bb045dc2805c9bf0fe7e 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 544016953c930fabeef5b5a245b0e143d3f3b106 172.38.0.15:6379
   slots: (0 slots) slave
   replicates 5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b
S: 4550790ef9da7dcefc03a79e1ec8788d418e34e6 172.38.0.16:6379
   slots: (0 slots) slave
   replicates 4c467610fb5552e91aa9bb045dc2805c9bf0fe7e
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
# redis集群搭建成功
/data # redis-cli -c
127.0.0.1:6379> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:140
cluster_stats_messages_pong_sent:137
cluster_stats_messages_sent:277
cluster_stats_messages_ping_received:132
cluster_stats_messages_pong_received:140
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:277
127.0.0.1:6379> cluster nodes
d26c60a4db2278a9e464b3c5120ef55b158f75f2 172.38.0.14:6379@16379 slave 903e6fbe6546d1d8775fdcc71866522db4420f29 0 1590047463592 4 connected
903e6fbe6546d1d8775fdcc71866522db4420f29 172.38.0.13:6379@16379 master - 0 1590047464719 3 connected 10923-16383
4c467610fb5552e91aa9bb045dc2805c9bf0fe7e 172.38.0.12:6379@16379 master - 0 1590047464309 2 connected 5461-10922
544016953c930fabeef5b5a245b0e143d3f3b106 172.38.0.15:6379@16379 slave 5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b 0 1590047463269 5 connected
4550790ef9da7dcefc03a79e1ec8788d418e34e6 172.38.0.16:6379@16379 slave 4c467610fb5552e91aa9bb045dc2805c9bf0fe7e 0 1590047463592 6 connected
5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b 172.38.0.11:6379@16379 myself,master - 0 1590047463000 1 connected 0-5460
127.0.0.1:6379> set a b
-> Redirected to slot [15495] located at 172.38.0.13:6379
OK
# 我们把redis-3停了
172.38.0.13:6379> get a
^C
/data # redis-cli -c
127.0.0.1:6379> get a
-> Redirected to slot [15495] located at 172.38.0.14:6379
"b"
172.38.0.14:6379> cluster nodes
903e6fbe6546d1d8775fdcc71866522db4420f29 172.38.0.13:6379@16379 master,fail - 1590047565579 1590047564174 3 connected
d26c60a4db2278a9e464b3c5120ef55b158f75f2 172.38.0.14:6379@16379 myself,master - 0 1590047628000 7 connected 10923-16383
4c467610fb5552e91aa9bb045dc2805c9bf0fe7e 172.38.0.12:6379@16379 master - 0 1590047630000 2 connected 5461-10922
544016953c930fabeef5b5a245b0e143d3f3b106 172.38.0.15:6379@16379 slave 5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b 0 1590047631265 5 connected
4550790ef9da7dcefc03a79e1ec8788d418e34e6 172.38.0.16:6379@16379 slave 4c467610fb5552e91aa9bb045dc2805c9bf0fe7e 0 1590047629551 6 connected
5e6bb32ddee37719c6fcc0dbfac6a6bf69f3dc3b 172.38.0.11:6379@16379 master - 0 1590047630000 1 connected 0-5460
~~~

故障转移

![image-20200521155500672](.\Docker.assets\image-20200521155500672.png)