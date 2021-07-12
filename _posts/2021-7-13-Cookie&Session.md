---
layout: post
title: javaweb之Cookie&Servlet
tags: java 技术
---

# 今日内容
  1. 会话技术
    1.Cookie
    2.Session
  2. JSP:入门学习


# 会话技术
  1. 会话：一次会话中包含多次请求和响应
    * 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止
  2. 功能：在一次会话的范围内的多次请求间共享数据
  3. 方式：
    1. 客户端会话技术: Cookie
    2. 服务器端会话技术：Session

# Cookie:
  1. 概念：客户端会话技术，将数据保存到客户端
  2. 快速入门：
    * 使用步骤：
        1. 创建Cookie对象，绑定数据
          * new Cookie(String name,String value)
        2. 发送Cookie对象，绑定数据
          * response.addCookie(Cookie cookie)
        3. 获取Cookie,拿到数据
          * Cookie[] cookies = request.getCookies();
  3. 实现原理
    * 基于响应头set-cookie和请求头cookie实现

  4. cookie的细节
    1. 一次可不可以发送多个cookie?
      * 可以
      * 可以创建多个COokie对象，使用response调用多次方法发送cookie即可
    2. cookie在cookie中保持多长时间？
      * 默认情况下，当浏览器关闭后，cookie数据被销毁
      * 持久化储存
        * setMaxAge(int seconds)
          1. 正数:将Cookie数据写到硬盘中，持久化储存。并指定cookie存活时间，时间到后，cookie文件自动失效。
          2. 负数，默认值
          3.删除cookie值
    3. cookie能不能存中文？
      * tomcat 8之前cookie不能直接存中文
        * 需要将中文数据转码---一般采用URL编码（%E3)
      * 在tomcat 8 之后，cookie支持中文数据，特殊字符还是不支持，如空格，建议还是使用URL编码转储。
   4. cookie共享问题：
      1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie能不能共享？
          * 默认情况下cookie不能共享
          * setPath(String path):设置cookie的获取范围。默认情况下，设置当前虚拟目录
            * 如果要共享，则可以将path设置为"/"
      2. 不同的tomcat服务器间cookie共享问题？
        * setDomain(String path):如果设置一级域名相同，那么多个服务器之间cookie可以共享
          * setDomain(".baidu.com"),那么tieba.baidu.com和news.baidu.com中cookie可以共享。
    5. Cookie的特点和使用
      1. cookie储存数据在客户端浏览器
      2. 浏览器对于单个cookie的=大小有限制（4kb)以及对同一个域名下的总cokie数量也有限制（20个）
      * 作用
        1. cookie一般用于存储少量的不太敏感的数据
        2. 在不登录的情况下，完成服务器对客户端的身份识别

    6. 案例：记住上一次访问时间
      1. 需求：
        1. 访问一个Servlet，如果是第一次访问，则提示：您好，欢迎您首次访问
        2. 如果不是第一次访问，则提示：欢迎回来，您上次访问时间为：显示时间字符串。
      2. 分析：
        1. 可以采用cokie来完成
        2. 在服务器中的servlet判断是否有一个明为lastTime的cookie
          1. 有：不是第一次访问
            1. 响应数据：1. 响应数据：欢迎回来，您上次访问时间为:2018年6月10日11:50:20
				    2. 写回Cookie：lastTime=2018年6月10日11:50:01
			    2. 没有：是第一次访问
				    1. 响应数据：您好，欢迎您首次访问
				    2. 写回Cookie：lastTime=2018年6月10日11:50:01

        3. 代码实现：
      ```java
      package cn.itcast.cookie;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

@WebServlet("/cookieTest")
public class CookieTest extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置响应的消息体的数据格式及编码
        response.setContentType("text/html;charset=utf-8");
        boolean flag = false; //没有cookie为lastTime
        //1.获取所有cookie
        Cookie[] cookies = request.getCookies();
        //遍历cookie数组
        if(cookies!=null&&cookies.length>0){
            for (Cookie c:cookies
            ) {
                //获取cookie的名称
                String name = c.getName();
//                String value = c.getValue();
                //4.判断名称是不是lastTime
                if("lastTime".equals(name)){
                    //有该cookie，不是第一次访问
                    //设置cookie的value
                    //获取当前时间，重新设置cookie，重新发送cookie
                    flag = true;
                    Date date = new Date();
                    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日HH:mm:ss");
                    String str_date = sdf.format(date);
                    c.setValue(str_date);
                    //设置cookie存活时间
                    c.setMaxAge(60*60*24*30);  //设置为1个月
                    response.addCookie(c);  //重新发送cookie

                    //响应数据
                    //获取cookie的value时间
                    String value = c.getValue();
                    response.getWriter().write("<h1>欢迎回来，您上次访问时间为："+value+"</h1>");

                    break;
                }
            }

        }
        if(cookies==null|| cookies.length==0||flag==false){
            //没有，第一次访问
            Date date = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日HH:mm:ss");
            String str_date = sdf.format(date);
            Cookie cookie = new Cookie("lastTime",str_date);

            //设置cookie存活时间
            cookie.setMaxAge(60*60*24*30);  //设置为1个月
            response.addCookie(cookie);  //重新发送cookie
            response.getWriter().write("<h1>您好，欢迎首次访问</h1>");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

      ```
