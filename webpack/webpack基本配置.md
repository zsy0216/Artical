```js
/*
    1. webpack 能够处理JS 文件的相互依赖关系；
    2. webpack 能够处理JS 的兼容问题，把高级的浏览器不识别的语法，转为低级的浏览器能够正常识别的语法；

    webpack ./src/main.js -o ./dist/bundle.js --mode development 

*/

/*
    使用webpack-dev-server这个工具，实现自动打包编译功能
    1.运行 npm i webpack-dev-server -D 安装工具到项目的本地开发依赖
    2.安装完毕后，这个工具的用法和webpack命令的用法一致；
    3.但是，由于这个工具是在项目本地安装的，所以无法将它当做脚本命令在powershell终端中直接运行；
      （只有那些安装到全局 -g 的工具，才能在终端中正常执行）
    4.此时可以在package.json文件中编写以下脚本，通过npm run dev 命令执行；
        "scripts": {
        "dev": "webpack-dev-server"
        },
    5.运行后，只要更改js文件，都会进行自动编译打包，不再需要手动操作；
    （webpack-dev-server 依赖于webpack，所以必须安装webpack）
    6.webpack-dev-server帮我们打包生成的main.js文件并没有存放到实际的物理磁盘中，而是直接托管到了电脑的内存中；
        我们可以认为，打包好的文件，以一种虚拟的形式托管到了项目的根目录中；

*/
```

## webpack 工具集

### top1：`webpack-dev-server`

* 下载命令：`npm i webpack-dev-server -D`

* 使用该工具打包需要在package.json文件中编写以下脚本（并依赖与`webpack`工具）

  ```json
   	"scripts": {
          "dev": "webpack-dev-server"
          //"dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
       },
  // 该脚本还可以指定此工具的其他属性值
  // --open:指定自动打开浏览器运行
  // --port 3000 :指定启动的运行端口
  // --contentBase src ：指定运行的初始目录
  // --hot ：启动热更新功能，每次修改后不需刷新浏览器即可查看更改
  ```

  打包命令：`npm run dev`

  ==此时，通过此种方法打包，该工具自动将打包好的文件以一种虚拟的方式托管到项目根目录中，再通过本地引入方式不再能正常访问打包好的js文件==

* 一种方法是，将引用的js文件地址修改为以根目录开始；	
* 另一种方法是，使用下面的`html-webpack-plugin`插件将整个html页面加载到内存中，并自动在页面最后加入自动打包好的js文件；

### top2：`html-webpack-plugin`

* 安装命令：`npm i html-webpack-plugin -D`

* 使用该插件需要在`webpack.config.js`(在项目根目录)中配置新安装的插件；

  ```js
  //首先声明htmlWebPackPlugin
  var htmlWebPackPlugin = require('html-webpack-plugin')
  
  module.exports = {
  	plugins = [
      	new htmlWebPackPlugin({
  			template: './src/index.html', //指定模板文件
      		filename: 'index.html' //生成的内存页面名称
  		})	
      ],
  }
  ```

* 此时再执行`npm run dev` 即可在内存自动生成html和js文件，不用考虑js的引入路径问题。

### top3：[loaders](<https://www.webpackjs.com/concepts/loaders/>)

#### css样式表的加载器`style-loader`  `css-loader`

* 安装命令：`npm i style-loader css-loader -D`

* 使用loader加载器需要在`webpack.config.js`(在项目根目录)中配置模块属性；

  ```js
  module.exports = {
  	module: {//这个节点用于配置所有第三方模块的加载器
          rules: [ //所有第三方模块的匹配规则
              { test: /\.css$/, use: ['style-loader', 'css-loader']
              }, //配置处理.css文件的处理规则
          ]
      }
  }
  ```

#### less文件的加载器`less-loader`,sass文件的加载器`sass-loader`

* 安装命令：`npm i less-loader less -D` 此处下载less是由于less-loader依赖于less；

* 安装命令：`npm i sass-loader node-sass -D` 此处下载node-sass是由于sass-loader依赖于node-sass；

* 配置：

  ```js
  module: {//这个节点用于配置所有第三方模块的加载器
          rules: [ //所有第三方模块的匹配规则
              { test: /\.css$/, use: ['style-loader', 'css-loader']}, //配置处理.css文件的loader
              { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader']},
              { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader']},
          ]
      }
  ```

  

