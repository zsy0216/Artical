## @Controller

在对应的方法上，视图解析器可以解析return 的jsp,html页面，并且跳转到相应页面

若返回json等内容到页面，则需要加`@ResponseBody`注解

### **`@RestController`**

相当于@Controller+@ResponseBody两个注解的结合，返回json数据不需要在方法前面加@ResponseBody注解了，但使用@RestController这个注解，就不能返回jsp,html页面，视图解析器无法解析jsp,html页面

1. 如果只是使用`@RestController`注解Controller，则Controller中的方法无法返回jsp页面，配置的视图解析器`InternalResourceViewResolver`不起作用，返回的内容就是Return 里的内容。

   例如：本来应该到success.jsp页面的，则其显示success.

2. 如果需要返回到指定页面，则需要用 @Controller配合视图解析器`InternalResourceViewResolver`才行。
3. 如果需要返回JSON，XML或自定义mediaType内容到页面，则需要在对应的方法上加上`@ResponseBody`注解。

## @ResponseBody

如果需要返回JSON，XML或自定义mediaType内容到页面，则需要在对应的方法上加上`@ResponseBody`注解。

若是添加到类上，则该类下的所有方法自动就添加了此注解。

## @RequestBody

  使用：一般注解在方法的参数上；

  介绍：针对传输格式为 application/json 类型的数据;

 	当传输数据格式为以下格式则不需要此注解;

1. x-www-form-urlencoded
  2. form-data

## @PathVariable

使用位置：controller方法的参数上

作用：得到url上带的参数

示例：

```java
 	@RequestMapping("show/{id}/{name}")
    public ModelAndView test(@PathVariable("id") Long ids ,@PathVariable("name") String names){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","占位符映射：id:"+ids+";name:"+names);
        mv.setViewName("hello2");
        return mv;
    }
```

## @RequestParam

映射请求参数；

- value：请求参数的参数名；
- required：该参数是否必须，默认是true；
- defaultValue：给请求参数设置默认值；

实例：

```java
//url : /testRequestParam?user=zs@pwd=123
public String testRequestParam(@RequestParam(value="user") String user,@RequestParam(value="pwd") String password){
    System.out.println("username:"+user+" password:"+password);
    return "success";
    }
```

## @ModelAttribute

标记在方法上，会在其他目标方法执行前被SpringMVC调用。