## Mybatis映射文件中#取值时指定参数相关规则

在#{}中，除了需要的数值外，还可以规定参数的一些其他规则。

例如：javaType，jdbcType，mode（存储过程），numericScale，resultMap,typeHandler,jdbcTypeName,expression;

其中比较常用的为jdbcType。

---

##### jdbcType

​	jdbcType通常在某种特定的条件下被设置：即在我们传入数据为null的时候，有些数据库可能不能识别mybatis对null的默认处理。比如Oracle（报错 ：JdbcType OTHER：无效的类型；）。我们需要在传值后面加上`jdbcType = 类型`，来确保数据库的正确识别。

​	因为mybatis对所有的null都映射的是原生Jdbc的OTHER类型，Oracle不能正确处理，mysql可以正常处理。即产生的主要原因是全局配置中，jdbcTypeForNull的默认值为OTHER，oracle不予支持。

​	所以，除了在#{}里面添加JdbcType的规则，还可以在mybatis全局配置里面添加全局配置：

​	`<setting name="jdbcTypeForNull" value="NULL|VARCHAR|OTHER">`