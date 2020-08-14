示例预览：[我的主页](https://zsy0216.github.io/)

### 背景图片添加

1. 自动切换背景

2. 静态本地背景

   首先将已选定的背景图片放到博客根目录下的\source\images下

   ​	示例：`D:\Blog\source\images\background.jpg`

   其次，打开配置文件：`D:\Blog\themes\next\source\css\_custom\custom.styl`

   ```stylus
   // Custom styles.
   body { 
   	background-image: url(/images/background.jpg);
   	background-attachment: fixed;
   	background-repeat: no-repeat;
   	background-size: cover；
   }
   ```

### 界面显示优化

在上面打开的配置文件body{}中继续添加以下配置；(`D:\Blog\themes\next\source\css\_custom\custom.styl`)

```stylus
// Custom styles.
body { 
	background-image: url(/images/background.jpg);
	background-attachment: fixed;
	background-repeat: no-repeat;
	background-size: cover；
    
	//改变背景色和透明度等
	.main-inner {
		background: #fff;
		padding: 25px;
		opacity: 0.75;
		border-radius: 10px;
	}
	
	//修改头部导航栏显示宽度，透明度，位置等
	#header {
	    padding: 5px 25px;
		margin: 0 auto;
		margin-top:15px;
		width: 700px;
		opacity: 0.8;
		border-radius: 10px;
		
	}
	
	//修改底部展示信息显示宽度，透明度，位置等
	#footer {
		padding: 5px 25px;
		position: relative;
		margin: 0 auto;
		margin-bottom: 5px;
		width: 700px;
		opacity: 0.8;
		border-radius: 10px;
	}
}
```

此时，修改完上面的配置可能会发现首页博客主体部分的下方与底部展示信息的间隙过大，我们加入以下配置进行调整，需要注意的是，此时添加的配置与上方添加位置不同，需要在body{}下方添加；

```stylus
body .main {
  margin-bottom: 45px;
}
```

完整配置如下：

```stylus
// Custom styles.
body { 
	background-image: url(/images/background.jpg);
	background-attachment: fixed;
	background-repeat: no-repeat;
	background-size: cover；
	
	
	//改变背景色和透明度
	.main-inner {
		background: #fff;
		padding: 25px;
		opacity: 0.75;
		border-radius: 10px;
	}
	
	#header {
	    padding: 5px 25px;
		margin: 0 auto;
		margin-top:15px;
		width: 700px;
		opacity: 0.8;
		border-radius: 10px;
		
	}
	
	#footer {
		padding: 5px 25px;
		position: relative;
		margin: 0 auto;
		margin-bottom: 5px;
		width: 700px;
		opacity: 0.8;
		border-radius: 10px;
	}
}

body .main {
  margin-bottom: 45px;
}

```

