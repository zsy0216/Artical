​	我们都知道，在Mybatis中的Mapper.xml映射文件可以定制动态SQL，在dao层定义的接口中定义的参数传到xml文件中之后，在查询之前mybatis会对其进行动态解析,通常使用#{}接收，下面介绍几种比较常用的用法。	

## 前言#{}与${}区别

- #{} 在SQL动态解析之后，编译将#{}替换为占位符？去替换参数，可以防止sql注入；
- #{} 只是表示占位，与参数的名字无关，如果只有一个参数会自动对应，下面会介绍多个参数的问题；
- ${} 是进行字符串拼接，直接取出参数值，放到sql语句中；

- 使用注意点，当表名作为变量时，必须使用${}获取参数作为表名进行查询，否则会出现语法错误；

总结：

* sql语句动态生成的时候，使用${}

- sql语句中某个参数进行占位的时候用#{}

## 接收多个参数

​	遇到这个问题是在昨天实现分页的时候接收参数时遇到了错误，最后用第一种方法解决，但是怀着路漫漫其修远兮的态度，吾必将上下而求索其他用法，所以在查询了网上很多教程和mybatis官方文档，总结出以下四种常用用法；

- 使用arg0，arg1或使用param1，param2接收（arg索引从0开始，param索引从1开始）；
- 多个参数封装成对象；
- 多个参数封装成Map集合；
- 使用@param绑定参数；

下面分别介绍

看传来的参数：

- pageNo是页码，即第几页；
- pageSize是每一页有多少数据；

```java
public interface CustomerDao {
     List<Customer> findAllPageSql(Integer pageNo,Integer pageSize);
}
```

### 第一种方法：使用arg或param

```java
<mapper namespace="com.vue.dao.CustomerDao">
    <select id="findAllPageSql" parameterType="java.lang.Integer" resultType="com.vue.entity.Customer">
        SELECT * FROM t_customer LIMIT #{param1},#{param2}
    </select>
</mapper>
```

或

```java
<mapper namespace="com.vue.dao.CustomerDao">
    <select id="findAllPageSql" parameterType="java.lang.Integer" resultType="com.vue.entity.Customer">
        SELECT * FROM t_customer LIMIT #{arg0},#{arg1}
    </select>
</mapper>
```

### 第二种方法：封装成对象

Page.java

```java
public class Page {
    private Integer pageNo;
    private Integer pageSize;
	//getter,setter略
}
```

传参：

控制层封装前台传来的page信息

```java
	@ResponseBody
    @GetMapping("/allClass")
    public String findAllPageClass(@RequestParam(required = true,defaultValue = "1") Integer pageNo,
                                 @RequestParam(required = false,defaultValue = "5") Integer pageSize) {
        Page page = new Page();
        page.setPageNo(pageNo-1);
        page.setPageSize(pageSize);
        return customerService.findAllPageClass(page);
    }
```

service层：

```java
public interface CustomerService{   
	String findAllPageClass(Page page);
}
```

持久层传参：

```java
public interface CustomerDao {
     List<Customer> findAllPageClass(Page page);
}   
```

mapper.xml映射文件

```xml
<mapper namespace="com.vue.dao.CustomerDao">
   <select id="findAllPageClass" resultType="com.vue.entity.Customer" parameterType="com.vue.util.Page">
       SELECT * FROM t_customer LIMIT #{pageNo},#{pageSize}
   </select>
</mapper>
```

### 第三种方法：封装成Map集合

传参：

控制层封装前台传来的page信息

```java
	@ResponseBody
    @GetMapping("/allMap")
    public String findAllPageMap(@RequestParam(required = true,defaultValue = "1") Integer pageNo,
                                   @RequestParam(required = false,defaultValue = "5") Integer pageSize) {
        Map<String,Integer> pageMap = new HashMap<>();
        pageMap.put("pageNo",pageNo-1);
        pageMap.put("pageSize",pageSize);
        return customerService.findAllPageMap(pageMap);
    }
```

service层：

```java
public interface CustomerService{   
	String findAllPageMap(Map<String, Integer> pageMap);
}
```

持久层传参：

```java
public interface CustomerDao {
     List<Customer> findAllPageMap(Map<String, Integer> pageMap);
}   
```

mapper.xml映射文件

```xml
<mapper namespace="com.vue.dao.CustomerDao">
   <select id="findAllPageMap" parameterType="java.util.Map" resultType="com.vue.entity.Customer">
       SELECT * FROM t_customer LIMIT #{pageNo},#{pageSize}
   </select>
</mapper>
```

### 第四种方法：使用@Param绑定参数

```java
public interface CustomerDao {
	List<Customer> findAllPageSql(
        @Param(value = "pageNo") Integer pageNo,
        @Param(value = "pageSize") Integer pageSize);
}
```

mapper.xml映射文件

```xml
<mapper namespace="com.vue.dao.CustomerDao">
	<select id="findAllPageSql" parameterType="java.lang.Integer" resultType="com.vue.entity.Customer">
        SELECT * FROM t_customer LIMIT #{pageNo},#{pageSize}
	</select>
</mapper>
```

