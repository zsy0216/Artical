# Blog project

##  Technology stack 

### backend

* develop language：Java
* develop framework：Spring+SpringMVC+Mybatis
* database：mysql 5.7.22
*  integrated development environment：IDEA 2019.2.3

### background

- 使用jsp视图渲染技术

### front desk

...

## 1. database design

### t_user

| Filed         | Type         | Null | Key     | Extra          |
| ------------- | ------------ | ---- | ------- | -------------- |
| id            | int(11)      | No   | Primary | auto_increment |
| user_name     | varchar(20)  | No   |         | 用户名         |
| user_password | varchar(50)  | No   |         | 用户密码       |
| user_email    | varchar(30)  | No   |         | 用户邮箱       |
| user_nickname | varchar(20)  | No   |         | 用户昵称       |
| user_remark   | varchar(255) |      |         |                |

### t_article

| Filed            | Type         | Null | Key     | Extra          |
| ---------------- | ------------ | ---- | ------- | -------------- |
| id               | int(11)      | No   | Primark | auto_increment |
| user_id          | int(11)      | No   |         | 用户id         |
| article_title    | varchar(50)  | No   |         | 文章标题       |
| article_subtitle | varchar(255) | No   |         | 文章副标题     |
| article_date     | date         | No   |         | 文章创建日期   |
| article_content  | longtext     | No   |         | 文章内容       |
| article_remark   | varchar(255) |      |         |                |

### t_category

| Filed           | Type         | Null | Key     | Extra          |
| --------------- | ------------ | ---- | ------- | -------------- |
| id              | int(11)      | No   | Primary | auto_increment |
| category_name   | varchar(20)  | No   |         | 分类名称       |
| category_alias  | varchar(20)  |      |         | 分类别名       |
| category_remark | varchar(255) |      |         |                |

### t_article_category

| Filed       | Type    | Null | Key     | Extra    |
| ----------- | ------- | ---- | ------- | -------- |
| article_id  | int(11) | No   | Primary | 联合主键 |
| category_id | int(11) | No   | Primary | 联合主键 |

### t_tag

| Filed      | Type         | Null | Key     | Extra          |
| ---------- | ------------ | ---- | ------- | -------------- |
| id         | int(11)      | No   | Primary | auto_increment |
| tag_name   | varchar(20)  | No   |         | 标签名称       |
| tag_alias  | varchar(20)  |      |         | 标签别名       |
| tag_remark | varchar(255) |      |         |                |

### t_article_tag

| Filed      | Type    | Null | Key     | Extra    |
| ---------- | ------- | ---- | ------- | -------- |
| article_id | int(11) | No   | Primary | 联合主键 |
| tag_id     | int(11) | No   | Primary | 联合主键 |

## 2. project developing

### Page Helper

使用步骤：

- 导入依赖

  ```xml
  <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>3.7.5</version>
  </dependency>
  ```

- 配置文件中加入插件

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

- 使用代码：

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

### PageInfo<>对象泛型的转化

正常情况下PageInfo中的泛型必须是数据库的实体类对象，即PO对象。正常开发中不可避免的要使用到其他的包括DTO，VO等对象，在对VO等对象分页时，如果直接返回VO泛型的PageInfo对象，该对象中则没有数据，所以要进一步对PageInfo对象进行封装，转换其泛型。

```java'
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
```

在调用之后，需要对Po和Vo之间的不同属性进行单独赋值，确保Vo对象数据的完整性。

```java
// 封装list到PageInfo对象中进行自动分页
PageInfo<Article> articlePageInfo = new PageInfo<>(articleList);

// 转化PageInfo中的Po到Vo
PageInfo<ArticleVo> articleVoPageInfo = PageInfoUtils.PageInfo2PageInfoVo(articlePageInfo);

List<ArticleVo> articleVos = new ArrayList<>();
    for (Article article : articleList) {
        ArticleVo articleVo = new ArticleVo();
        BeanUtils.copyProperties(article,articleVo);
        User user = userMapper.selectByPrimaryKey(article.getUserId());
        List<Category> categories = articleMapper.selectCatesByArticleId(article.getId());
        List<Tag> tags = articleMapper.selectTagsByArticleId(article.getId());
        String date = DateUtils.fotmat(article.getArticleDate());

        articleVo.setUser(user);
        articleVo.setCategories(categories);
        articleVo.setTags(tags);
        articleVo.setArticleDate(date);
        articleVos.add(articleVo);
    }
for (ArticleVo articleVo : articleVos) {
    articleVoPageInfo.getList().add(articleVo);
}

return articleVoPageInfo;
```

### 前端jsp+bootstrap分页

```jsp
<nav aria-label="Page navigation">
    <ul class="pagination">
        <%-- 判断当前页是否是第一页 --%>
        <c:if test="${pageinfo.isFirstPage}">
            <li class="disabled">
                <a href="javascript:void(0);" aria-label="Previous">
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
        </c:if>
        <c:if test="${!pageinfo.isFirstPage}">
            <li>
                <a href="article/find/search?pageNum=${pageinfo.pageNum-1}" aria-label="Previous">
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
        </c:if>

        <%-- 分页页码显示判断 --%>
        <c:choose>
            <%-- 当总页数不足6页时,显示所有页码 --%>
            <c:when test="${pageinfo.pages<=6}">
                <c:set var="begin" value="1"/>
                <c:set var="end" value="${pageinfo.pages}"/>
            </c:when>
            <%-- 当总页数大于6页时，通过公式算出begin和end --%>
            <c:otherwise>
                <c:set var="begin" value="${pageinfo.pageNum-3}"/>
                <c:set var="end" value="${pageinfo.pageNum+2}"/>
                <%-- 头溢出 --%>
                <c:if test="${begin<1}">
                    <c:set var="begin" value="1"/>
                    <c:set var="end" value="6"/>
                </c:if>
                <%-- 尾溢出 --%>
                <c:if test="${end>pageinfo.pages}">
                    <c:set var="begin" value="${pageinfo.pages-5}"/>
                    <c:set var="end" value="${pageinfo.pages}"/>
                </c:if>
            </c:otherwise>
        </c:choose>
        <c:forEach begin="${begin}" end="${end}" var="i" step="1">
            <c:choose>
                <c:when test="${i eq pageinfo.pageNum}">
                    <li class="active"><a href="article/find/search?pageNum=${i}">${i}</a></li>
                </c:when>
                <c:otherwise>
                    <li><a href="article/find/search?pageNum=${i}">${i}</a></li>
                </c:otherwise>
            </c:choose>
        </c:forEach>
        <%-- 判断当前页是否是最后一页 --%>
        <c:if test="${pageinfo.isLastPage}">
            <li class="disabled">
                <a href="javascript:void(0);" aria-label="Next">
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </c:if>
        <c:if test="${!pageinfo.isLastPage}">
            <li>
                <a href="article/find/search?pageNum=${pageinfo.pageNum+1}" aria-label="Next">
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </c:if>
    </ul>
</nav>
```



