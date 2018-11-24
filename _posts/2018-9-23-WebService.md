--- 
layout: post
title: webservice
date: 2018-9-23
tags: java
---

###  **基于jax的WebService**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092301.png)  
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092302.png)
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092303.png)
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092304.png)
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092305.png)
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092306.png)
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092307.png)  
**然后一路点确定**

``` 
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.test</groupId>
  <artifactId>01_jaxes_server</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>01_jaxws_server</name>
    <dependencies>
      <!--进行jaxws服务开发-->
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-frontend-jaxws</artifactId>
        <version>3.0.1</version>
      </dependency>

      <!--内置jetty web服务器-->
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-transports-http-jetty</artifactId>
        <version>3.0.1</version>
      </dependency>
      <!--日志实现-->
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.12</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
        <scope>test</scope>
      </dependency>
    </dependencies>
  <build>
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.2</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
            <encoding>UTF-8</encoding>
            <showWarnings>true</showWarnings>
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```
**删除com.test下自动生成的文件**  
**然后在com.test下新建service包**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092308.png)  
**再新建一个接口HelloService.java**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092309.png) 

``` 
package com.test.service;

/**
 * 对外发布服务的接口
 */

import javax.jws.WebService;

@WebService
public interface HelloService {
    /**
     * 对外发布服务的接口的方法
     */
    public String sayHello(String name);
}

```
**创建接口的实现类**

![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092310.png)  

``` 
package com.test.service.impl;

import com.test.service.HelloService;

public class HelloServiceImpl implements HelloService{
    public String sayHello(String name) {
        return name + ",welcome!";
    }
}

```
**发布服务**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092311.png)  
psvm：快捷敲出main方法
sout: 快捷敲出system.out.println

``` 
package com.test;

import com.test.service.impl.HelloServiceImpl;
import org.apache.cxf.jaxws.JaxWsServerFactoryBean;

public class Server {
    public static void main(String[] args) {
        //发布服务的工厂
        JaxWsServerFactoryBean factory = new JaxWsServerFactoryBean();

        //设置服务地址
        factory.setAddress("http://localhost:8000/ws/hello");

        //设置服务类
        factory.setServiceBean(new HelloServiceImpl());

        //发布服务
        factory.create();

        System.out.println("发布服务成功，端口8000....");
    }
}

```
**运行**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092312.png) 
**测试**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092313.png)  
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092314.png)  
**客户端**
新建一个moudle
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092315.png)  
绑定本地仓库
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092316.png)  
把服务端的pom.xml文件的依赖复制到客户端的pom.xml中

**再把服务端的接口复制到客户端HelloService**

![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092317.png) 
**客户端测试**

``` 
package com.test;

import com.test.service.HelloService;
import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;

public class Client {
    public static void main(String[] args) {
        //服务接口访问地址：http://localhost:8000/ws/hello

        //创建cxf代理工厂
        JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();

        //设置远程访问服务端的地址
        factory.setAddress("http://localhost:8000/ws/hello");

        //设置接口类型
        factory.setServiceClass(HelloService.class);

        //对接口生成代理对象
        HelloService helloService = factory.create(HelloService.class);

        //代理对象类型 class com.sun.proxy.$Proxy34
        System.out.println(helloService.getClass());

        //远程访问服务端方法
        String Content = helloService.sayHello("jet");
        System.out.println(Content);
    }
}

```
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092318.png) 
alt+enter 自动导包
**配置日志拦截器**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092320.png) 
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092319.png) 
在服务端添加拦截器  
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092321.png) 
### **总结**
服务端编写接口及其实现类，再通过工厂设置好服务地址及服务类并发布服务   
客户端编写接口，再通过代理工厂设置要访问的服务端地址及服务接口，由工厂生成可直接使用的代理对象

### **WebService整合Spring**
**新建module**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092322.png) 
ctrl + n 搜索类文件
**配置pom.xml文件**

``` 
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.test</groupId>
  <artifactId>03_jaxws_spring_server</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>03_jaxws_spring_server Maven Webapp</name>
 <dependencies>
   <!--进行jaxws服务开发-->
   <dependency>
     <groupId>org.apache.cxf</groupId>
     <artifactId>cxf-rt-frontend-jaxws</artifactId>
     <version>3.0.1</version>
   </dependency>
   <dependency>
     <groupId>junit</groupId>
     <artifactId>junit</artifactId>
     <version>4.10</version>
     <scope>test</scope>
   </dependency>
   <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-context</artifactId>
     <version>4.2.4.RELEASE</version>
   </dependency>
   <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-web</artifactId>
     <version>4.2.4.RELEASE</version>
   </dependency>
   <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-test</artifactId>
     <version>4.2.4.RELEASE</version>
   </dependency>
 </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8 </source>
          <target>1.8</target>
          <encoding>utf-8</encoding>
          <showWarnings>true</showWarnings>
        </configuration>
      </plugin>
      <!--运行tomcat7的方法：tomcat7:run-->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <!--指定端口-->
          <port>8080</port>
          <!--请求路径-->
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>

```
**配置web.xml**

``` 
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

    <web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!--cxfservlet配置-->
  <servlet>
    <servlet-name>cxfservlet</servlet-name>
    <servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>cxfservlet</servlet-name>
    <url-pattern>/ws/*</url-pattern>
  </servlet-mapping>
  <!--Spring容器的配置-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
</web-app>


```
**还是之前的接口和实现类**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092323.png)
**配置applicationContext.xml**

``` 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:cxf="http://cxf.apache.org/core"
       xmlns:jaxws="http://cxf.apache.org/jaxws"
       xmlns:jaxrs="http://cxf.apache.org/jaxrs"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://cxf.apache.org/core
        http://cxf.apache.org/schemas/core.xsd
        http://cxf.apache.org/jaxws
        http://cxf.apache.org/schemas/jaxws.xsd
        http://cxf.apache.org/jaxrs
        http://cxf.apache.org/schemas/jaxrs.xsd">

    <!--Spring整合cxf发布服务：关键点：
        1.服务地址
        2.服务类
        服务的完整访问地址：http://localhost:8080/ws/hello
        -->
    <jaxws:server address="/hello">
        <jaxws:serviceBean>
            <bean class="com.test.service.impl.HelloServiceImpl"></bean>
        </jaxws:serviceBean>
    </jaxws:server>

</beans>
```
在地址栏输入：http://localhost:8080/ws/hello?wsdl 测试 
注意问号是英文的问号
### **服务端**
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092324.png)
**接口还是用原来的代码不变**
**applicationContext.xml**

``` 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:cxf="http://cxf.apache.org/core"
       xmlns:jaxws="http://cxf.apache.org/jaxws"
       xmlns:jaxrs="http://cxf.apache.org/jaxrs"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://cxf.apache.org/core
        http://cxf.apache.org/schemas/core.xsd
        http://cxf.apache.org/jaxws
        http://cxf.apache.org/schemas/jaxws.xsd
        http://cxf.apache.org/jaxrs
        http://cxf.apache.org/schemas/jaxrs.xsd">

    <!--Spring整合cxf客户端配置：
        1.服务地址 http://localhost:8080/ws/hello
        2.服务的接口类型
        -->
    <jaxws:client
            id="helloService"
            serviceClass="com.test.service.HelloService"
            address="http://localhost:8080/ws/hello">
    </jaxws:client>

</beans>
```
**测试**

``` 
package com.test;

import com.test.service.HelloService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Client {

    //注入对象
    @Resource
    private HelloService helloService;

    /*public void setHelloService(HelloService helloService) {
        this.helloService = helloService;
    }*/
    @Test
    public void remote(){
        //查看接口的代理对象类型
        System.out.println(helloService.getClass());

        //远程访问服务端方法
        System.out.println(helloService.sayHello("jerry"));
    }
}

```
### **restful风格的webservice**
同一个URL地址可以通过指定不同的请求方式(eg. post,get,delete,put...)来对应  
Control中的不同的方法  
get    对应  查询操作
post   对应  新建操作
put    对应  更新操作
delete 对应  删除操作
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092325.png)  
![enter description
here](https://viabcde.github.io/images/2018-09-23/2018092326.png)
**配置pom.xml依赖包**

``` 
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.test</groupId>
  <artifactId>05_jaxrs_server</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>05_jaxrs_server</name>
  <dependencies>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxrs</artifactId>
      <version>3.0.1</version>
    </dependency>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-transports-http-jetty</artifactId>
      <version>3.0.1</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
    </dependency>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-client</artifactId>
      <version>3.0.1</version>
    </dependency>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-extension-providers</artifactId>
      <version>3.0.1</version>
    </dependency>
    <dependency>
      <groupId>org.codehaus.jettison</groupId>
      <artifactId>jettison</artifactId>
      <version>1.3.7</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.10</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8 </source>
          <target>1.8</target>
          <encoding>utf-8</encoding>
          <showWarnings>true</showWarnings>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>


```
**实体类**

``` 
package com.test.entity;

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="Car")
public class Car {
    private Integer id;
    private String carName;
    private Double price;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getCarName() {
        return carName;
    }

    public void setCarName(String carName) {
        this.carName = carName;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "id=" + id +
                ", carName='" + carName + '\'' +
                ", price=" + price +
                '}';
    }
}

```


``` 
package com.test.entity;

import javax.xml.bind.annotation.XmlRootElement;
import java.util.ArrayList;
import java.util.List;

@XmlRootElement(name="User")
public class User {
    private Integer id;
    private String name;
    private String city;
    private List<Car> cars = new ArrayList<Car>();

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public List<Car> getCars() {
        return cars;
    }

    public void setCars(List<Car> cars) {
        this.cars = cars;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", city='" + city + '\'' +
                ", cars=" + cars +
                '}';
    }
}

```
**接口及实现类**

``` 
package com.test.service;

import com.test.entity.User;

import javax.ws.rs.*;
import java.util.List;

/**
 * path访问本服务的路径、produces /* 表示支持任意处理请求完毕后返回给客户端的类型
 */
@Path("/userService")
@Produces("/*")
public interface IUserService {

    /**
     *path 连上本服务后 访问服务的方法的路径
     * Consumes 表示支持的请求格式
     */
    @POST
    @Path("/user")
    @Consumes({"application/xml","application/json"})
    public void saveUser(User user);

    @PUT
    @Path("/user")
    @Consumes({"application/xml","application/json"})
    public void updateUser(User user);

    @GET
    @Path("/user")
    @Produces({"application/xml","application/json"})
    public List<User> findAllUser();

    @GET
    @Path("/user")
    @Consumes({"application/xml"})
    @Produces({"application/xml","application/json"})
    public User findUserById(@PathParam("id")Integer id);

    @DELETE
    @Path("/user")
    @Consumes({"application/xml","application/json"})
    public void deleteUser(@PathParam("id") Integer id);
}
```


``` 
package com.test.service;

import com.test.entity.Car;
import com.test.entity.User;

import java.util.ArrayList;
import java.util.List;

public class UserServiceImpl implements IUserService{
    public void saveUser(User user) {
        System.out.println("save user"+user);
    }

    public void updateUser(User user) {
        System.out.println("update user"+user);
    }

    public List<User> findAllUser() {
        List<User> users = new ArrayList<User>();
        User user1 = new User();
        user1.setId(1);
        user1.setName("小明");
        user1.setCity("北京");

        List<Car> carList1 = new ArrayList<Car>();
        Car car1 = new Car();
        car1.setId(101);
        car1.setCarName("保时捷1");
        car1.setPrice(10000000d);
        carList1.add(car1);

        Car car2 = new Car();
        car2.setId(102);
        car2.setCarName("保时捷2");
        car2.setPrice(10000000d);
        carList1.add(car2);
        user1.setCars(carList1);
        users.add(user1);

        User user2 = new User();
        user2.setId(2);
        user2.setName("小小");
        user2.setCity("北京");
        users.add(user2);

        return users;
    }

    public User findUserById(Integer id) {
        if(id==1){
            User user1 = new User();
            user1.setId(1);
            user1.setName("小明");
            user1.setCity("北京");
            return user1;
        }
        return null;
    }

    public void deleteUser(Integer id) {
        System.out.println("delete user id:"+id);
    }
}

``` 


**测试启动服务**

``` 
package com.test;

import com.test.service.UserServiceImpl;
import org.apache.cxf.interceptor.LoggingInInterceptor;
import org.apache.cxf.interceptor.LoggingOutInterceptor;
import org.apache.cxf.jaxrs.JAXRSServerFactoryBean;

public class Server {
    public static void main(String[] args) {
        //创建发布服务的工厂
        JAXRSServerFactoryBean factory = new JAXRSServerFactoryBean();

        //设置服务地址
        factory.setAddress("http://localhost:8001/ws");

        //设置服务类
        factory.setServiceBean(new UserServiceImpl());

        //添加日志输入输出拦截器
        factory.getInInterceptors().add(new LoggingInInterceptor());
        factory.getOutInterceptors().add(new LoggingOutInterceptor());

        //发布服务
        factory.create();

        System.out.println("发布服务成功，端口8001");
    }
}

```
**客户端**
**配置依赖**

``` 
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.test</groupId>
    <artifactId>06_jaxrs_client</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>06_jaxrs_client</name>
    <dependencies>
        <dependency>
            <groupId>org.apache.cxf</groupId>
            <artifactId>cxf-rt-frontend-jaxrs</artifactId>
            <version>3.0.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.cxf</groupId>
            <artifactId>cxf-rt-transports-http-jetty</artifactId>
            <version>3.0.1</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.12</version>
        </dependency>
        <dependency>
            <groupId>org.apache.cxf</groupId>
            <artifactId>cxf-rt-rs-client</artifactId>
            <version>3.0.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.cxf</groupId>
            <artifactId>cxf-rt-rs-extension-providers</artifactId>
            <version>3.0.1</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jettison</groupId>
            <artifactId>jettison</artifactId>
            <version>1.3.7</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.2</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>utf-8</encoding>
                    <showWarnings>true</showWarnings>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>


```

**实体类**

``` 
package com.test.entity;

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="Car")
public class Car {
    private Integer id;
    private String carName;
    private Double price;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getCarName() {
        return carName;
    }

    public void setCarName(String carName) {
        this.carName = carName;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "id=" + id +
                ", carName='" + carName + '\'' +
                ", price=" + price +
                '}';
    }
}

```

==

``` 
package com.test.entity;

import javax.xml.bind.annotation.XmlRootElement;
import java.util.ArrayList;
import java.util.List;

@XmlRootElement(name="User")
public class User {
    private Integer id;
    private String name;
    private String city;
    private List<Car> cars = new ArrayList<Car>();

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public List<Car> getCars() {
        return cars;
    }

    public void setCars(List<Car> cars) {
        this.cars = cars;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", city='" + city + '\'' +
                ", cars=" + cars +
                '}';
    }
}

```

**客户端测试**

``` 
package com.test;

import com.test.entity.User;
import org.apache.cxf.jaxrs.client.WebClient;
import org.junit.Test;

public class Client {

    @Test
    public void testSave(){
        //通过WebClient对象远程调用服务端
        WebClient.create("http://localhost:8001/ws/userService/user").post(new User());
    }
}

```
### **resful风格的webservice整合Spring**
**配置依赖**

``` 
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.test</groupId>
  <artifactId>07_jaxrs_spring_server</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>07_jaxrs_spring_server</name>
  <dependencies>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxrs</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
    </dependency>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-client</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-extension-providers</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.codehaus.jettison</groupId>
      <artifactId>jettison</artifactId>
      <version>1.3.7</version>
    </dependency>
<!--    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-transports-http-jetty</artifactId>
      <version>3.0.1</version>
    </dependency>-->

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>utf-8</encoding>
          <showWarnings>true</showWarnings>
        </configuration>
      </plugin>
      <!--运行tomcat7的方法：tomcat7:run-->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <!--指定端口-->
          <port>8080</port>
          <!--请求路径-->
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>


```
**实体类和服务类和未整合的一样**
**applicationContext.xml**

``` 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:cxf="http://cxf.apache.org/core"
       xmlns:jaxws="http://cxf.apache.org/jaxws"
       xmlns:jaxrs="http://cxf.apache.org/jaxrs"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://cxf.apache.org/core
        http://cxf.apache.org/schemas/core.xsd
        http://cxf.apache.org/jaxws
        http://cxf.apache.org/schemas/jaxws.xsd
        http://cxf.apache.org/jaxrs
        http://cxf.apache.org/schemas/jaxrs.xsd">

    <!--Spring整合cxf发布基于restful风格的服务：关键点：
        1.服务地址
        2.服务类
        服务的完整访问地址：http://localhost:8080/ws/hello
        -->
    <jaxrs:server address="/userService">
        <jaxrs:serviceBeans>
            <bean class="com.test.service.UserServiceImpl"></bean>
        </jaxrs:serviceBeans>
    </jaxrs:server>

</beans>
```

**客户端**
**依赖**

``` 
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.test</groupId>
  <artifactId>08_jaxrs_spring_client</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>08_jaxrs_spring_client</name>
  <dependencies>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxrs</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
    </dependency>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-client</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-extension-providers</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.codehaus.jettison</groupId>
      <artifactId>jettison</artifactId>
      <version>1.3.7</version>
    </dependency>
    <!--    <dependency>
          <groupId>org.apache.cxf</groupId>
          <artifactId>cxf-rt-transports-http-jetty</artifactId>
          <version>3.0.1</version>
        </dependency>-->

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>4.2.4.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>utf-8</encoding>
          <showWarnings>true</showWarnings>
        </configuration>
      </plugin>
      <!--运行tomcat7的方法：tomcat7:run-->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <!--指定端口-->
          <port>8080</port>
          <!--请求路径-->
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>


```

**客户端测试**

``` 
package com.test;

import com.test.entity.User;
import org.apache.cxf.jaxrs.client.WebClient;
import org.junit.Test;

public class Client {

    @Test
    public void testSave(){
        //通过WebClient对象远程调用服务端
        //第一个/userService是在spring的配置文件中配置的
        //第二个/userService是在impl类中定义的
        WebClient.create("http://localhost:8080/ws/userService/userService/user").post(new User());
    }
}

```