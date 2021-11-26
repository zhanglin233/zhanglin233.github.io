---
title: java之junit测试
tags: java SpringBoot
categories:
- [计算机科学, Java, SpringBoot]
---
# junit
## 什么是junit
JUnit是一个Java语言的单元测试框架。它由Kent Beck和Erich Gamma建立，逐渐成为源于Kent Beck的sUnit的xUnit家族中最为成功的一个。 JUnit有它自己的JUnit扩展生态圈。多数Java的开发环境都已经集成了JUnit作为单元测试的工具。

也就是说junit就是别人写好的单元测试框架，使用此框架你可以大大缩短你的测试时间和准确度（笔者现在还记得大一刚来的的时候，c语言写的小程序，每次都是重启测试，那种编译-输入--停止-编译的苦日子，很痛苦，今天用junit这个单元测试框架好多了）。

### 单元测试是什么
百度百科的解释是这样的：单元测试（模块测试）是开发者编写的一小段代码，用于检验被测代码的一个很小的、很明确的功能是否正确。通常而言，一个单元测试是用于判断某个特定条件（或者场景）下某个特定函数的行为。例如，你可能把一个很大的值放入一个有序list 中去，然后确认该值出现在list 的尾部。或者，你可能会从字符串中删除匹配某种模式的字符，然后确认字符串确实不再包含这些字符了。

简单的说，单元测试就是对你程序中最小的功能模块进行测试，在c语言里可能是一个函数，java中可能是一个方法或者类。目的就是为了提高代码的质量。

###为什么要引入单元测试
平常写代码的时候经常需要检验某些方法功能是否正常，正常情况下需要创立完整的类来运行检验该方法，这样难免效率低下，引入junit之后就不用构建一个完整的程序便可以对某一方法进行检验。

## IDEA中junit的使用

### 创建包名及代码

 包名规范

单元测试的代码都放在test包下，和源码不在同一个包下
![]({{site.url}}/images/2021_7_10_junit/1.png)           
如图所示，DaoTest类单独放在test包下。
测试的类方法都以test开头，后面接要测试的类或者方法的名字

#### junit使用方法
以下图代码为例
```java
package cn.itcast.test;

import cn.itcast.dao.UserDao;
import cn.itcast.domain.User;


public class DaoTest {

    public void testLogin(){
        User loginUser = new User();
        loginUser.setUsername("superBaby");
        loginUser.setPassword("123");


        UserDao dao = new UserDao();
        User user = dao.login(loginUser);
        System.out.println(user);
    }
}

```
在要测试的方法前键入@test，这是会发现test为红色提示。
![]({{site.url}}/images/2021_7_10_junit/2.png)
这是我们只需按住Ctrl+shift+Alt+s打开项目结构，找到库并在右边找到+号按钮新建一个java库，并在idea的安装目录中的lib文件夹找到junit-4.12.jar文件并导入即可。
![]({{site.url}}/images/2021_7_10_junit/6.png)
![]({{site.url}}/images/2021_7_10_junit/3.png)
接下来，便需要将junit.jar文件导入到模块中。
![]({{site.url}}/images/2021_7_10_junit/4.png)
选中junit4.12jar文件并点击右上方的+号并添加到模块依赖中。
![]({{site.url}}/images/2021_7_10_junit/5.png)
接下来便返回项目中，导入org.junit.test包后，可以看到@Test变为了正常的颜色，接下来点击方法前面的绿色三角形便可以对方法进行测验了。
![]({{site.url}}/images/2021_7_10_junit/7.png)
