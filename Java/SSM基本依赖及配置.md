# 依赖

## 基本配置

```xml
 <properties>
        <spring.version>5.0.6.RELEASE</spring.version>
     	<mybatis.version>3.4.6</mybatis.version>
 </properties>
```

```xml
<dependencies>  
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
        <!-- 添加mysql驱动依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.45</version>
        </dependency>
        <!-- 数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.20</version>
        </dependency>
    </dependencies>
```

## log4j相关

```xml
 <!-- log4j -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.10.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.10.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-web</artifactId>
            <version>2.9.1</version>
        </dependency>
```

完整

```xml
		<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <!-- ************** 1.Spring ************** -->
        <!-- 1)Spring核心 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- 2)Spring DAO层 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- 3)Spring web -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- 4)Spring test -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- ************** Dao： Mybatis ************** -->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>

        <!-- **************  数据库 **************  -->
        <!-- 添加mysql驱动依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.45</version>
        </dependency>
        <!-- 数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.20</version>
        </dependency>

        <!-- ************** 3.Servlet web ************** -->
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>

        <!-- ************** log4j ************** -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.10.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.10.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-web</artifactId>
            <version>2.9.1</version>
        </dependency>
```



# 配置

## database

`jdbc.properties`

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/user?useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=123456
```

## spring

`applicationContext.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- service层 配置注解扫描 -->
    <context:component-scan base-package="com.demo.service"/>

    <!-- 加载配置文件 -->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!-- 配置数据源 数据库连接池 数据源不同需要导入的jar包也不容 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--   配置SqlSessionFactory对象
        Spring与Mybatis完美整合，不予要mybatis的配置映射文件
        1. dataSource：配置数据源，连接数据库；
        2. configLocation：配置mabatis配置文件位置，加载mybatis的配置；
        3. mapperLocations：指明映射文件位置，自动扫描mapping.xml文件。
    -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 扫描pojo包，使用别名配置(在mybatis中可以使用别名，即pojo的名称) -->
        <property name="typeAliasesPackage" value="com.demo.entity"/>
        <!-- 扫描mapper的配置文件 -->
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
        <!--也可以引入mybatis配置文件 -->
        <!--<property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml"/>-->
    </bean>

    <!-- 使用mybatis的接口代理开发模式(必须保证接口和对应的mybatis的xml名称相同，且在一个文件夹内) -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.demo.mapper"/>
        <!--<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>-->
    </bean>
</beans>
```



## springmvc

`springMvc.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/mvc
         http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 开启注解扫描 -->
    <context:component-scan base-package="com.demo.controller"/>

    <!-- 配置springmvc的基本控制器，并开启了对json数据格式的支持 -->
    <!-- mvc注解驱动：可以代替注解映射器和适配器的配置
         mvc注解驱动默认加载了很多的参数绑定，比如默认加载json转换解析器
         如果使用该注解驱动，就不用配置注解映射器和适配器  -->
    <mvc:annotation-driven/>
    
    <!-- 对静态资源的访问 -->
    <mvc:resources mapping="/static/**" location="/WEB-INF/static/" />

    <!-- 配置springmvc的视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## web

`web.xml`

```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<!--<?xml version="1.0" encoding="UTF-8"?>-->
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
         id="WebApp_ID" version="2.5">

    <display-name>Archetype Created Web Application</display-name>
    <welcome-file-list>
        <welcome-file>/WEB-INF/index.jsp</welcome-file>
    </welcome-file-list>

    <!-- 配置spring的字符编码过滤器，保证request请求的中文字符不会乱码（注意这个过滤器要放到最前面） -->
    <filter>
        <filter-name>CharacterEncoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncoding</filter-name>
        <!-- 设置这个字符编码过滤器作用与每一个请求 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 配置Spring的servlet监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- 加载spring容器 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring/applicationContext.xml</param-value>
    </context-param>

    <!-- 配置springmvc的前端控制器，request请求会先经过这个控制器 -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- contextConfigLocation配置springmvc加载的配置文件(配置处理器映射器，适配器)
         如果不配置，默认加载的是/WEB-INF/servlet名称-servlet.xml(springmvc-servlet.xml)-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring/springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <!--  url-patten:
          1.*.action,访问以.action结尾由DispatcherServlet进行解析；
          2./，所有访问的地址都由DispatcherServlet进行解析，对于静态的文件的解析需要配置不让DispatcherServlet解析
             使用此种方法可以实现RESTful风格的url；
          3./*，不对，使用此种配置，最终要转发到一个jsp页面时，仍然会由DispatcherServlet解析jsp页面
             不能根据jsp页面找到handler，会报错。
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

## log

`log4j.properties`

```properties
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# MyBatis logging configuration...
log4j.logger.com.how2java=TRACE
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

## mybatis

`sqlMapConfig.xml`

spring与mybatis整合不需要额外配置mybatis；