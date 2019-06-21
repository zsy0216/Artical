### 更换博客主题

主题可参考：https://hexo.io/themes/ 

hexo默认主题：Landscape

示例主题：Next

#### 下载Next主题

1. 进入Blog所在目录，输入下载命令

   ```shell
   #进入Blog所在目录
   d:
   cd Blog
   #下载
   git clone https://github.com/iissnan/hexo-theme-next themes/next
   ```

2. 启用Next主题

   在Blog文件夹下打开配置文件_config.yml,找到theme字段，将默认的landscape修改为next。

   ```yaml
   # Extensions
   ## Plugins: https://hexo.io/plugins/
   ## Themes: https://hexo.io/themes/
   theme: next
   ```

   保存。

3. 验证

   命令行窗口输入：

   ```shell
   hexo s --debug
   ```

   浏览器输入：localhost:4000查看界面主题。

#### 主题配置

1. 主题配置文件在：/Blog/themes/next/_config.yml

2. 打开主题配置文件，选择next提供的4种子主题,示例启动Mist

   ```yaml
   # ---------------------------------------------------------------
   # Scheme Settings
   # ---------------------------------------------------------------
   
   # Schemes
   #scheme: Muse
   scheme: Mist
   #scheme: Pisces
   #scheme: Gemini
   ```

#### 菜单设置

1. 打开主题配置文件，/Blog/themes/next/_config.yml；

2. 找到menu settings

   ```yaml
   # ---------------------------------------------------------------
   # Menu Settings
   # ---------------------------------------------------------------
   
   menu:
     home: / || home
     about: /about/ || user
     tags: /tags/ || tags
     categories: /categories/ || th
     archives: /archives/ || archive
     schedule: /schedule/ || calendar
     sitemap: /sitemap.xml || sitemap
    # commonweal: /404/ || heartbeat
   
   # Enable/Disable menu icons.
   #icon name(和http://fontawesome.io/icons/ 上面图标name对应) 
   #上方||后面的即为menu_icon名称，不许在下面重复定义
   menu_icons:
     enable: true
   ```

   关闭注释即可打开博客菜单。

3. 创建菜单文件目录，打开菜单后需要创建菜单文件存放的目录；

   一下命令是在命令窗口，Blog目录下执行：D:\Blog>

   ```shell
   #标签菜单
   hexo new page tags
   #分类菜单
   hexo new page categories
   #关于菜单
   hexo new page about
   ```

   执行完上面的命令后，会在Blog/source目录下多出tags和categories文件夹，文件夹下的makedown文件即为标签和分类菜单的展示页面内容，下面为标签和分类添加配置。

   打开/tags/index.md

   ```markdown
   ---
   title: tags
   date: 2019-05-30 14:07:09
   type: "tags"
   ---
   ```

   打开/categories/index.md

   ```markdown
   ---
   title: categories
   date: 2019-05-30 14:07:09
   type: "categories"
   ---
   ```

   保存后，即可在新的文章撰写前添加标签和分类。

   标签分类示例：（新建文件参考上一篇start my blog）

   ```markdown
   ---
   title: my first blog
   date: 2019-05-30 14:07:09
   tags:
   - hexo
   - example
   categories:
   - Example
   ---
   
   the blog content.
   ```

   上面新建文件标签为：`hexo`,`example`;分类为：`Example`.

   其他菜单类似。

#### 侧边栏设置

1. 打开主题配置文件，/Blog/themes/next/_config.yml；

2. 找到sidebar settings

   ```yml
   # ---------------------------------------------------------------
   # Sidebar Settings
   # ---------------------------------------------------------------
   
   # Social Links.
   #取消注释即可打开社交网络链接
   social:
     GitHub: https://github.com/yourname || github
     #E-Mail: mailto:yourname@gmail.com || envelope
     #Google: https://plus.google.com/yourname || google
     #Twitter: https://twitter.com/yourname || twitter
     #FB Page: https://www.facebook.com/yourname || facebook
     #VK Group: https://vk.com/yourname || vk
     #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
     #YouTube: https://youtube.com/yourname || youtube
     #Instagram: https://instagram.com/yourname || instagram
     #Skype: skype:yourname?call|chat || skype
     
   #上方||后面的即为social_icon名称，不许在下面重复定义
   social_icons:
     enable: true
     GitHub: github
     icons_only: false
     transition: false
   ```

3. 头像设置

   在Blog/source目录下新建images文件夹，将自己喜欢的头像放在文件夹下，

   Blog/source/head.jpg，

   在主题配置文件中找到avatar，

   ```yaml
   # Sidebar Avatar
   # in theme directory(source/images): /images/avatar.gif
   # in site  directory(source/uploads): /uploads/avatar.gif
   avatar: /images/head.jpg
   ```

4. 作者和简述及语言

   打开hexo配置文件，/Blog/_config.yml

   ```yaml
   # Site
   title: Tassel   #网站标题
   description: welcome to my blog. #描述
   author: tassel #作者
   language: zh-Hans #语言：简体中文
   ```

#### 首页文章设置

​	控制在首页文章显示字数，next主题默认显示文章所有内容，可以通过以下配置实现缩略显示，并给出显示更多按钮。

1. 方法一：

   在每一篇文章任意位置添加

   ```markdown
   <!-- more -->
   ```

2. 方法二：

   在文章的front-matter（即头部添加标签和分类的地方）中添加description，并提供文章摘录。

3. 方法三：

   自动形成摘要，在主题配置文件中(/Blog/themes/next/config.yml)修改如下配置

   ```yaml
   # Automatically Excerpt. Not recommend.
   # Please use <!-- more --> in the post to control excerpt accurately.
   auto_excerpt:
     enable: true
     length: 150
   ```
   
   

修改完配置后需要重新生成并部署到github
```yaml
hexo clean
hexo generate
hexo deploy
```


其他更多更高级配置参考next主题官方网站：[Next](http://theme-next.iissnan.com/theme-settings.html)

