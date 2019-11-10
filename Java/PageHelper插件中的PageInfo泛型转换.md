> 在使用Mybatis查询数据库展示到前端的过程中不可避免的要考虑到分页问题，这时就引入了Mybatis的PageHelper插件，这个插件对分页功能进行了强有力的封装，只需要将查询出来的数据List集合放入到它指定的对象中，就完成了分页，下面简单的对分页过程介绍一下。

## 分页代码

```java
public PageInfo<Po> pageList(Integer pageNum, Integer pageSize){
	// 开启分页
    PageHelper.startPage(pageNum, pageSize);
    List<Po> poList = poMapper.selectXxx();
    // 封装list到PageInfo对象中自动分页
    PageInfo<Po> poPageInfo = new PageInfo<>(poList);
    return poPageInfo;
}
```

- Po是数据库对应的实体类对象；
- PageInfo是需要返回的对象类型，里面封装了包含Po数据的List集合；
- pageNum：指定了查询第几页数据；
- pageSize：指定每一页显示多少条数据；

但一般使用PageHelper，要完成以下步骤：

### 1. 导入依赖包

这里使用的是maven管理。

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>3.7.5</version>
</dependency>
```

### 2. 配置插件

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!-- 配置插件 -->
    <property name="plugins">
        <array>
            <!-- 分页插件 -->
            <bean class="com.github.pagehelper.PageHelper">
                <property name="properties">
                    <props>
                        <!-- 配置数据库方言 -->
                        <prop key="dialect">mysql</prop>

                        <!-- 配置自动修正页码 -->
                        <!-- pageNo的有效范围：1~总页数 -->
                        <prop key="reasonable">true</prop>
                    </props>
                </property>
            </bean>
        </array>
    </property>
</bean>
```

# PageInfo<>对象泛型的转化

既然这样就完成了分页，那么今天讨论的问题到底是干嘛的呢？

是这样的，正常情况下PageInfo中的泛型必须是数据库的实体类对象，即PO对象。正常开发中不可避免的要使用到其他的包括DTO，VO等对象，在对VO等对象分页时，如果直接返回VO泛型的PageInfo对象，该对象中则没有数据，所以要进一步对PageInfo对象进行封装，转换其泛型。

即要对非数据库对应的实体类集合分页时，直接传入list是不生效的，必须对PageInfo进行进一步的处理。

## 泛型转换

这里定义了一个静态的工具方法，用于将PageInfo中的泛型Po对象转换为Vo对象，其他对象类似，只需要传入对应的泛型。

```java
public class PageUtils{
    /**
         * 将PageInfo对象泛型中的Po对象转化为Vo对象
         * @param pageInfoPo PageInfo<Po>对象</>
         * @param <P> Po类型
         * @param <V> Vo类型
         * @return
     */
     public static <P, V> PageInfo<V> PageInfo2PageInfoVo(PageInfo<P> pageInfoPo) {
         // 创建Page对象，实际上是一个ArrayList类型的集合
         Page<V> page = new Page<>(pageInfoPo.getPageNum(), pageInfoPo.getPageSize());
         page.setTotal(pageInfoPo.getTotal());
         return new PageInfo<>(page);
     }
}
```

- 泛型P：Po对象类型；
- 泛型V：Vo或其他需要分页的数据类型；
- Page对象是保存list数据集合，实际上是ArrayList类型；

转换泛型的实质就是给新的`PageInfo<Vo>`对象中的page对象添加pageNum、pageSize、Total等属性。

在得到PageInfo对象后可以使用getList()方法获取存储数据的List集合，进而给PageInfo对象保存需要分页的数据。

## 举例:chestnut:

### 先定义两个对象;

UserPo.java

```java
public class UserPo{
	private Integer id;
    private String userName;
    private String userPassword;
    private Integer addressId;
}
```

UserVo.java

```java
public class UserVo{
	private Integer id;
    private String userName;
    private String addressName;
}
```

### 分页逻辑实现：

UserServiceImpl.java

```java
public class UserServiceImpl implements UserService{
	public pageInfo<UserVo> pageUserVoList(Integer pageNum, Integer pageSize){
    	// 1. 开启分页
        PageHelper.startPage(pageNum, pageSize);
        // 2. 从数据库中查询出
        List<UserPo> UserPoList = UserPoMapper.selectXxx();
        // 3. 封装list到PageInfo对象中自动分页
        PageInfo<Po> userPoPageInfo = new PageInfo<>(UserPoList);
        
        // 4. 转换为UserVo类型的PageInfo对象
        PageInfo<UserVo> UserVoPageInfo = PageUtils.PageInfo2PageInfoVo(userPoPageInfo);
        // 5. 创建需要分页的UserVoList
        List<UserVo> userVoList = new ArrayList<>();
        // 6. 遍历UserPo给UserVoList初始化
        for(UserPo userpo : UserPoList){
        	UserVo uservo = new UserVo();
            // 6.1 将与Po对象相同属性的值赋值到Vo对象中
            BeanUtils.copyProperties(userpo, uservo);
            String addressName = userPoMapper.selectAddressById(userpo.AdrressId).getAddressName();
            uservo.setAddressName(addressName);
            userVoList.add(uservo);
        }
        for (UserVo uservo : userVoList) {
    		userVoPageInfo.getList().add(articleVo);
		}
    	return userVoPageInfo;
    }
}
```

这里主要处理了UserVo与UserPo中不同的数据，真正处理分页的逻辑代码非常简单。只要能够获得需要分页Vo对象的集合，存入到VoPageInfo的List中即可。



