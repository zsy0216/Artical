# 1    Spring  IOC容器配置

## 1.1   applicationContext.xml

### 1.1.1   配置数据源

1. 导入资源文件，db.properties

```xml
<context:property-placeholder location="classpath:db.properties"/>
```

注：db.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm_paging?useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=123456
```

2. 配置数据源，连接数据库。

```xml
<!-- 配置数据源  数据源不同 依赖的jar包也不同 -->
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
    <property name="maxActive" value="30"/>
    <property name="maxIdle" value="5"/>
</bean>
```

### 1.1.2   配置mybatis 的sessionFactory

Spring与Mybatis完美整合，不予要mybatis的配置映射文件

1. dataSource：配置数据源，连接数据库；

2. configLocation：配置mabatis配置文件位置，加载mybatis的配置；

3. mapperLocations：指明映射文件位置，自动扫描mapping.xml文件。

```xml
<!--配置sqlSessionBean-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="configLocation" value="classpath:sqlMapConfig.xml"/>
    <!-- 扫描pojo包，使用别名配置(在mybatis中可以使用别名，即pojo的名称) -->
    <property name="typeAliasesPackage" value="com.demo.entity"/>
    <!--用于指明mybatis的配置文件的位置-->
    <property name="mapperLocations" value="classpath:mappers/*.xml"/>
</bean>
```

### 1.1.3   配置MapperScannerConfigurer

```xml
	<!-- 使用mybatis的接口代理开发模式(必须保证接口和对应的mybatis的xml名称相同，且在一个文件夹内) -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cn.tycoding.mapper"/>
        <!--<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>-->
    </bean>
```

#### SqlSessionFactoryBean

Spring框架简化数据库访问的方式之一就是管理数据库连接的声明周期和ORM框架的Session，以确保他们正常的打开和关闭。而在mybatis-spring中，SqlSessionFactoryBean实现了Spring的FactoryBean接口，用于创建`SqlSessionFactory`。

- `dataSource`是必要的配置，上面我们仅仅是写了数据源配置，接下来的数据库访问对象肯定要使用这个数据源来获取链接对象，所以必须指定数据源。
- `typeAliases`则是启用别名配置，`typeAliasesPackage`则会为`pojo`包下的所有类对象都配置别名，当我们在对应的XML映射中指定`ResultType`等时，就可以直接写pojo的类名，而不需要写全限定名。
- `mapperLocations`: 用于指定Mapper的XML文件位置（对于那种Mapper接口和XML配置同名且在同一个配置文件夹下，Mybatis其实会根据Mapper的Class文件自动找对应的XML配置文件，但是这里我建议写上）。
- `configLocation`: 用于指定mybatis的配置文件位置，因为本项目中并没有涉及太多复杂的逻辑，所以这里我就没有特意配置mybatis的config。

#### MapperScannerConfigurer

在了解`MapperScannerConfigurer`之前我们需要了解一下`MapperFactoryBean`，Mybatis-Spring提供了一个动态代理的实现：`MapperFactoryBean`，他能实现将映射器接口（因为代理创建在运行环境中，那么指定的映射器必须是接口而不是实现类）直接注入到service的Bean中。这样通过代理对象去创建编写数据访问对象，从而代替`SqlSessionTepmlate`等工具。
但是在Mybatis-Spring中还提供了一个转换器`MapperScannerConfigurer`，它会查找类路径下的映射器（接口）并自动将他们创建成`MapperFactoryBean`。通过将接口转换成Spring容器中的Bean，在Service层直接注入接口即可，这样大大简化了Dao层实例的编写。
`basePackage`正是指定接口的位置。
注意我们这里并不需要指定`SqlSessionFactory`(即我们注释掉的部分)，因为使用`MapperScannerConfigurer`会创建`MapperFactoryBean`后自动装配（XML配置）。但是如果存在多个`dataSource`，该自动装配可能就会失效，这时我们必须使用`sqlSessionFacoryBeanName`来指定引用哪个`bean`。

在`beans.xml`配置阶段算是基本告一段落，我们回想一下：在没有使用Mybatis前我们使用Spring提供的JDBC模板来访问数据库，我们发现这种方式代码量特别大，而且于Java代码的耦合性也比较大，Mybatis则正好弥补了这些缺点，它使用的XML配置更加的小巧方便且支持原生SQL的编写。
**Mybatis-Spring**提供了移动动态代理的方式来代替手工的`SqlSessionTemplate`编写数据库访问对象，我们只需要编写一个接口，通过在Mapper对应的XML中直接写SQL就可以进行数据库访问。当然我们必须配置`SqlSessionFactoryBean`和`MapperScannerConfigurer`。

### 1.1.4   配置声明式事务

步骤：（基于注解）

1. 配置事务管理器

```xml
<!-- 配置事务管理器 -->
<bean id="transactionManager"
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager"
      p:dataSource-ref="dataSource"/>
```

```xml
<!-- 事务注解驱动，标注@Transactional的类和方法将具有事务性 -->
<tx:annotation-driven/>
```

2. 配置事务属性，需要事务管理器

```xml
<tx:advice id="txAdvice"/>
```

3. 配置事务切面，并把事务属性与事务切面关联起来

```xml
<!-- 通过AOP配置提供事务增强，让service包下所有Bean的所有方法拥有事务 -->
<aop:config proxy-target-class="true">
    <aop:pointcut id="serviceMethod"
                  expression="(execution(* zut.hospital.service..*(..))) and (@annotation(org.springframework.transaction.annotation.Transactional))"/>
    <aop:advisor pointcut-ref="serviceMethod" advice-ref="txAdvice"/>
</aop:config>
```

<!-- 配置声明式事务步骤：(基于配置文件)

​          1.配置事务管理器

​          2.配置事务属性

​          3.配置事务切面，并把事务属性与事务切面关联起来 

```xml
<bean id="transactionManager"  class="org.springframework.jdbc.datasource.DataSourceTransactionManager" 			p:dataSource-ref="dataSource"/>

<tx:advice id="txAdvice" transaction-manager=”transactionManager”/>
	<tx:attributes>
	 <tx:methods name="" propagation=""
             ...
   </tx:attributes>
</tx:advice

<aop:config>
 <aop:advisor advice-ref="txAdvice" pointcut="execution(* zut.hospital.service.*.*(..))"id="txPointCut"/>
<aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
</aop:config>
```

使用注解开发时，spring配置文件中还需要加入自动扫描包的配置，用于自动装配bean，以及自动依赖注入，@Autowired标注的参数自动装配

```xml
<context:component-scan base-package="zut.hospital.dao"/>                          
```

# 2    Springmvc 配置相关

## 2.1   Springmvc

![1543729079239](https://zsy0216.coding.me/image/java/ssm/mvc.png)

1. 前端控制器：接收请求，响应结果，相当于转发器。

2. 处理器映射器HandlerMapping（不需要程序员编写）

    作用：根据请求的url查找Handler

3. 处理器适配器HandlerAdapter

    作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler

4. 处理器Handler（需要程序员开发）

   注意：编写Handler要按照HandlerAdapter的要求去做，这样适配器才可以去正确执行Handler 

5. 视图解析器View resolver（不需要程序员开发）

    作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）

6. 视图View（需要程序员开发 页面）

    View是一个接口，实现类支持不同的View类型（jsp，freemarker，pdf...）

## 2.2   前端控制器

作用：接收用户请求，响应结果，相当于转发器

在**==web.xml==**中配置，是一个servlet。

```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- contextConfigLocation配置springmvc加载的配置文件(配置处理器映射器，适配器)
        如果不配置，默认加载的是/WEB-INF/servlet名称-servlet.xml(springmvc-servlet.xml)-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>    
    <load-on-startup>1</load-on-startup>
    <async-supported>true</async-supported>
</servlet>
```

`<init-param>`

contextConfigLocation配置springmvc加载的配置文件(配置处理器映射器，适配器)，如果不配置，默认加载的是/WEB-INF/servlet名称-servlet.xml(springmvc-servlet.xml)

```xml
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!--  url-patten:
        1.*.action,访问以.action结尾由DispatcherServlet进行解析；
        2./，所有访问的地址都由DispatcherServlet进行解析，对于静态的文件的解析需要配置不让DispatcherServlet解析
            使用此种方法可以实现RESTful风格的url；
        3./*，不对，使用此种配置，最终要转发到一个jsp页面时，仍然会由DispatcherServlet解析jsp页面
            不能根据jsp页面找到handler，会报错。
        -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

## 2.3   处理器映射器、处理器适配器

`springmvc.xml`

处理器映射器：根据请求的url查找Handler；

处理器适配器：按照特定的规则执行Handler；

```xml
<!-- 注解映射器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>

<!-- 注解适配器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
```

注解的映射器和适配器是配对使用的。

此外，使用以下配置时就不需再重新配置上面两个处理器映射器和处理器适配器；

```xml
<!-- mvc注解驱动：可以代替注解映射器和适配器的配置
    mvc注解驱动默认加载了很多的参数绑定，比如默认加载json转换解析器
    如果使用该注解驱动，就不用配置注解映射器和适配器-->
<mvc:annotation-driven></mvc:annotation-driven>
```

一般开发中使用<mvc:annotation-driven>，

对标注了注解@RequestMapping(“/url”)的方法进行映射。

## 2.4   视图解析器

作用：进行视图解析，根据逻辑视图名解析成真正的视图(view)

`springmvc.xml`

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
      p:prefix="/WEB-INF/pages/"
      p:suffix=".jsp"/>
```

##  2.5  静态资源处理

servlet会拦截所有请求，这时候静态资源的访问不到了

```
	<!-- 对静态资源的访问 -->
    <mvc:resources mapping="/static/**" location="/WEB-INF/static/" />
```



# 3    Mybatis

## 3.1   sqlMapConfig.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
		"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

</configuration>
```

mybatis基本配置已经在与spring整合时在applicationContext.xml中配置

详细配置见http://www.mybatis.org/mybatis-3/zh/configuration.html#