--- 
layout: post
title: Spring的web.xml配置
date: 2018-11-18
tags: Spring
---
### **web.xml**
### **内容一：**
配置默认页面：
当用户在浏览器中输入的URL不包含某个servlet名或JSP页面时，welcome-file-list元素可指定显示的默认文件。

``` 
<!-- displayname 只是用来显示项目名称 可配置可不配置 -->
	<display-name>ssm-demo</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
```
### **内容二：**
``` 
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
```
context-param是以键值对形式存在的
context-param使用方法：由于context-param是配置在web下面，属于上下文参数，在整个环境中都可使用，存放在getServletContext对像中，因此使用方法是：getServletContext().getInitParameter(“user”)，如：

``` 
public void service(HttpServletRequest request, HttpServletResponse response)throws ServletException,IOException{
        String user=getServletContext().getInitParameter("user");
        System.out.println(getServletContext().getInitParameter("user"));
        System.out.println(user);       
    }
```
### **内容三：**
配置字符编码过滤器
先配置过滤的实现类
再配置被过滤的请求类型 /* 表示所有
``` 
   <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
### **内容四：**
配置Spring的监听器
和SpribgMVC转发器以及拦截的请求类型
``` 
  <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>

```
init-param使用方法：由于init-param是配置在servlet中，属于某一下servlet，存放在getServletConfig中，因此使用方法是：getServletConfig().getInitParameter(“user1”); 由于它属于当前的servlet类，所以用this替代getServletConfig(), 使用this.getInitParmeter(“user1”) , 如：

``` 
    public void service(HttpServletRequest request, HttpServletResponse response)throws ServletException,IOException{
        String user=getServletContext().getInitParameter("user1");
        System.out.println(getServletConfig().getInitParameter("user1"));
        //或this.getInitParmeter("user1");
        System.out.println(user1);      
    }

```

### **内容五：**
配置各种错误返回类型所对应的返回页面

``` 
<error-page>
        <error-code>404</error-code>
        <location>/main.jsp</location>
    </error-page>

    <error-page>
        <error-code>500</error-code>
        <location>/main.jsp</location>
    </error-page>
```
### **beans.xml文件**
![enter description
here](https://viabcde.github.io/images/blog/2018092897.png)  

