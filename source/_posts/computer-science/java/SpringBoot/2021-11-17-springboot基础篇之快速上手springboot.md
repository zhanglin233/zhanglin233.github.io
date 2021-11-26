---
title: springboot基础篇之快速上手springboot
tags: java SpringBoot
categories:
- [计算机科学, Java, SpringBoot]
---

# 快速上手Springboot

## SpringBoot简介

* SpringBoot是由Pivotal团队提供的全新框架，其设计目的是用来简化Spring应用的初始搭建以及开发过程 。

*  Spring程序缺点 

> 依赖设置繁琐 

> 配置繁琐 

* SpringBoot程序优点 

> 起步依赖（简化依赖配置） 

> 自动配置（简化常用工程相关配置）

> 辅助功能（内置服务器，……

* Spring程序与SpringBoot程序对比

  ![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117200144.png)

  

## 创建SpringBoot工程的四种方式

### 基于Idea创建SpringBoot工程 

1. 创建新模块，选择Spring Initializr，并配置模块相关基础信息

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117200639.png)

2. 选择当前模块需要使用的技术集

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117200722.png)

3. 开发控制器类

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211119174712.png)

4. 运行自动生成的Application类

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211119174735.png)

从图中可以看到项目所运行的服务器为Tomcat及服务器运行的端口号为8080.

5. 小结

* 开发SpringBoot程序可以根据向导进行联网快速制作（基于idea开发SpringBoot程序需要确保联网且能够加载到程序框架结构）

* SpringBoot程序需要基于JDK8进行制作

* SpringBoot程序中需要使用何种功能通过勾选选择技术

* 运行SpringBoot程序通过运行Application程序入口进行

### 于官网创建SpringBoot工程 

基于SpringBoot官网创建项目，地址 :<https://start.spring.io>

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117201433.png)

所填属性与通过Idea创建工程相同

### 基于阿里云创建SpringBoot工程 

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117201551.png)

基于阿里云创建springboot项目只需在第三步中取消勾选默认的start来源，改为勾选Custom用户自定义，并填入阿里云网址<http://start.aliyun.com>即可，其它操作与基于Idea创建SpringBoot工程相同。

* 注意事项

1. 阿里云提供的坐标版本较低，如果需要使用高版本，进入工程后手工切换SpringBoot版本 

2. 阿里云提供的工程模板与Spring官网提供的工程模板略有不通

### 手工创建Maven工程修改为SpringBoot工程

1. 创建普通Maven工程 
2. 继承spring-boot-starter-parent 
3. 添加依赖spring-boot-starter-web 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.6</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>spring_01_01_quickStart</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>

```

4.  制作引导类Application

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Spring0101QuickStartApplication {

    public static void main(String[] args) {
        SpringApplication.run(Spring0101QuickStartApplication.class, args);
    }

}
```





## 隐藏指定文件/文件夹

1. Setting → File Types → Ignored Files and Folders 

2. 输入要隐藏的文件名，支持*号通配符 

3. 回车确认添加

## 入门案例解析

### parent

* 开发SpringBoot程序要继承spring-boot-starter-parent

```java
	<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.6</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>spring_01_01_quickStart</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>

```

*  spring-boot-starter-parent中定义了若干个依赖管理 

* 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突 

* 继承parent的形式也可以采用引入依赖的形式实现效果

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117203328.png)

### starter

```java
	<dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
     </dependency>
```

starter中包含了许多依赖

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117203725.png)

将鼠标放在starter上按住Ctrl+鼠标左击即可查看详细信息。

1. starter 

   SpringBoot中常见项目名称，定义了当前项目使用的所有依赖坐标，以达到减少依赖配置的目的 

2. parent 

   所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的 spring-boot-starter-parent各版本间存在着诸多坐标版本不同 

3. 实际开发 

   使用任意坐标时，仅书写GAV中的G(groupId)和A(artifactId)，V(version)由SpringBoot提供，除非SpringBoot未提供对应版本V，如发生坐标错误(starter中未包含当前依赖)，再指定Version（要小心版本冲突)     

### 引导类

* 启动方式 

  SpringBoot的引导类是Boot工程的执行入口，运行main方法就可以启动项目

  SpringBoot工程运行后初始化Spring容器，扫描引导类所在包加载bean

```java
@SpringBootApplication
public class Spring0101QuickStartApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = SpringApplication.run(Spring0101QuickStartApplication.class, args);
        bookController bean = ctx.getBean(bookController.class);
        System.out.println("=====>"+bean);
    }

}
```

![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117210124.png) 

### Tomcat

1. 内嵌Tomcat服务器是SpringBoot辅助功能之一 

2. 内嵌Tomcat工作原理是将Tomcat服务器作为对象运行，并 将该对象交给Spring容器管理 

   查看spring-boot-starter-web依赖详细信息可以看到其中包括了Tomcat

   ![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117212854.png)

3. 变更内嵌服务器思想是去除现有服务器，添加全新的服务器

   想要变更服务器只需修改pom.xml文件中的相关信息即可。

   ```java
          <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
   <!--            web起步依赖环境中，排除Tomcat起步依赖-->
               <exclusions>
                   <exclusion>
                       <groupId>org.springframework.boot</groupId>
                       <artifactId>spring-boot-starter-tomcat</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
   <!--        添加jetty起步依赖，版本由SpringBoot的starter控制-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-jetty</artifactId>
           </dependency>
   ```

   重新启动程序后，发现服务器相关信息以变为jetty

   ![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211117213752.png)

4. 内置服务器

   tomcat(默认)   apache出品，粉丝多，应用面广，负载了若干较重的组件

   jetty     			 更轻量级，负载性能远不及tomcat

   undertow  	  undertow，负载性能勉强跑赢tomcat