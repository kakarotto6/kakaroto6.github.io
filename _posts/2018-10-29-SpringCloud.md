--- 
layout: post
title: SpringCloud
date: 2018-10-29
tags: SpringCloud
---
## **SpringCloud分布式开发de五大神兽**
**•服务发现——Netflix Eureka**  
**•服务网关——Netflix Zuul**  
**•客服端负载均衡——Netflix Ribbon**  
**•断路器——Netflix Hystrix**  
**•分布式配置——Spring Cloud Config**  

### **Eureka服务治理体系**
用来实现微服务实例自动注册和发现  
**服务提供者**向**服务注册中心**注册服务    
**服务消费者**向**服务注册中心**获取服务列表去寻找服务者
服务列表（包括服务的主机与端口号、服务版本号、通讯协议等一些附加信息）   
**服务注册**  
在服务治理框架中，通常都会构建一个注册中心，每个服务单元向注册中心登记自己提供的服务，包括服务的主机与端口号、服务版本号、通讯协议等一些附加信息。注册中心按照服务名分类组织服务清单，同时还需要以心跳检测的方式去监测清单中的服务是否可用，若不可用需要从服务清单中剔除，以达到排除故障服务的效果  
 **服务发现**  
在服务治理框架下，服务间的调用不再通过指定具体的实例地址来实现，而是通过服务名发起请求调用实现。服务调用方通过服务名从服务注册中心的服务清单中获取服务实例的列表清单，通过指定的负载均衡策略取出一个服务实例位置来进行服务调用。
## **Zuul路由网关**
### **1. 代理：**
Zuul提供外部的请求转发到具体的微服务实例中的服务  
### **2. 路由：**
Zuul可以对外部访问实现统一的入口  
### **3. 过滤：**
Zuul可以对外部访问进行干预，如请求校验、服务聚合等  
### **4. Zuul需要配合Eureka使用，**
需要在Eureka中注册并获得其他微服务的信息  
### **5. 理解：**
Zuul就像大楼的保安，可以请他找人（代理），找的人在外面叫什么名字（路由），准不准你进楼（过滤）。因为保安属于物业公司，所以保安要在物业公司注册，所获得的信息也来源于物业公司（与Eureka的关系）。   
 [Zuul的源码](http://github.com/Netflix/zuul )    
### **zuul的基本配置**
**第一步：构建新的Zuul模块并在pom.xml中加入依赖（Zuul和Eureka必须同时加入）；**  
``` 
<dependencies>
        <!-- zuul路由网关 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zuul</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        <!-- actuator监控 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!-- hystrix容错 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <!-- 日常标配 -->
        <dependency>
            <groupId>com.wangcw.springcloud</groupId>
            <artifactId>springclouddemo-api</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <!-- 热部署插件 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```

**第二步：新建application.yml文件并配置（一定要向Eureka注册，因为Zuul本身也是一个微服务）；**

``` 
server: 
  port: 6001
spring: 
  application:
    name: microservicecloud-zuul-gateway
 
eureka: 
  client: #客户端注册进eureka服务列表内
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  instance:
    instance-id: gateway-zull-6001
    prefer-ip-address: true 
#配置微服务的info信息
info:
  app.name: wangcw-dept-provider
  company.name: www.amoins.top
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```
**第三步： 修改hosts文件（非必须，不过能更好看出效果）**

``` 
127.0.0.1		zuul.com
```

**第四步：创建主启动类，并加入@EnableZuulProxy注解**

``` 
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
@EnableZuulProxy
public class Zuul_6001_App
{
	public static void main(String[] args)
	{
		SpringApplication.run(Zuul_6001_App.class, args);
	}
}

```

**第五步：启动测试，访问规则：步骤3中指定映射域名 + 端口号 + 微服务名称 + 访问路径。**
   例子：http://zuul.com:6001/springclouddemo-provider-dept/provider/dept/list  
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101024.png) 
### **路由访问映射规则**

    服务名映射和统一公共前缀 : 当不想暴露真实的服务名时，可以对服务名进行映射，只需在application.yml中配置即可。配置如下：

``` 
zuul:
   prefix: /wangcw        #设置统一公共前缀
   ignored-services: "*"  #对外隐藏所有的服务名，可以使用通配符，配置之后通过这个微服务名访问就失效了，外部无法访问，但是微服务内部依然可用
   routes:
     mydept.serviceId: springclouddemo-provider-dept
     mydept.path: /zull-dept/**          # 访问/zull-dept/*相当于访问微服务【springclouddemo-provider-dept】
     #最终访问路径变成了 zuul.com:6001/wangcw/zull-dept/* 
```
     
经过上述配置，访问路径变成  http://zuul.com:6001/wangcw/zull-dept/provider/dept/list
测试结果：
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101025.png) 
注：因为Zuul是针对外部访问管理的，所以配置了隐藏的服务，在系统中其他模块进行服务名访问时依然可以正常运行的。
## **ribbon**
负载均衡：在微服务之前，客户端与服务端之间是存在专门的负载均衡的中间硬件的
微服务出现后，把负载均衡的功能以库的方式集成到客户端的进程中称为软负载均衡
## **Netflix hystrix**
想象这样一个情况：一个电子商务网站因为在黑色周五发生过载，因为过多访问负载，在线支付延迟半天都没有响应，用户的支付因为太多并发请求导致响应时间过长，在等待很长时间以后最终失败。这样的错误导致退出购物车的用户重新刷新或者再次尝试支付，更加增加了服务器的负载，如同滚雪球一样爆炸，导致更多长时间等待线程和网络拥塞。  
hystrix用来设计隔离访问远程系统端点或微服务等，防止级联爆炸式的失败    
如果它在一段时间内侦测到许多类似的错误，会强迫其以后的多个调用快速失败，也就是不让它们再次访问远程服务器了  

## **spring cloud config来统一管理配置文件**  
和Eureka有点你相似，即我们先把配置文件推送到git等服务中，当我们需要配置文件 ConfigClient系统可以通过ConfigServer去获取配置文件

### **分布式链路跟踪 Sleuth 与 Zipkin**
随着业务发展，系统拆分导致系统调用链路愈发复杂一个前端请求可能最终需要调用很多次后端服务才能完成，当整个请求变慢或不可用时，我们是无法得知该请求是由某个或某些后端服务引起的，这时就需要解决如何快读定位服务故障点，以对症下药。于是就有了分布式系统调用跟踪的诞生。  
**一般的，一个分布式服务跟踪系统主要由三部分构成**  
数据收集  
数据存储  
数据展示  
每个 trace 中会调用若干个服务，为了记录调用了哪些服务，以及每次调用的消耗时间等信息，在每次调用服务时，埋入一个调用记录，称为一个 span。这样，若干个有序的 span 就组成了一个 trace。  
附带上 span 中的请求的服务的名称，响应时间，以及请求成功与否等信息，就可以在发生问题的时候，找到异常的服务；根据历史数据，还可以从系统整体层面分析出哪里性能差，定位性能优化的目标。  
可以用于  
**耗时分析:** 通过 Sleuth 可以很方便的了解到每个采样请求的耗时，从而分析出哪些服务调用比较耗时;  
**可视化错误:** 对于程序未捕捉的异常，可以通过集成 Zipkin 服务界面上看到;  
**链路优化:** 对于调用比较频繁的服务，可以针对这些服务实施一些优化措施。  
### **BUS消息总线技术**
**路由(routing)**是指分组从源到目的地时，决定端到端路径的网络范围的进程
消息代理中间件可以将消息路由到一个或多个目的地，实现对配置信息的实时更新
### **RabbitMQ作为消息的总代理**  
config-client修改pom.xml增加spring-cloud-starter-bus-amqp模块  

``` 
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

在配置文件中增加关于RabbitMQ的连接和用户信息  
``` 
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=springcloud
spring.rabbitmq.password=123456
```
启动config-server,再启动两个config-clien(分别在不同的端口上)  
可以在config-client-eureka中的控制台中看到如下内容，在启动时候，客户端程序多了一个/bus/refresh请求。  
``` 
o.s.b.a.e.mvc.EndpointHandlerMapping     : Mapped "{[/bus/refresh],methods=[POST]}" onto public void org.springframework.cloud.bus.endpoint.RefreshBusEndpoint.refresh(java.lang.String)
```

•先访问两个config-client的/from请求，会返回当前config-repo/didispace-dev.properties中的from属性。  
•接着，我们修改config-repo/didispace-dev.properties中的from属性值，并发送POST请求到其中的一个/bus/refresh。  
•最后，我们再分别访问启动的两个config-client的/from请求，此时这两个请求都会返回最新的config-repo/didispace-dev.properties中的from属性。  
上面已经能够通过Spring Cloud Bus来实时更新总线上的属性配置了。  
### **消息驱动的微服务**
