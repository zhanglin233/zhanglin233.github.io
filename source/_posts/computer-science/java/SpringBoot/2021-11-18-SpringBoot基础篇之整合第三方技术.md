---
title: springboot基础篇之整合第三方技术
tags: java springboot
---

# Spring基础之整合第三方技术

## 整合Junit

1. 导入测试对应的starter
2. 测试类使用@SpringBoot修饰
3. 使用自动装配的形式添加要测试的对象

```java
// 名称: @SpringBootTest
// 类型: 测试类注解
// 位置: 测试类上方
// 作用: 设置Junit加载的SpringBoot启动类
// classes:设置SpringBoot的启动类
@SpringBootTest(classes=SpringBootJunitApplication.class)
class SpringBootJunitApplicationTest{}
```

4. 如果测试类在SpringBoot启动类的包或子包中，可以忽略启动类的设置，也就是忽略classes的设定

## 整合MyBatis

* 核心配置：数据库连接相关信息
* 映射配置：SQL映射(XML/注解)

1. 创建新模块，选择Spring初始化，并配置模块相关基础信息

2. 选择当前模块需要使用的技术集（MyBatis、MySQL）(勾选MyBatis技术，也就是导入MyBatis对应的starter)

   ![](https://gitee.com/nobody_heard_of_it/pic-md1/raw/master/image/20211119171012.png)

3. 设置数据源参数

 ```yaml
   # 数据库连接相关信息转换成配置
   spring:
     datasource:
       # 驱动类过时，提醒更换为com.mysql.cj.jdbc.Driver
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/ssm_db
       username: root
       password: root
 ```

4. SpringBoot版本低于2.4.3(不含)，Mysql驱动版本大于8.0时，需要在url连接串中配置时区 或在MySQL数据库端配置时区解决此问题

```yaml
jdbc:mysql: //localhost:3306/ssm_db?serverTimezone=UTC
```

5. 定义数据层接口与映射配

```java
// 数据库SQL映射需要添加@Mapper被容器识别
@Mapper
public interface UserDao {
    @Select("select * from user")
    public List<User> getAll();
}
```

6. 测试类中注入dao接口，测试功能	

```java
@SpringBootTest
class Springboot08MybatisApplicationTests {
    @Autowired
    private BookDao bookDao;
    @Test
    public void testGetById() {
        Book book = bookDao.getById(1);
        System.out.println(book);
    }
}
```

## 整合MyBatis-Plus

* MyBatis-Plus与MyBatis区别
  * 导入坐标不同
  * 数据层实现简化

1. 手动添加SpringBoot整合MyBatis-Plus的坐标，可以通过mvnrepository获取

   ```xml
   <dependency>
   	<groupId>com.baomidou</groupId>
   	<artifactId>mybatis-plus-boot-starter</artifactId>
   	<!-- 由于SpringBoot中未收录MyBatis-Plus的坐标版本，需要指定对应的Version-->
   	<version>3.4.3</version>
   </dependency>
   ```

2. 定义数据层接口与映射配置，继承BaseMapper

   ```java
   @Mapper
   public interface UserDao extends BaseMapper<User> {
   }
   ```

3. 其他同SpringBoot整合MyBatis

## 整合Druid

* 指定数据源类型

  ```yaml
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
      type: com.alibaba.druid.pool.DruidDataSourc
  ```

* 导入Druid对应的starter

```xml
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>druid-spring-boot-starter</artifactId>
<version>1.2.6</version>
</dependency>
```

* 变更Druid的配置方式

```yaml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

## 整合第三方技术通用方式

* 导入对应的starter

* 根据提供的配置格式，配置非默认值对应的配置项
