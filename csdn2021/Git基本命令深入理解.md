# 前言

Git 作为一个开源的分布式版本控制系统，在今天的项目版本控制中占有很大的比重。而想要快速简单上手 Git，只需要掌握几个简单的命令就足以应付日常大部分的使用场景。

- `git add .` 
- `git commit -m "commit message"`
- `git push`

> It's so easy.

# 工作区和暂存区

工作区：本地代码存放的目录，在这个文件夹对代码进行操作；

暂存区：版本库中的一个区域；

版本库：工作区中的有一个隐藏目录 `.git` 就是 Git 的版本库；Git 的版本库除了暂存区（称为 stage 或者 index）之外，Git 还会自动创建一个默认分支 `master` （Github 更新后命名为 `main`），以及指向 `master` 的一个指针 `HEAD` ;

![图片来源：廖雪峰官方网站](https://gitee.com/zsy0216/typora-image/raw/master/typora/0)

# 提交步骤

![Git工作区和暂存区](https://gitee.com/zsy0216/typora-image/raw/master/typora/Git工作区和暂存区.png) 

## 1. git add

`git add .`  命令将当前工作区所有被修改的文件添加到暂存区。

- 这里参数 `.` 表示添加所有修改，如果想要添加指定的修改文件可以使用 `git add filename` ；

## 2. git commit

`git commit -m "commit message"` 命令将暂存区的内容提交到当前所在的**本地**分支。

Git 提交格式请参考：[git commit 消息的格式](https://blog.csdn.net/Ep_Little_prince/article/details/108292145) 

## 3. git push

`git push` 命令将本地分支的内容推送到远端仓库的分支（Github、Gitee 或 GitLab）。

- push 推送之前需要提前设置远端仓库地址
  - `git remote add origin git@github.com:zsy0216/xxx.git` 
  - `git push -u origin master` 

