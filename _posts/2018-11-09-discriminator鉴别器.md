--- 
layout: post
title: discriminator鉴别器
date: 2018-11-19
tags: Mybatis
---
**discriminator鉴别器（一）**  
有时一个单独的数据库查询也许返回很多不同（但是希望有些关联）数据类型的结果集。
鉴别器元素就是被设计来处理这个情况的，还有包括类的继承层次结构。鉴别器非常容易理
解，它很像 Java的 switch   
定义鉴别器指定了 column 和 javaType 属性  
列是 MyBatis 查找比较值的地方    
JavaType是需要被用来保证等价测试的合适类型（尽管字符串在很多情形下都会有用）。比如：

``` 
<resultMap id="vehicleResult" type="Vehicle">
  <id property=”id” column="id" />
  <result property="vin" column="vin"/>
  <result property="year" column="year"/>
  <result property="make" column="make"/>
  <result property="model" column="model"/>
  <result property="color" column="color"/>
  <discriminator javaType="int" column="vehicle_type">
    <case value="1" resultMap="carResult"/>
    <case value="2" resultMap="truckResult"/>
    <case value="3" resultMap="vanResult"/>
    <case value="4" resultMap="suvResult"/>
  </discriminator>
</resultMap>

<resultMap id="carResult" type="Car">
  <result property="doorCount" column="door_count" />
</resultMap>
```
这个例子中，如果vehicle_type的值是1，则会使用carResult，即只加载doorCount属性，如果需要加载vehicleResult中的属性，则需要让carResult继承vehicleResult，使用如下：

``` 
<resultMap id="carResult" type="Car" extends=”vehicleResult”>
  <result property=”doorCount” column="door_count" />
</resultMap>
```

**discriminator鉴别器（二）**  

还有一种方式可以实现上面的效果：

``` 
<resultMap id="vehicleResult" type="Vehicle">
  <id property=”id” column="id" />
  <result property="vin" column="vin"/>
  <result property="year" column="year"/>
  <result property="make" column="make"/>
  <result property="model" column="model"/>
  <result property="color" column="color"/>
  <discriminator javaType="int" column="vehicle_type">
    <case value="1" resultType="carResult">
      <result property=”doorCount” column="door_count" />
    </case>
    <case value="2" resultType="truckResult">
      <result property=”boxSize” column="box_size" />
      <result property=”extendedCab” column="extended_cab" />
    </case>
    <case value="3" resultType="vanResult">
      <result property=”powerSlidingDoor” column="power_sliding_door" />
    </case>
    <case value="4" resultType="suvResult">
      <result property=”allWheelDrive” column="all_wheel_drive" />
    </case>
  </discriminator>
</resultMap>
```

