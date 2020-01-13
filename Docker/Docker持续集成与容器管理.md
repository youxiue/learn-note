# 持续集成与容器管理

学习目标：

- 掌握DockerMaven插件的使用
- 掌握持续化集成工具Jenkins的使用
- 掌握容器管理用户Rancher的使用

## 1.DockerMaven插件

Maven插件自动部署步骤：

1. 修改宿主机的docker配置，让其可以远程访问

   ```java
   vi /lib/systemd/system/docker.service
   ```

   其中`ExecStart=`后添加配置`-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock`

   修改后如下：

![QQ截图20191222144956](.\img\QQ截图20191222144956.png)

2. 刷新配置，重启服务

   ```java
   systemctl daemon-reload
   systemctl restart docker
   docker start registry
   ```

3. 在pom.xml文件中增加配置

   ```java
       <build>
           <finalName>app</finalName>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
   
               <plugin>
                   <groupId>com.spotify</groupId>
                   <artifactId>docker-maven-plugin</artifactId>
                   <version>0.4.13</version>
                   <configuration>
                       <imageName>192.168.134.8:5000/${project.artifactId}:${project.version}</imageName>
                       <baseImage>jdk1.8</baseImage>
                       <entryPoint>["java","-jar","/${project.build.finalName}"]</entryPoint>
                       <resources>
                           <resoure>
                               <targetPath>/</targetPath>
                               <directory>${project.build.directory}</directory>
                               <include>${project.build.finalName}.jar</include>
                           </resoure>
                       </resources>
                       <dockerHost>http://192.168.134.8:2375</dockerHost>
                   </configuration>
               </plugin>
           </plugins>
       </build>
   
   ```

   相当于执行了

   ```shell
   FROM jdk1.8
   ADD app.jar /
   ENTRYPOINT ["java","-jar","/app.jar"]
   ```

   

4. 在windows的命令 提示符下，进入工程所在的目录，输入一下命令，进行打包和上传镜像

   ```java
   mvn clean package docker:build -DpushImage
   // 或者是下面这种？ 
   mvn docker:build -DpushImage
   ```

   浏览器访问 http://192.168.134.8:5000/v2/_catalog ，输出

   ```java
   {"repositories":["tensquare_base"]}
   ```

   进入宿主机，查看镜像

