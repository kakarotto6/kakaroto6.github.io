--- 
layout: post
title: SpringBoot的配置文件application.properties
date: 2018-11-13
tags: java
---
### **SpringBoot的配置文件**  
用来配置数据库连接、日志相关配置等  
自定义属性与加载  
我们在使用Spring Boot的时候，通常也需要定义一些自己使用的属性，我们可以如下方式直接定义：  
``` 
com.didispace.blog.name=程序猿DD
com.didispace.blog.title=Spring Boot教程
``` 
然后通过@Value("${属性名}")注解来加载对应的配置属性，具体如下：  
``` 
@Component
public class BlogProperties {
    @Value("${com.didispace.blog.name}")
    private String name;
    @Value("${com.didispace.blog.title}")
    private String title;
    // 省略getter和setter
}
```
按照惯例，通过单元测试来验证BlogProperties中的属性是否已经根据配置文件加载了。  
``` 
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(Application.class)
public class ApplicationTests {
	@Autowired
	private BlogProperties blogProperties;
	@Test
	public void getHello() throws Exception {
		Assert.assertEquals(blogProperties.getName(), "程序猿DD");
		Assert.assertEquals(blogProperties.getTitle(), "Spring Boot教程");
	}
}
```
#### **配置文件里参数间可以互相引用**  
在application.properties中的各个参数之间也可以直接引用来使用，就像下面的设置：  
``` 
com.didispace.blog.name=程序猿DD
com.didispace.blog.title=Spring Boot教程
com.didispace.blog.desc=${com.didispace.blog.name}正在努力写《${com.didispace.blog.title}》
```
com.didispace.blog.desc参数引用了上文中定义的name和title属性，最后该属性的值就是程序猿DD正在努力写《Spring Boot教程》。
### **配置文件的使用方法**
#### **使用随机数**
在一些情况下，有些参数我们需要希望它不是一个固定的值，比如密钥、服务端口等。Spring Boot的属性配置文件中可以通过${random}来产生int值、long值或者string字符串，来支持属性的随机值。  
#### **随机字符串**  
com.didispace.blog.value=${random.value}  
#### **随机int**  
com.didispace.blog.number=${random.int}  
#### **随机long**  
com.didispace.blog.bignumber=${random.long}
#### **10以内的随机数**  
com.didispace.blog.test1=${random.int(10)}  
#### **10-20的随机数**  
com.didispace.blog.test2=${random.int[10,20]}  