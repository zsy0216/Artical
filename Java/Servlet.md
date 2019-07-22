HTTP传输数据类型：ContentType

### Servlet请求响应流程

1. 浏览器发送请求到服务器（请求）
2. 服务器接收浏览器的请求进行解析，创建request对象存储请求数据
3. 服务器调用对应的Servlet进行请求处理，并将request对象作为实参传递给Servlet的service方法
4. Servlet的service方法执行请求处理：
   1. 设置请求的编码格式
   2. 设置响应的编码格式
   3. 获取请求信息
   4. 处理请求信息
      1. 创建业务层对象
      2. 调用业务层对象的方法
   5. 响应请求处理
      1. 直接响应
      2. 请求转发
      3. 重定向

**请求转发：**

```java
request.getRequestDispatcher("servletname").forward(request,response);
```

* 请求转发的两个Servlet属于一次请求，浏览器地址栏信息不改变，刷新时可能导致表单重复提交。
* / :表示项目根目录

**重定向**

```java
response.sendRedirect("url");
```

* 使用时机：解决当前Servlet无法处理请求的问题以及表单重复提交的问题。

* 重定向的两个Servlet属于两次请求，拥有两个request对象，浏览器地址会改变，Servlet之间数据不能共享。

两次请求的数据共享需要使用session，session依赖于cookie。

* / : 表示服务器根目录，绝对路径需要加上虚拟项目名

### Cookie

1. 创建cookie对象

```java
Cookie cookie = new Cookie(String name, String value);
```

2. 配置cookie
   1. 设置有效期

      ```java
      cookie.setMaxAge(int seconds);
      ```

   2. 设置有效路径

      ```java
      cookie.setPath(String uri);
      //指定路径请求携带cookie信息，不配置默认所有请求都携带
      ```

3. 响应cookie信息给客户端

   ```java
   response.addCookie(cookie);
   ```

4. 获取cookie

   ```java
   Cookie[] cks = request.getCookies();//数组
   ```

### Session

#### 特点：

​	依赖cookie技术，生命周期为一次回话，默认存储时间是30分钟

​	session对象的唯一标识JSESSIONID同过cookie技术作为一个cookie数据存储到浏览器的运行内存中

#### 作用：

​	解决了一个用户不同请求处理的数据共享问题

#### 使用：

1. 创建session对象/获取session对象

   ```java
   HttpSession hs = req.getSession();
   //如果请求中拥有session的标识符也就是JSSESIONID，则返回其对应的session对象
   //如果请求中没有JSSESIONID，则创建新的session对象，并将其JSSESIONID作为cookie对象存储到浏览器的运行内存中
   //如果session对象失效了，也会创建一个session对象，并将其JSESSION对象存储到浏览器的运行内存中
   ```

2. 设置session

   ```java
   //设置session存储时间，默认30分钟
   hs.setMaxInactiveInterval(int seconds);
   //设置session强制失效
   hs.invalidate();
   ```

3. 存储和获取数据

   ```java
   //存储
   hs.setAttribute(String name, Object value);
   //获取，返回的数据为Object类型
   hs.getAtrribute(String name);
   //注意：存储的动作和取出的动作发生在不同的请求中，但是存储要先于取出执行；
   //使用时机：用户在登录web项目时会将用户的个人信息存储到session中，供该用户的其他请求使用
   ```

4. 总结：session解决了一个用户的不同请求的数据共享问题，只要在JSESSIONID不失效和session对象不失效的情况下，用户的任意请求在处理时都能获取到同一个session对象

5. 注意：JSESSIONID存储在了cookie的临时存储空间中，浏览器关闭即失效；

### ServletContext对象

> request解决了一次请求内的数据共享问题，session解决了一个用户不同请求的数据共享问题，而不同用户之间的数据共享问题则需要使用ServletContext对象。

#### 特点

 	1. 由服务器创建
 	2. 用户共享
 	3. 一个项目只有一个

#### 生命周期

​	服务器启动到服务器关闭

#### 使用：

```java
//1.获取ServletContext对象（三种方式，等价）
ServletContext sc = this.getServletContext();
ServletContext sc2 = this.getServletConfig().getServletContext();
ServletContext sc3 = req.getSession().getServletContext();
//2.使用ServletContext对象完成数据共享
sc.setAttribute(String name,Object value);
sc.getAttribute(String name);//返回Object对象类型数据
//3.获取项目中web.xml中的全局配置数据
sc.getInitParameter(String name);//返回String类型数据
//4.获取项目根的根路径信息
sc.getRealPath("");

```

### ServletConfig

​	ServletConfig对象是Servlet的专属对象.

#### 问题：

​	如何获取在web.xml中给每个Servlet单独配置的数据？

#### 解决：

​	使用ServletConfig对象

#### 使用：

 1. 获取ServletConfig对象

    ```java
    ServletConfig sc = this.getServletConfig();
    ```

    

 2. 获取web.xml中的配置数据

    ```java
    String value = sc.getInitParameter("name")
    ```

### Web.xml文件使用总结

#### 作用：

​	存储项目相关的配置信息，保护Servlet。解耦一些数据对程序的依赖。

#### 使用位置：

​	每个Web 项目中；

​	Tomcat服务器中（在服务器conf目录下）

#### 区别：

​	Web项目下的web.xml文件为局部配置，针对本项目的配置；

​	Tomcat下的web.xml文件为全局配置，配置公共信息；

#### 内容（核心组件）

​	全局上下文配置（全局配置参数）

​	Servlet配置

​	过滤器配置

​	监听器配置

#### 加载顺序：

​	Web容器会按ServletContext -> context-param -> listener -> filter -> servlet这个顺序加载组件，这些元素可配置在web.xml文件中的任意位置。

​	在服务器启动时完成加载。

### server.xml

#### 问题：

​	浏览器发起请求后，服务器根据请求在webapps目录下调用对应的的Servlet进行请求处理。那么为什么是webapps目录，难道不能是其他的文件吗？

#### 解决：

​	了解server.xml文件的配置信息

server.xml文件的核心组件

```xml
<Server>
	<Service>
        <Connector/>
        <Connector/>
        <Engine>
            <Host>
                <Context/>
            </Host>
        </Engine>
    </Service>
</Server>
```

#### 热部署

```xml
<Context path="/Pet" reloadable="true" docBase="F:/PetWebroot"/>
```

