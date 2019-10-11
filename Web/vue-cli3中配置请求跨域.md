# 关于vue-cli3中配置请求跨域的问题

根据Vue CLI3官方文档，[](https://cli.vuejs.org/zh/config/#devserver-proxy)

需要在`vue.config.js`文件中配置`devServer.proxy`选项来解决跨域问题。

1. 关于`vue.config.js`文件

   此文件在vue-cli3中不会自动生成，需要手动在项目根目录下创建。

2. 配置`devServer.proxy`选项

   以豆瓣的电影接口为例，进行如下配置。

   - `proxy`中的`/j/search_subjects`代表匹配`api`的路径，当命中请求中的`url`时进入这个代理，进行跨域请求；
   - `target`代表想要跨域的目标`url`；
   - `changeOrigin`：true代表允许跨域；
   - `pathRewrite`：代表当匹配到上面的路径`/j/search_subjects`时需要在`target`后面拼接什么路径;

```js
module.exports = {
    devServer: {
        proxy: {
            '/j/search_subjects': {
                target: 'https://movie.douban.com',
                changeOrigin: true,
                pathRewrite: {
                    '^/j/search_subjects': '/j/search_subjects'
                }
            }
        }
    }
}
```

3. 在发起请求时：

```js
this.axios.get("/j/search_subjects?type=movie&tag=%E7%83%AD%E9%97%A8&page_limit=5&page_start=0")
      .then(response => {
        console.log(responses);
      })
```

**可以看到，当在此发起get请求时，在上面的配置文件中匹配到了请求url中的“/j/search_subjects”，此时配置就会生效，把这个请求拼接到上面的target后面，请求不同域中远端的资源。**

而此时打开浏览器开发者模式，通过network可以看到，

这个请求的路径显示为：`http://localhost:8080/j/search_subjects?type=movie&tag=%E7%83%AD%E9%97%A8&page_limit=5&page_start=0`,

而实际请求的地址却是：`https://movie.douban.com/j/search_subjects?type=movie&tag=%E7%83%AD%E9%97%A8&page_limit=5&page_start=0`

这就是实际解决跨域的方式，通过把域名修改来达到跨域的目的，而实际访问的路径却是另一个域名的资源，这就是通过proxy来实现跨域的方法。