---
title: SpringBoot运维实用篇之配置高级
tags: java SpringBoot
categories:
- [计算机科学, Java, SpringBoot]
---

# springboot运维实用篇之配置高级

## 临时属性配置

* 带属性数启动SpringBoot,使用替换配置文件中的属性

  ```java
  java -jar 工程名.jar --server.port=80
  ```

  ![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211119220801.png)

* 通过编程形式带参数启动SpringBoot程序，为程序添加运行参数

  ```java
  public static void main(String[] args) {
  	String[] arg = new String[1];
  	arg[0] = "--server.port=8080";
  	SpringApplication.run(SSMPApplication.class, arg);
  }
  ```

* 不携带参数启动SpringBoot程序

  ```java
  public static void main(String[] args) {
  	SpringApplication.run(SSMPApplication.class);
  }
  
  ```

  

* 携带多个属性启动SpringBoot,属性之间使用空格分隔

* 临时属性必须是当前boot工程支持的属性，否则设置无效

* 属性加载优先级

  []([Core Features (spring.io)](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config))

  ![/](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211119214529.png)

## 配置文件分类

### SpringBoot中4级配置文件 

1级： file ：config/application.yml 【最高】 （file：与jar包位于同一目录下）

2级： file ：application.yml 

3级：classpath：config/application.yml 

4级：classpath：application.yml 【最低】

### 作用

* 1级与2级留做系统打包后设置通用属性，1级常用于运维经理进行线上整体项目部署方案调控
* 3级与4级用于系统开发阶段设置通用属性，3级常用于项目经理进行整体项目属性调控
* 如果yml与properties在不同层级中共存会是什么效果？ 例：类路径application.properties属性是否覆盖文件系统config目录中application.yml属性
  * 项目类路径配置文件：服务于开发人员本机开发与测试 
  * 项目类路径config目录中配置文件：服务于项目经理整体调控 
  * 工程路径配置文件：服务于运维人员配置涉密线上环境 
  * 工程路径config目录中配置文件：服务于运维经理整体调控 

* 多层级配置文件间的属性采用叠加并覆盖的形式作用于程序

## 自定义配置文件

