> 使用hexo+github搭建一个可以外网访问的个人博客，此文用于记录博客初级的使用方法。

新建-编写-生成-部署文章的全过程

### 1、使用cmd完成

1. 打开命令提示符[win+r输入cmd]

2. 切换到自己本地blog目录

3. 创建md文件，以供后面的文章编写

4. 创建好的文件在D:\Blog\source\_posts下，打开后即可进行编辑

5. 编辑完成后进行对文章的页面生成及部署

6. 成功访问https://zsy0216.github.io/

```shell
#1.打开命令提示符cmd
win+r :cmd
#2.切换到本地目录
d:
cd Blog
#3.创建可编辑的文件
hexo new post "blog-name"
#4.进入到文件所在目录双击编辑
#5.对文章进行生成部署
hexo d -g
#5.1.需要的扩展：
npm install hexo-deployer-git
#6.访问
https://zsy0216.github.io/
```

### 2.在git bash下完成

1. 编辑文章同上
2. 发布时进入文章所在目录
3. 生成与部署

```shell
#1.
win+r :cmd
d:
cd Blog
hexo new post "blog-name"
#1.1.进入到文件所在目录双击编辑
#2.进入文章所在目录鼠标右键选择git bash here
#3.生成部署
hexo generate
hexo deploy
```
