---
title: JavaWEB之Servlet&HTTP&Request
tags: Java JavaWEB
categories:
- [计算机科学, Java, JavaWEB]
---

# Servlet
## 1.概念  
  Servlet（Server Applet）是Java Servlet的简称，称为小服务程序或服务连接器。

狭义的Servlet是指Java语言实现的一个接口，
广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。
## 2.步骤
## 3.执行原理
  当服务器接收到浏览器客户的请求之后，会解析请求的URL路径，获取访问的servlet的资源路径，找到项目，查找web.xml文件，是否有对应的标签体内容，如果有，则找到对应的标签内的全类名，tomcat会将字节码文件加载进内存，并且创建其对象，调用其方法。
## 4.生命周期
### 被创建：
  执行servlet的 init()方法 ， 只执行一次，说明servlet在内存中是单例的（多用户同时访问，可能存在线程安全问题，尽量不要在servlet中定义成员变量，即使定义了成员变量，也不要对其修改值），默认情况下，第一次被访问时被创建（可配置servlet的创建时期：值为负整数，第一次被访问时创建/值为0或者为正整数，则在服务器启动时创建
### 提供服务：
执行service（）方法，每次访问servlet时，service（）方法都会被调用一次。
### 被销毁：
  destory（）方法在servlet被销毁之前只执行一次，用于释放资源。服务器关闭时，servlet被销毁。只有服服务器正常关闭时，才会执行destory()方法。
## 5.Servlet3.0注解配置
### 什么是Servlet3.0
  Servlet3.0是Java EE6规范的一部分，Servlet3.0提供了注解(annotation)，使得不再需要在web.xml文件中进行Servlet的部署描述，简化开发流程。  
### 开发Servlet3.0程序的所需要的环境
  开发Servlet3.0的程序需要一定的环境支持。MyEclipse10和Tomcat7都提供了对Java EE6规范的支持。Tomcat需要Tomcat7才支持Java EE6，Tomcat7需要使用JDK1.6以上的版本。
  详细内容(https://www.cnblogs.com/xdp-gacl/p/4222902.html)
## 6.Servlet的体系结构
  Servlet -- 接口
        |
  GenericServlet -- 抽象类
        |
  HttpServlet -- 抽象类  


### GenericServlet：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象，将来定义Servlet类时，可以继承GenericServlet，实现service()方法即可
### HttpServlet：对http协议的一种封装，简化操作
#### 定义类继承HttpServlet
#### 复写doGet/doPost方法


# HTTP
## 概念
  Hyper Text Transfer Protocol 超文本传输协议
### 特点
  - 基于TCP/IP的高级协议
  - 默认端口号:80
  - 基于请求/响应模型的:一次请求对应一次响应
  - 无状态的：每次请求之间相互独立，不能交互数

### 版本
 1.0版本：每一次连接都会建立新的连接
 1.1版本：复用链接

## 请求消息数据格式
### 1.请求行
  请求方式  请求url      请求协议/版本
  GET      /login.html  HTTP/1.1

 #### HTTP协议有7种请求方式，常用的有2种
 ##### GET:
 - 请求参数在请求行中，在url后
 - 请求的url长度有限制
 - 不太安全

##### POST：
- 请求参数在请求体中
- 请求的url长度没有限制
- 相对安全

### 2.请求头: 客户端告诉服务器一些信息
  请求头名称：请求头值
#### 常见的请求头
- user-agent:浏览器告诉服务器，我访问你使用的浏览器版本信息。可以在服务器端获取该头的信息，解决浏览器的兼容性问题
- Referer: ：http://localhost/login.html
告诉服务器，我(当前请求)从哪里来？

```java
  package cn.itcast.web.request;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;

  @WebServlet("/requestDemo3")
  public class RequestDemo3 extends HttpServlet {
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      }

      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          //获取请求头数据：user-agent

          String agent = request.getHeader("user-agent");
          //判断agent的浏览器版本
          if(agent.contains("Chrome")){
              //谷歌
              System.out.println("谷歌.....");
          }
          else if(agent.contains("Firefox")){
              System.out.println("火狐来了");
          }
      }
  }
```


##### 作用：

###### 防盗链：


```java
  package cn.itcast.web.request;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;

  @WebServlet("/requestDemo4")
  public class RequestDemo4 extends HttpServlet {
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      }

      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          //演示获取请求头数据:reference
          String refer = request.getHeader("referer");
          System.out.println(refer);

          //防盗链
         if(refer!=null){
              if(refer.contains("/day14_servlet_http_request_war_exploded")){
  //                System.out.println("播放电影");

                  response.getWriter().write("播放电影");
              }
              else{
                  //盗链
  //                System.out.println("想看电影吗");
                  response.setContentType("text/html;charset=utf-8");
                  response.getWriter().write("想看电影吗");
              }
          }
      }
  }
```
###### 统计工作：

### 3.请求空行
 空行，就是用于分割POST请求头和请求体的

### 4.请求体(正文)
- 封装POST请求消息的请求参数
- 字符串格式：

```
POST /login.html HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101
Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://localhost/login.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1
username=zhangsan
```

# Request
## 1.request对象和response对象的原理
### 1.request和response对象是由服务器创建的，我们来使用它。
### 2.request对象是来获取请求消息，response对象是来设置响应信息

## 2.request对象继承体系结构
	ServletRequest -- 接口
		| 继承
	HttpServletRequest -- 接口
		| 实现
	org.apache.catalina.connector.RequestFacade 类(tomcat)

## 3.request功能
### 1获取请求消息数据
####  1.获取请求行数据
##### GET /day14/demo1?name=zhangsan HTTP/1.1
方法：
-  获取请求方式 ：GET
	 String getMethod()
-  获取虚拟目录：/day14

 	String getContextPath()
-  获取Servlet路径: /demo1
	String getServletPath()
- 获取get方式请求参数：name=zhangsan
	 String getQueryString()
- 获取请求URI：/day14/demo1
	String getRequestURI(): /day14/demo1
	StringBuffer getRequestURL() :http://localhost/day14/demo1
	URL:统一资源定位符 ： http://localhost/day14/demo1 中华人民共和国
	URI：统一资源标识符 : /day14/demo1 共和国
- 获取协议及版本：HTTP/1.1
	String getProtocol()
- 获取客户机的IP地址：
	String getRemoteAddr()

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        /**
         * GET
         * /day14_servlet_http_request_war_exploded
         * /requestDemo1
         * null
         * /day14_servlet_http_request_war_exploded/requestDemo1
         * HTTP/1.1
         * 0:0:0:0:0:0:0:1
         */

        //1.获取请求方式
        String method = request.getMethod();
        System.out.println(method);
        //2.获取虚拟路径
        String contextPath = request.getContextPath();
        System.out.println(contextPath);

        //3.获取servlet路径：/demo1
        String servletPath = request.getServletPath();
        System.out.println(servletPath);

        //4.获取get方式请求参数
        String queryString = request.getQueryString();
        System.out.println(queryString);

        //5.获取请求URI
        String requestURI = request.getRequestURI();
        System.out.println(requestURI);

        //6.获取协议及版本
        String protocol = request.getProtocol();
        System.out.println(protocol);

        //7.获取客户机的ip地址
        String remoteAddr = request.getRemoteAddr();
        System.out.println(remoteAddr);
    }
}
```

#### 2.获取请求头数据
	方法：
	* String getHeader(String name):通过请求头的名称获取请求头的值
 Enumeration<String> getHeaderNames():获取所有的请求头名称   

```java
//演示获取请求头数据:reference
String refer = request.getHeader("referer");
System.out.println(refer);

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //演示获取请求头数据
        //1.获取所有请求头名称
        Enumeration<String> headerNames = request.getHeaderNames();
        while(headerNames.hasMoreElements()){
            String name = headerNames.nextElement();
            String value = request.getHeader(name);
            System.out.println(name+"------"+value);
        }
    }
```

####  3.获取请求体数据:
* 请求体：只有POST请求方式才有请求体，在请求体中封装了POST请求的请求参数
* 步骤：
	1. 获取流对象
		* BufferedReader getReader()：获取字符输入流，只能操作字符数据
		* ServletInputStream getInputStream()：获取字节输入流，可以操作所有类型数据案例：用户登录
		* 在文件上传知识点后讲解
	2. 再从流对象中拿数据
##  其它功能

### 1. 获取请求参数通用方式：不论get还是post请求方式都可以使用下列方法来获取请求参数
#### 1. String getParameter(String name):根据参数名称获取参数值 username=zs&password=123
#### 2. String[] getParameterValues(String name):根据参数名称获取参数值的数组
hobby=xx&hobby=game
#### 3. Enumeration<String> getParameterNames():获取所有请求的参数名称
#### 4. Map<String,String[]> getParameterMap():获取所有参数的map集合

- 中文乱码问题：
- get方式：tomcat 8 已经将get方式乱码问题解决了
- post方式：会乱码
- 解决：在获取参数前，设置request的编码request.setCharacterEncoding("utf-8");  

```java
package cn.itcast.web.request;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedReader;
import java.io.IOException;
import java.util.Enumeration;
import java.util.Map;
import java.util.Set;

@WebServlet("/requestDemo6")
public class RequestDemo6 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取参数通用方式：不论是get方式还是Post方式都可以使用下列方法
        //post获取请求参数


        //post请求中文乱码问题
        //1.设置流的编码
        request.setCharacterEncoding("utf-8");
        //根据参数名获取参数值
        String username = request.getParameter("username");
//        System.out.println("post");
//        System.out.println(username);

        //根据参数名获取参数值的数组
        String[] hobbies = request.getParameterValues("hobby");
//        for (String hobby:hobbies
//             ) {
//            System.out.println(hobby);
//        }

        Enumeration<String> parameterNames = request.getParameterNames();;
//        while(parameterNames.hasMoreElements()){
//            String name = parameterNames.nextElement();
//            System.out.println(name);
//            String value = request.getParameter(name);
//            System.out.println(value);
//            System.out.println("------------------------------");
//        }

        //获取所有参数map集合
        Map<String, String[]> parameterMap = request.getParameterMap();
        //遍历
        Set<String> keySets = parameterMap.keySet();
        for (String name:keySets
             ) {
            String[] values = parameterMap.get(name);
            System.out.println(name);
            for (String value:values
                 ) {
                System.out.println(value);
            }
            System.out.println("---------------------");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {


        //get获取请求参数
//        String username = request.getParameter("username");
//        System.out.println("get");
//        System.out.println(username);
        this.doPost(request,response);
    }
}
```

### 2. 请求转发：一种在服务器内部的资源跳转方式
#### 1. 步骤：
- 通过request对象获取请求转发器对象：RequestDispatcher
getRequestDispatcher(String path)
- 使用RequestDispatcher对象来进行转发：forward(ServletRequest request,
ServletResponse response)

```java
 request.getRequestDispatcher("/requestDemo8").forward(request,response);
 ```
#### 2.特点：
- 浏览器地址栏路径不发生变化
- 只能转发到当前服务器内部资源中。
- 转发是一次请求
###3. 共享数据：
* 域对象：一个有作用范围的对象，可以在范围内共享数据
* request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
#### 方法：
- void setAttribute(String name,Object obj):存储数据
- Object getAttitude(String name):通过键获取值
- void removeAttribute(String name):通过键移除键值对
- 获取ServletContext：
  * ServletContext getServletContext()

  ```java
  request.setAttribute("msg","hello");
  Object msg = request.getAttribute("msg");
        System.out.println(msg);
  ```
