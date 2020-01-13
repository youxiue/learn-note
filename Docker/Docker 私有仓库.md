# Docker私有化仓库

## 1. 搭建与配置

1. 拉取私有仓库镜像

   ```shell
   docker pull registry
   ```

2. 启动私有仓库镜像

   ```shell
   docker run -di --name=registry -p 5000:5000 registry
   ```

3. 打开浏览器输入地址http://192.168.134.8:5000/v2/_catalog看到`{“repositories”:[]}`表示私有仓库搭建成功并且内容为空

4. 修改daemon.json

   ```shell
   vi /etc/docker/daemon.json
   ```

   添加以下内容，保存并退出。

   ```java
   {"insecure-registries":["192.168.134.8:5000"]}
   ```

   此步让docker信任私有仓库地址

5. 重启docker服务

   ```shell
   systemctl restart docker
   ```
   
   

## 2. 镜像上传至私有仓库

1. 标记此镜像为私有仓库的镜像

   ```shell
   docker tag jdk1.8 192.168.134.8:5000/jdk1.8
   ```

2. 上传标记的镜像

   ```shell
   docker push 192.168.134.8:5000/jdk1.8
   ```

   

## 3.构建jdk1.8镜像

Dockerfile文件

```shell
FROM centos
MAINTAINER youxiue
WORKDIR /usr
RUN mkdir /usr/local/java
ADD jdk-8u161-linux-x64.tar.gz /usr/local/java/

ENV JAVA_HOME /usr/local/java/jdk1.8.0_161
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/bin/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
```

构建命令

```shell
docker build -t jdk1.8 .
```

