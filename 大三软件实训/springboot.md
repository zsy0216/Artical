# 注解

## dao层Xxxmapper.class 类上加@Repository

```java
@Repository
public interface UserMapper {
}
```

## service层XxxServiceImpl 类上加@Service

```java
@Service
public class UserServiceImpl implements UserService{
}
```

## controller层XxxController类上加@Controller

```java
@Controller
public class UserController {

    @RequestMapping("/hello")
    public String test(){
        return "redirect:/index.html";
    }
}
```

## 主程序类和测试类上加@MapperScan

```java
@MapperScan("com.zut.hostest.dao")
//参数是dao层接口的包名
public class HosTestApplication {
    public static void main(String[] args) {
        SpringApplication.run(HosTestApplication.class, args);
    }
}
```

```java
@RunWith(SpringRunner.class)
@SpringBootTest()
//@ComponentScan
@MapperScan("com.zut.hostest.dao")
//不加时报No qualifying bean of type 'com.zut.hostest.dao.OfficeMapper' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
public class HosTestApplicationTests {
    @Test
    public void contextLoads() {
    }
}
```

# 配置

## 修改springboot启动图案

在/src/main/resources下新建banner.txt,访问[此网站](http://patorjk.com/software/taag)输入需要的文字，然后在下面复制图案，粘贴到新建的文件中即可。

```txt
  _                         _  _          _
 | |                       (_)| |        | |
 | |__    ___   ___  _ __   _ | |_  __ _ | |
 | '_ \  / _ \ / __|| '_ \ | || __|/ _` || |
 | | | || (_) |\__ \| |_) || || |_| (_| || |
 |_| |_| \___/ |___/| .__/ |_| \__|\__,_||_|
                    | |
                    |_|
```

## 主配置文件中的配置

```properties
#端口号配置
server.port=8080

#开启驼峰命名
mybatis.configuration.map-underscore-to-camel-case=true
#声明实体类所在包
mybatis.type-aliases-package=com.zut.hostest.entity

#声明与数据库交互用的XxxMapper文件位置
mybatis.mapper-locations=classpath:mapper/*.xml

spring.mvc.static-path-pattern=/static/**

#--------------------数据库配置-------------------------
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost/test?characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=123456
```

