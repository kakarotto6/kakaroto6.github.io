--- 
layout: post
title: dao向resultMap传值
date: 2018-11-22
tags: java
---
### **对应实体的各种属性**
``` 
    <resultMap type="Article" id="ArticleResult">
        <result property="id" column="id"/>
        <result property="articleTitle" column="article_title"/>
        <result property="articleCreateDate" column="article_create_date"/>
        <result property="articleContent" column="article_content"/>
        <result property="isTop" column="is_top"/>
        <result property="addName" column="add_name"/>
    </resultMap>
```
 
传入的参数是Map Mybatis会自动解析Map里的字段
``` 
<select id="findArticles" parameterType="Map" resultMap="ArticleResult">
        select id,article_title,article_create_date,article_content,add_name from ssm_article
        <where>
            <if test="articleTitle!=null and articleTitle!='' ">
                and article_title like #{articleTitle}
            </if>
        </where>
        <if test="start!=null and size!=null">
            limit #{start},#{size}
        </if>
    </select>
```
### **Mapper文件参数获取的方式**  
参数个数为1个（string或者int）
dao层方法为以下两种  
单个int型  
``` 
public List<UserComment> findByDepartmentId(int dapartmentId);
```
单个string型  
``` 
public Source findByTitle(String title);
```
**对应的Mapper取值：**  
取值时应当注意，参数名字应该与dao层传入的参数名字相同。  
**单个int型**
``` 
<select id="findByDepartmentId"  resultType="com.bonc.wechat.entity.publicserver.UserComment">
	select * from wx_user_comment where 
	department_id=#{departmentId} 
	order by createtime desc;
</select>
```
**单个string型**  
``` 
<select id="findByTitle"  parameterType="java.lang.String" resultType="com.bonc.wechat.entity.publicserver.Source">
	select * from wx_source where 
        title=#{title};
</select>
```
2、参数个数为多个。  
dao层方法：  
1.正常传参

``` 
public Dailyuserinfo findStutaByUserAndDaily(String username,String dailyid);
 
```

 
2.注解传参  

``` 
public List<UserTab> selectUserListExceptUserId
(@Param("USER_ID")String USER_ID, 
 @Param("LIMIT_POS")int LIMIT_POS, 
 @Param("LIMIT_SIZE")int LIMIT_SIZE);
```

对应的Mapper取值：
取值时应当注意，参数名字应该与dao层传入的参数名字相同。
### 正常传参方式参数获取

``` 
<select id="findStutaByUserAndDaily"
           parameterType="java.lang.String" 
           resultType="com.thinkgem.jeesite.modules.dailynews.entity.Dailyuserinfo">
 
    select * from daily_user_info 
    where login_name=#{username} 
    And daily_id=#{dailyid};
 
</select>
```

 
 
### 注解传参方式参数获取

``` 
<select id="selectUserListExceptUserId" 
            resultMap="userResMap">
 
	select * from MH_USER 
        where USER_ID!=#{USER_ID} 
        and USER_STATE>9 
        order by NICK_NAME 
        limit #{LIMIT_POS},#{LIMIT_SIZE}
</select>
```

### 3、参数为map的形式
mapper中使用map的key取值  
dao中的方法  
### 1.参数为map

``` 
public List<Source> search(Map<String,Object> param);
```

 
### 2.map的内部封装

``` 
 Map<String,Object> param=new HashMap<String,Object>();
    param.put("page", (page-1)*pageSize);
    param.put("pageSize",pageSize);
    param.put("keyword","%"+keyword+"%");
    param.put("type",type);
    List<Source> sources=sourceDao.search(param);
```
	
###  **对应的Mapper取值**

``` 
<select id="search" 
            parameterType="java.util.Map" 
            resultType="com.bonc.wechat.entity.publicserver.Source">
	select * from wx_source 
        where 
	 <if test="keyword != null and keyword != ''">
        (title like #{keyword} or content like #{keyword}) and
         </if>
          type=#{type}
	  order by ordernum asc 
          limit #{page},#{pageSize};
</select>
```

## **参数为对象**
mapper中使用对象中的属性直接取值，或者【对象.属性】取值。
dao中的方法  
### **使用对象传参**
``` 
public int addUserComment(UserComment UC);
```
**对象中的属性**  

``` 
private int id;
	private int department_id;		
	private String telphone;		
	private String content;			
	private String createtime; 

```

### **对应的Mapper取值：**
``` 
<insert id="addUserComment" 
            parameterType="com.bonc.wechat.entity.publicserver.UserComment">
	insert into wx_user_comment
                   (department_id,
                    telphone,
                    content,
                    createtime) 
	values(#{department_id},
                   #{telphone},
                   #{content},
                   #{createtime});
</insert>
```

### **使用【对象.属性】取值**

**dao层：**  
******此示例中直接省去dao层，service直接绑定mapper层******

``` 
public PageResult findPanoramaPage(Page page) throws Exception{
    List<PageData> panoramaList = new ArrayList<PageData>();
    try {
	panoramaList = (List<PageData>)dao.findForList("PanoramaMapper.findPagePanorama", page);
	} catch (Exception e) {
		e.printStackTrace();
	    }
	PageResult pageResult = new PageResult(page.getTotalResult(),panoramaList);
	return pageResult;
}
```

**对应的Mapper取值：**

``` 
<select id="findPagePanorama" 
            parameterType="page" 
            resultType="pd">

	SELECT 
		a.id as id,
		a.project_code as projectCode,
		a.project_name as projectName,
		a.project_addr as projectAddr,
		a.project_type as projectType
	FROM 
		calm_project a 
	WHERE 
		a.is_valid=1	
	<if test="page.projectType != null and page.projectType != ''">
        AND a.project_type = #{page.projectType} 
        </if>
	<if test="page.keyword != null and page.keyword != ''">
       AND (a.project_name LIKE '%${page.keyword}%' OR a.project_code LIKE '%${page.keyword}%')
        </if>
        ORDER BY 
             a.sort
</select>
```


