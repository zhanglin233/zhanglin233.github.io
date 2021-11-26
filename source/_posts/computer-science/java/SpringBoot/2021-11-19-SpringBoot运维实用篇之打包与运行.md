---
layout: post
title: springboot运维实用篇之打包与运行
tags: java SpringBoot
categories:
- [计算机科学, Java, SpringBoot]
---

# 打包与运行

SpringBoot项目可以基于java环境下独立运行jar文件下独立运行jar文件启动服务

## 程序打包与运行（Windows版）

### SpringBoot项目快速启动

1. 对SpringBoot项目打包(执行Maven构建指令package)

   ```java
   mvn package
   ```

2. 运行项目(执行启动指令)

   ```java
   java -jar 打包后的项目名称.jar
   ```

   **注意事项：jar支持命令行启动需要依赖maven插件支持，请确认打包时是否具有SpringBoot对应的maven插件**

   ```xml
   <build>
   	<plugins>
   		<plugin>
   			<groupId>org.springframework.boot</groupId>
   			<artifactId>spring-boot-maven-plugin</artifactId>
   		</plugin>
   	</plugins>
   </build>
   ```

### 可执行jar包目录结构

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211119212734.png)

### jar包描述文件(MANIFEST.MF)

* 普通工程

  ```json
  Manifest-Version: 1.0
  Implementation-Title: spring_01_01_quickStart
  Implementation-Version: 0.0.1-SNAPSHOT
  Build-Jdk-Spec: 1.8
  Created-By: Maven Jar Plugin 3.2.0
  
  ```

* 基于Spring-Boot-maven-plugin打包的工程

  ```json
  Manifest-Version: 1.0
  Spring-Boot-Classpath-Index: BOOT-INF/classpath.idx
  Implementation-Title: spring_01_01_quickStart
  Implementation-Version: 0.0.1-SNAPSHOT
  Spring-Boot-Layers-Index: BOOT-INF/layers.idx
  Start-Class: com.example.Spring0101QuickStartApplication
  Spring-Boot-Classes: BOOT-INF/classes/
  Spring-Boot-Lib: BOOT-INF/lib/
  Build-Jdk-Spec: 1.8
  Spring-Boot-Version: 2.5.6
  Created-By: Maven Jar Plugin 3.2.0
  Main-Class: org.springframework.boot.loader.JarLauncher
  ```

  

### 命令行启动常见问题及解决方案

* Windows端口被占用

  ```json
  # 查询端口
  netstat -ano
  # 查询指定端口
  netstat -ano |findstr "端口号"
  # 根据进程PID查询进程名称
  tasklist |findstr "进程PID号"
  # 根据PID杀死任务
  taskkill /F /PID "进程PID号"
  # 根据进程名称杀死任务
  taskkill -f -t -im "进程名称"
  ```

## 程序打包与运行(Linux版)

1. 上传安装包
2. 执行jar命令：java -jar 工程名.jar
3. Windows与Linux下执行Boot打包程序流程相同，仅需确保运行环境有效即可