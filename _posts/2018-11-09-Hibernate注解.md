--- 
layout: post
title: Hibernate注解
date: 2018-11-18
tags: Hibernate
---
### **@Transaction**
![enter description
here](https://viabcde.github.io/images/blog/2018092832.png)  
### **指定时间的精度@Temporal**
![enter description
here](https://viabcde.github.io/images/blog/2018092833.png)  
### **枚举类型注解@Enumerated**
**写好枚举类**
![enter description
here](https://viabcde.github.io/images/blog/2018092834.png)  
**引用枚举类并加上注解**
![enter description
here](https://viabcde.github.io/images/blog/2018092835.png)  
**EnumType.ORDINAL这是按枚举类型的下标插入表，而不是按他的值插入表**
![enter description
here](https://viabcde.github.io/images/blog/2018092836.png)  
### **@Table**
当表名和类名不一致 还需在类上加上 @Table 指明 数据库中对应的表的表名
![enter description
here](https://viabcde.github.io/images/blog/2018092837.png)  
###  **ID生成策略**
在xml.让所使用的数据库选择ID策略（在mysql中，使用uuid,此时的实体类id改为String类型,如果是native（自动增长）则类型是int）选native和uuid方便数据库跨平台
![enter description
here](https://viabcde.github.io/images/blog/2018092838.png)  
最后在插入数据时，不要求设置ID,只需要用户提供name 和age即可
Annotation版：
![enter description
here](https://viabcde.github.io/images/blog/2018092839.png)  
同样，最后xml配置版的需要在配置文件加上映射 注解版则不需要
![enter description
here](https://viabcde.github.io/images/blog/2018092840.png)  
### **联合主键**
#### **编写联合主键类**
![enter description
here](https://viabcde.github.io/images/blog/2018092841.png)  
重写equal和hashcode原因：数据库区分实体使用的是主键，但在内存中也需要使用类似的hashcode算法来区分不同的实体
#### **修改实体类**
![enter description
here](https://viabcde.github.io/images/blog/2018092842.png)  

#### **修改xml映射文件**
![enter description
here](https://viabcde.github.io/images/blog/2018092843.png)  
### **Annotation版联合主键**
![enter description
here](https://viabcde.github.io/images/blog/2018092844.png)  

