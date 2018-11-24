--- 
layout: post
title: SpringBoot核心组件
date: 2018-11-14
tags: java
---
###  **核心组件starter**
Starter主要用来简化依赖用的。比如我们之前做 MVC时要引入日志组件，那么需要去找到log4j的版本，然后引入，现在有了Starter之后，直接用这个之后，log4j就自动引入了，也不用关心版本这些问题。  
###  **核心组件actuator**
主要作用是监控与管理，它就是：spring-boot-starter-actuator  
只需要在pom.xml的dependencies节点中，新增spring-boot-starter-actuator的依赖即可，具体如下：  

``` 
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
模块中已经实现的一些原生端点。
如果根据端点的作用来说，我们可以原生端点分为三大类：    
应用配置类：获取应用程序中加载的应用配置、环境变量、自动化配置报告等配置类信息    
度量指标类：获取应用程序运行过程中用于监控的度量指标，比如：内存信息、线程池信息、HTTP请求统计等  
操作控制类：提供了对应用的关闭等操作类功能  
## **应用配置类**  
Spring Boot让代码变得简洁，但是应用实例创建和依赖关系等信息都被离散到了各个配置类的注解上，使得分析应用中资源和实例的各种关系变得困难。而这类端点就可以帮助获取关于Spring 应用配置内容的详细报告，比如：自动化配置的报告、Bean创建的报告、环境属性的报告等。  
### **/autoconfig**
**作用：**获取应用的自动化配置报告，包括所有自动化配置的候选项。同时还列出了每个候选项自动化配置的各个先决条件是否满足。  
**使用场景：**帮助找到自动化配置为什么没有生效的具体原因。
**该报告内容将自动化配置内容分为两部分：**      
positiveMatches中返回的是条件匹配成功的自动化配置    
negativeMatches中返回的是条件匹配不成功的自动化配置     
``` 
{
    "positiveMatches": { // 条件匹配成功的
        "EndpointWebMvcAutoConfiguration": [
            {
                "condition": "OnClassCondition",
                "message": "@ConditionalOnClass classes found: javax.servlet.Servlet,org.springframework.web.servlet.DispatcherServlet"
            },
            {
                "condition": "OnWebApplicationCondition",
                "message": "found web application StandardServletEnvironment"
            }
        ],
        ...
    },
    "negativeMatches": {  // 条件不匹配成功的
        "HealthIndicatorAutoConfiguration.DataSourcesHealthIndicatorConfiguration": [
            {
                "condition": "OnClassCondition",
                "message": "required @ConditionalOnClass classes not found: org.springframework.jdbc.core.JdbcTemplate"
            }
        ],
        ...
    }
}
```

每个自动化配置候选项中都有一系列的条件，比如上面没有成功匹配的HealthIndicatorAutoConfiguration.DataSourcesHealthIndicatorConfiguration配置。
它的先决条件就是需要在工程中包含org.springframework.jdbc.core.JdbcTemplate类，由于没有引入相关的依赖，它就不会执行自动化配置内容。  所以，当发现有一些配置没有生效时，就可以通过该端点来查看没有生效的原因。    

### **/beans**
**作用：**该端点用来获取应用上下文中创建的所有Bean  

``` 
[
    {
        "context": "hello:dev:8881",
        "parent": null,
        "beans": [
            {
                "bean": "org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration$DispatcherServletConfiguration",
                "scope": "singleton",
                "type": "org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration$DispatcherServletConfiguration$$EnhancerBySpringCGLIB$$3440282b",
                "resource": "null",
                "dependencies": [
                    "serverProperties",
                    "spring.mvc.CONFIGURATION_PROPERTIES",
                    "multipartConfigElement"
                ]
            },
            {
                "bean": "dispatcherServlet",
                "scope": "singleton",
                "type": "org.springframework.web.servlet.DispatcherServlet",
                "resource": "class path resource [org/springframework/boot/autoconfigure/web/DispatcherServletAutoConfiguration$DispatcherServletConfiguration.class]",
                "dependencies": []
            }
        ]
    }
]
```
如上示例中，可以看到每个bean都包含了下面这几个信息：  
bean：Bean的名称  
scope：Bean的作用域  
type：Bean的Java类型  
reource：class文件的具体路径  
dependencies：依赖的Bean名称  
### **/configprops**
**作用：**用来获取应用中配置的属性信息报告。  
从下面该端点返回示例片段中，返回了关于该短信的配置信息：  
prefix属性代表了属性的配置前缀  
properties代表了各个属性的名称和值  
所以，我们可以通过该报告来看到各个属性的配置路径，比如我们要关闭该端点，就可以通过使用endpoints.configprops.enabled=false来完成设置
``` 
{
    "configurationPropertiesReportEndpoint": {
        "prefix": "endpoints.configprops",
        "properties": {
            "id": "configprops",
            "sensitive": true,
            "enabled": true
        }
    },
    ...
}
```

### **/env**
**作用：**该端点与/configprops不同，它用来获取应用所有可用的环境属性报告。包括：环境变量、JVM属性、应用的配置配置、命令行中的参数    
从下面该端点返回的示例片段中，可以看到它不仅返回了应用的配置属性，还返回了系统属性、环境变量等丰富的配置信息，其中也包括了应用还没有没有使用的配置。  
**用途：**帮助看当前应用可以加载的配置信息，并配合@ConfigurationProperties将它们引入到应用来使用。  
为了配置属性的安全，对于一些类似密码等敏感信息，该端点都会进行隐私保护，但是需要让属性名中包含：password、secret、key这些关键词，这样该端点在返回它们的时候会使用" *  "来替代实际的属性值
``` 
{
    "profiles": [
        "dev"
    ],
    "server.ports": {
        "local.server.port": 8881
    },
    "servletContextInitParams": {
        
    },
    "systemProperties": {
        "idea.version": "2016.1.3",
        "java.runtime.name": "Java(TM) SE Runtime Environment",
        "sun.boot.library.path": "C:\\Program Files\\Java\\jdk1.8.0_91\\jre\\bin",
        "java.vm.version": "25.91-b15",
        "java.vm.vendor": "Oracle Corporation",
        ...
    },
    "systemEnvironment": {
        "configsetroot": "C:\\WINDOWS\\ConfigSetRoot",
        "RABBITMQ_BASE": "E:\\tools\\rabbitmq",
        ...
    },
    "applicationConfig: [classpath:/application-dev.properties]": {
        "server.port": "8881"
    },
    "applicationConfig: [classpath:/application.properties]": {
        "server.port": "8885",
        "spring.profiles.active": "dev",
        "info.app.name": "spring-boot-hello",
        "info.app.version": "v1.0.0",
        "spring.application.name": "hello"
    }
}

```

### **/mappings**
**作用：**该端点用来返回所有Spring MVC的控制器映射关系报告  
从下面的示例片段中，可以看该报告的信息  
与我们在启用Spring MVC的Web应用时输出的日志信息类似，其中bean属性标识了该映射关系的请求处理器，method属性标识了该映射关系的具体处理类和处理函数  

``` 
{
    "/webjars/**": {
        "bean": "resourceHandlerMapping"
    },
    "/**": {
        "bean": "resourceHandlerMapping"
    },
    "/**/favicon.ico": {
        "bean": "faviconHandlerMapping"
    },
    "{[/hello]}": {
        "bean": "requestMappingHandlerMapping",
        "method": "public java.lang.String com.didispace.web.HelloController.index()"
    },
    "{[/mappings || /mappings.json],methods=[GET],produces=[application/json]}": {
        "bean": "endpointHandlerMapping",
        "method": "public java.lang.Object org.springframework.boot.actuate.endpoint.mvc.EndpointMvcAdapter.invoke()"
    },
    ...
}
```

### **/info**
**作用：**用来返回应用自定义的信息。默认情况下，该端点只会返回一个空的json内容。我们可以在application.properties配置文件中通过info前缀来设置一些属性，比如下面这样：  

info.app.name=spring-boot-hello  
info.app.version=v1.0.0  
再访问/info端点，我们可以得到下面的返回报告，其中就包含了上面我们在应用自定义的两个参数。  

``` 
{
    "app": {
        "name": "spring-boot-hello",
        "version": "v1.0.0"
    }
}
```

## **度量指标类**  
应用配置类端点所提供的信息报告在应用启动时都已经确定了其返回内容，是一个静态报告。  
而度量指标类端点提供的报告内容则是动态变化的，这些端点提供了应用程序运行中的信息  
比如：内存使用情况、HTTP请求统计、外部资源指标等。  
这些端点对于我们构建微服务架构中的监控系统非常有帮助，由于Spring Boot应用自身实现了这些端点，所以可以利用它们来收集想要的信息，以制定出各种自动化策略。    

### **/metrics**
**作用：**该端点用来返回当前应用的比如：内存信息、线程信息、垃圾回收信息等
``` 
{
  "mem": 541305,
  "mem.free": 317864,
  "processors": 8,
  "instance.uptime": 33376471,
  "uptime": 33385352,
  "systemload.average": -1,
  "heap.committed": 476672,
  "heap.init": 262144,
  "heap.used": 158807,
  "heap": 3701248,
  "nonheap.committed": 65856,
  "nonheap.init": 2496,
  "nonheap.used": 64633,
  "nonheap": 0,
  "threads.peak": 22,
  "threads.daemon": 20,
  "threads.totalStarted": 26,
  "threads": 22,
  "classes": 7669,
  "classes.loaded": 7669,
  "classes.unloaded": 0,
  "gc.ps_scavenge.count": 7,
  "gc.ps_scavenge.time": 118,
  "gc.ps_marksweep.count": 2,
  "gc.ps_marksweep.time": 234,
  "httpsessions.max": -1,
  "httpsessions.active": 0,
  "gauge.response.beans": 55,
  "gauge.response.env": 10,
  "gauge.response.hello": 5,
  "gauge.response.metrics": 4,
  "gauge.response.configprops": 153,
  "gauge.response.star-star": 5,
  "counter.status.200.beans": 1,
  "counter.status.200.metrics": 3,
  "counter.status.200.configprops": 1,
  "counter.status.404.star-star": 2,
  "counter.status.200.hello": 11,
  "counter.status.200.env": 1
}

```
从上面的示例中，有这些重要的度量值：  
系统信息：包括处理器数量processors、运行时间uptime和instance.uptime、系统平均负载systemload.average     
mem. * ：内存概要信息，包括分配给应用的总内存数量以及当前空闲的内存数量。这些信息来自java.lang.Runtime    
heap.* ：堆内存使用情况。来自java.lang.management.MemoryMXBean接口中getHeapMemoryUsage方法获取的java.lang.management.MemoryUsage。  
nonheap.* ：非堆内存使用情况。来自java.lang.management.MemoryMXBean接口中getNonHeapMemoryUsage方法获取的java.lang.management.MemoryUsage。  
threads.* ： 线程使用情况，包括线程数、守护线程数（daemon）、线程峰值（peak）等，这些数据均来自java.lang.management.ThreadMXBean。  
classes.* ：应用加载和卸载的类统计。这些数据均来自java.lang.management.ClassLoadingMXBean。  
gc.* ：垃圾收集器的详细信息，包括垃圾回收次数gc.ps_scavenge.count、垃圾回收消耗时间gc.ps_scavenge.time、标记-清除算法的次数gc.ps_marksweep.count、标记-清除算法的消耗时间gc.ps_marksweep.time。这些数据均来自java.lang.management.GarbageCollectorMXBean。 
httpsessions.* ：Tomcat容器的会话使用情况。包括最大会话数httpsessions.max和活跃会话数httpsessions.active。该度量指标信息仅在引入了嵌入式Tomcat作为应用容器的时候才会提供。  
gauge.* ：HTTP请求的性能指标之一，它主要用来反映一个绝对数值。比如上面示例中的gauge.response.hello: 5，它表示上一次hello请求的延迟时间为5毫秒。  
counter.* ：HTTP请求的性能指标之一，它主要作为计数器来使用，记录了增加量和减少量。如上示例中counter.status.200.hello: 11，它代表了hello请求返回200状态的次数为11。  
对于gauge.* 和counter.* 的统计，这里有一个特殊的内容请求star-star，它代表了对静态资源的访问。这两类度量指标非常有用，我们不仅可以使用它默认的统计指标，还可以在程序中轻松的增加自定义统计值。只需要通过注入org.springframework.boot.actuate.metrics.CounterService和org.springframework.boot.actuate.metrics.GaugeService来实现自定义的统计指标信息。比如：我们可以像下面这样自定义实现对hello接口的访问次数统计。    

``` 
@RestController
public class HelloController {
    
    @Autowired
    private CounterService counterService;

    @RequestMapping("/hello")
    public String greet() {
        counterService.increment("didispace.hello.count");
        return "";
    }
  
}
```

### **/metrics**
**作用：**提供应用运行状态的完整度量指标报告，对于监控系统中的各项监控功能，它们的监控内容、数据收集频率都不同，如果我们每次都通过全量获取报告的方式来收集，略显粗暴。所以，我们还可以通过/metrics/{name}接口来更细粒度的获取度量信息，比如我们可以通过访问/metrics/mem.free来获取当前可用内存数量。  
### **/health**
用来获取应用的各类健康指标信息。  
在spring-boot-starter-actuator模块中自带实现了一些常用资源的健康指标检测器。这些检测器通过HealthIndicator接口实现，并且会根据依赖关系的引入实现自动化装配，比如用于检测磁盘的DiskSpaceHealthIndicator、检测DataSource连接是否可用的DataSourceHealthIndicator等。有时候，我们可能还会用到一些Spring Boot的Starter POMs中还没有封装的产品来进行开发，比如：当使用RocketMQ作为消息代理时，由于没有自动化配置的检测器，所以需要自己来实现一个用来采集健康信息的检测器。比如，我们可以在Spring Boot的应用中，为org.springframework.boot.actuate.health.HealthIndicator接口实现一个对RocketMQ的检测器类：

``` 
@Component
public class RocketMQHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        int errorCode = check();
        if (errorCode != 0) {
          return Health.down().withDetail("Error Code", errorCode).build();
        }
        return Health.up().build();
    }

  	private int check() {
     	// 对监控对象的检测操作
  	}
}
```

通过重写health()函数来实现健康检查，返回的Heath对象中，共有两项内容，一个是状态信息，除了该示例中的UP与DOWN之外，还有UNKNOWN和OUT_OF_SERVICE，可以根据需要来实现返回；还有一个详细信息，采用Map的方式存储，在这里通过withDetail函数，注入了一个Error Code信息，也可以填入一下其他信息，比如，检测对象的IP地址、端口等。重新启动应用，并访问/health接口，我们在返回的JSON字符串中，将会包含了如下信息：  

``` 
"rocketMQ": {
  "status": "UP"
}
```

### **/dump**
用来暴露程序运行中的线程信息。它使用java.lang.management.ThreadMXBean的dumpAllThreads方法来返回所有含有同步信息的活动线程详情。  

### **/trace**
用来返回基本的HTTP跟踪信息。默认情况下，跟踪信息的存储采用org.springframework.boot.actuate.trace.InMemoryTraceRepository实现的内存方式，始终保留最近的100条请求记录。它记录的内容格式如下：  

``` 
[
    {
        "timestamp": 1482570022463,
        "info": {
            "method": "GET",
            "path": "/metrics/mem",
            "headers": {
                "request": {
                    "host": "localhost:8881",
                    "connection": "keep-alive",
                    "cache-control": "no-cache",
                    "user-agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36",
                    "postman-token": "9817ea4d-ad9d-b2fc-7685-9dff1a1bc193",
                    "accept": "*/*",
                    "accept-encoding": "gzip, deflate, sdch",
                    "accept-language": "zh-CN,zh;q=0.8"
                },
                "response": {
                    "X-Application-Context": "hello:dev:8881",
                    "Content-Type": "application/json;charset=UTF-8",
                    "Transfer-Encoding": "chunked",
                    "Date": "Sat, 24 Dec 2016 09:00:22 GMT",
                    "status": "200"
                }
            }
        }
    },
    ...
]
```

## **操作控制类**  
由于介绍的所有端点都是用来反映应用自身的属性或是运行中的状态，相对于操作控制类端点没有那么敏感，所以他们默认都是启用的。    
而操作控制类端点拥有更强大的控制能力，如果要使用它们的话，需要通过属性来配置开启。  
在原生端点中，只提供了一个用来关闭应用的端点：/shutdown。我们可以通过如下配置开启它：    
endpoints.shutdown.enabled=true      
在配置了上述属性后，只需要访问应用的/shutdown端点就能实现关闭该应用的远程操作。由于开放关闭应用的操作本身是一件非常危险的事，所以真正在线上使用的时候，我们需要对其加入一定的保护机制，比如：定制Actuator的端点路径、整合Spring Security进行安全校验等。

