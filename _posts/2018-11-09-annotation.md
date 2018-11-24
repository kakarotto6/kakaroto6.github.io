--- 
layout: post
title: Annotation
date: 2018-11-12
tags: java
---
三个基本的Annotation如下：  
@Override         限定必须重写父类的方法  
@Deprecated     标示已过时  
@SuppressWarnings     抑制编译器警告  
### **自定义anotation**
1.标记Annotation： 一个没有成员定义的Annotation类型被称为标记。这种Annotation仅使用自身的存在与否来为我们提供信息。如前面介绍的@Override。

``` 
/**
 * 定义一个Annotation
 */
public @interface Login {
  
}

class LoginTest{
    /**
     * 使用Annotation
     */
    @Login
　　 public void login(){
        
    }
}
```
2.元数据Annotation：那些包含成员变量的Annotation，因为它们可接受更多元数据，所以也被称为元数据Annotation。

``` 
/**
 * 定义一个注解
 */
public @interface Login {
    //定义两个成员变量
    //以default为两个成员变量指定初始值
    String username() default "zhangsan";
    String password() default "123456";
}

class LoginTest{
    /**
     * 使用注解
　　　* 因为它的成员变量有默认值，所以可以无须为成员变量指定值，而直接使用默认值
     */
    @Login
    public void login(){
        
    }
}
```

