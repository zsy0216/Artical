> 前提：
>
> 1.新建一个github仓库
>
> 2.安装配置Node.js
>
> 3.安装配置Git

# 前提

## 步骤1.新建一个github仓库

1. 打开[github网站](https://github.com/)，（注册）登录账号，[新建一个仓库](https://github.com/new);

注：==仓库名称要求，yourname.github.io；==

​	勾选Initialize this repository with a README；

2. 新建完成后打开仓库，点击右侧settings按钮，开启下面的GitHub Pages；
3. Source保存为master branch，上面的链接是之后博客访问的链接

## 步骤2.安装Node.js

1. 打开[Node.js官网](https://nodejs.org/en/download/)下载对应操作系统版本及位数的node.js

2. 安装完成后可通过以下命令验证是否已配置成功（默认安装已自动配置环境变量）

   ```shell
   node -v
   npm -v
   ```

## 步骤3.安装git

1. 打开[Git官网](https://git-scm.com/downloads)下载对应操作系统版本及位数的git

2. 安装完成后可通过一下命令验证是否配置成功（默认安装已自动配置环境变量,即选择use Git from the Windows Command Prompt）

   ```shell
   git --version
   ```

3. 配置git

   1. 鼠标右键打开git bash here(以下命令都是在git bash下执行)

   2. 设置git的user name和email（第一次使用）

      ```shell
      git config --global user.name "yourname"
      git config --global user.email "youremail"
      ```

   3. 配置ssh

      ```shell
      #生成密钥，默认存储路径：C:\User\Administrator\.ssh
      ssh-keygen -t rsa -C"youremail"
      ```

   4. 在github上添加ssh key.

      ```shell
      #步骤1
      登录github，点击头像下的settings
      #步骤2
      打开左侧的SSH and GPG keys
      #步骤3
      点击右侧的new SSH key
      #步骤4
      Title 自定义
      Key输入刚才生成的C:\User\Administrator\.ssh路径下的id_rsa.pub
      ```

   5. 验证ssh是否添加成功

      ```shell
      ssh -T git@github.com
      ```

   6. ssh-key配置失败解决方法

      ```shell
      首先，清除所有的key-pair
      ssh-add -D
      rm -r ~/.ssh
      删除你在github中的public-key
      
      重新生成ssh密钥对
      ssh-keygen -t rsa -C "xxx@xxx.com"
      
      接下来正常操作
      在github上添加公钥public-key:
      1、首先在你的终端运行 xclip -sel c ~/.ssh/id_rsa.pub将公钥内容复制到剪切板
      2、在github上添加公钥时，直接复制即可
      3、保存
      ```

> 小试牛刀

# hexo+github设置

## 安装Hexo

1. 找到一个合适的位置创建一个新的文件夹，必须是空的。实例文件夹：D:\Blog

2. 打开cmd，进入新建的文件

   ```shell
   d:
   cd Blog
   ```

3. 安装hexo

   ```shell
   npm install hexo -g
   ```

4. 验证是否安装成功

   ```shell
   hexo -v
   ```

## 配置Hexo

1. 初始化Blog文件夹

   ```shell
   hexo init
   ```

2. 安装必要的组件

   ```shell
   npm install
   ```

3. 生成目录结构

   ```shell
   hexo g
   #或
   hexo generate
   ```

4. 开启hexo服务,预览界面

   ```shell
   hexo s
   #或
   hexo server
   ```

5. 第一次访问

   访问：localhost:4000,可看到加载的页面。

## Hexo联系Github

打开hexo配置文件；D:\Blog\\_config.yml

文件末尾处配置：

repository:打开github仓库点击Clone or download，复制里面ssh对应的仓库地址；

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: master
```

之后执行下面的命令进行github的部署

```shell
//安装自动部署工具
npm install hexo-deployer-git --save
//更新
hexo g
//部署
hexo d
```

