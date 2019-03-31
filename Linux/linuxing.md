##### 1.修改/home目录下的中文目录为英文

1. 修改目录名

   ```
   mv 桌面 desktop
   mv 下载 downloads
   mv 音乐 musics
   mv 公共的 public
   mv 视频 videos
   mv 文档 documents
   mv 图片 pictures
   mv 模板 templates
   ```

2. 修改配置文件

   ```
   sudo gedit ～/.config/user-dirs.dirs
   ```

   sudo 提供root读写权限.

   之后会进入编辑界面，将配置文件中的文件名字修改为第一步修改的文件名即可.

![1547386959864](assets/%5CUsers%5CASUS%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1547386959864.png)

![1547387162554](assets/%5CUsers%5CASUS%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1547387162554.png)

##### 2.centos安装gcc 之前的无法上网问题

* 管理员身份输入命令：`ls /etc/sysconfig/network-scripts`

  ![1551348870348](assets/%5CUsers%5CASUS%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1551348870348.png)

* 输入命令：`vi /etc/sysconfig/network-scripts/ifcfg-ens33`

* 进入编辑界面后按`i` 进入编辑模式

* 修改`BOOTPROTO=dhcp，ONBOOT=yes`

  ![1551348980004](assets/%5CUsers%5CASUS%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1551348980004.png)

* 修改完成后 先按Esc键，再输入冒号（：），然后输入wq，最后按回车键方可退出vim编辑器
* 最后输入命令：`service network restart`
* 安装gcc：`yum install gcc -y`

