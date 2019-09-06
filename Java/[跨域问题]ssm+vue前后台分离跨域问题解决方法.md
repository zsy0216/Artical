## 跨域未解决

Access to XMLHttpRequest at ‘http://localhost:8080/vue/findall from origin ‘http://localhost:8000’ has been blocked by CORS policy: No ‘Access-Control-Allow-Origin’ header is present on the requested resource.

## ssm后台解决跨域

`pom.xml`文件中加入依赖：

```xml
<dependency>
    <groupId>com.thetransactioncompany</groupId>
    <artifactId>cors-filter</artifactId>
    <version>2.5</version>
</dependency>
```

`web.xml`文件中加入：

```xml
<!--跨域-->
<!--加入跨域过滤器配置-->
<filter>
    <filter-name>CORS</filter-name>
    <filter-class>com.thetransactioncompany.cors.CORSFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>CORS</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## vue打包导入后台解决跨域

==打包前进行以下配置==

build文件夹中的`utils.js`，指定位置添加`publicPath: '../../'`

![utils](https://zsy0216.coding.me/image/java/vue/utils.png)

config文件夹下的`index.js`，指定位置添加修改为`assetsPublicPath: './',`

![index](https://zsy0216.coding.me/image/java/vue/index.png)

之后执行

```bash
npm run build
```

打包后将dist下的index.html ,css , js等文件添加到webapp下即可通过访问index.html访问