---
title: springboot基础篇之基础配置
tags: java SpringBoot
categories:
- [计算机科学, Java, SpringBoot]
---

# springboot基础篇之基础配置

## 属性配置

### 复制工程

* 原则
  * 保留工程基础结构
  * 抹掉原始工程结构

1. 在工作空间中复制对应工程，并修改工程名称
2. 删除与Idea有关的配置，仅保留src目录与pom.xml文件
3. 修改pom.xml文件中的artifachId与新工程/模块名相同
4. 删除name标签（可选）
5. 保留备份工程后期使用

### 修改配置

* 修改服务器端口

  SpringBoot默认配置文件为application.properties，通过键值对配置相应属性。修改服务器端口：

```properties
server.port = 80
```

* 关闭运行日志图标

```properties
spring.main.banner-mode=off
```

* 设置日志相关

```properties
logging.level.root = debug
```

* [SpringBoot内置属性查询]([Common Application Properties (spring.io)](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties)),官方文档中参考第一项：Application properties

* 书写Spring Boot配置采用关键字+提示形式书写，SpringBoot中只有导入了对应Starter后，才后提供对应配置属性提示

## 配置文件分类

* SpringBoot提供了多种属性配置方式，分别为application.properties、application.yaml、application.yml, SpringBoot默认的配置方式为application.properties,但以properties文件结构不够清晰简洁，因此常用yml配置文件。

  * application.properties

  ```properties
  server.port = 80
  ```

  * application.yml(：后必须留有空格)

  ```yaml
  server:
  	port: 80
  ```

  * application.yaml

  ```yaml
  server:
  	port: 80

* SpringBoot允许三种配置文件共存，配置文件加载顺序：.properties > yml > yaml
* 不同配置文件中相同配置按照优先级相互覆盖，不同配置文件中不同配置全部保留
* 自动提示功能消失方案解决（首先确保引入了对应的starter）--指定SpringBoot配置文件
  * Idea中打开Setting->Pro金额词条Structure->Facets
  * 选中相应项目/工程
  * Customize Spring Boot
  * 选择配置文件

## yaml文件

* YAML(YAML Ain't Markup Language) 一种数据序列化格式
* 优点：
  * 容易阅读
  * 容易与脚本语言交互
  * 以数据为核心
  * 重数据轻格式
* YAML文件拓展名
  * .yml(主流)
  * .yaml
* yaml语法规则
  * 大小写敏感
  * 属性层级关系使用多行描述，每行结尾使用冒号结束
  * 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格，不允许使用tab键
  * 属性名与属性值之间使用冒号+空格作为分隔
  * \# 表示注释
  * 核心规则：数据前面要使用冒号与空格隔开

```yaml
# 字面值表示方式
boolean: TRUE
float: 3.14
int: 123
null: ~
string: HelloWorld
string2: "HelloWorld" # 字符串可以直接而书写也可以使用引号包裹
date: 2018-02-17      # 日期必须使用yyyy-MM-dd格式
datatime: 2018-02-17T15:02:31+08:00    #日期和时间之间使用T连接，最后使用+代表时区


# 数组表示方法：在属性名下方使用-作为数据开始符号，每行书写一个数据,减号与数据间使用空格分隔
subject:
  - Java
  - 前端
  - 大数据
users:             # 对象数组格式
  - name: tom
    age: 4
  - name: Jerry
    age: 5
    
# 对象数组缩略格式
users2: [{name:tom , age:4}]
```



## yaml数据读取

* 单个数据读取

  * 使用@Value配合SpeEL读取单个数据

  * 如果数据存在多层级，依次书写多层级名称即可

  * ```java
    public classtest(){
    	@value("${lesson}")
        private String lessonName;
        
        @value("${serve.port}")
        private int port;
    }
    ```

  * 属性中如果存在转义字符，需要使用双引号包裹

  * ```java
    lesson: "Spring\boot\lesson"
    ```

* 封装全部数据到Environment对象

```java
public classtest(){
    # 使用Autowired自动装配到Environment对象中
	@Autowired
    private Environment env;
    System.out.println(env.getProperty("lesson"));
	System.out.println(env.getProperty("users"));
}
```

* 自定义对象封装指定数据

* ```java
  @Component
  @ConfigurationProperties(prefix = "users")
  public  class users{
      private String name;
      private int age;
  }
  ```

* 使用@ConfigurationProperties注解绑定配置信息到封装类中

* 封装类需要定义为Spring管理的Bean,否则无法进行属性注入

