## Webpack 4 : ERROR in Entry module not found: Error: Can't resolve './src'

---

### 不通过配置文件打包方式

webpack4 默认可以不需要在webpack.config.js中来配置打包的入口和出口路径；

默认情况下，

入口为`./src/index.js`文件

出口为`./dist/main.js`文件

目录结构：

![webpack](https://zsy0216.github.io/image/csdn/webpack/webpack.png)

---

确保入口文件`/src/index.js`位置正确，在项目根目录下运行命令：

```shell
webpack
```

即可完成打包。

---

此时可以看到项目目录下的`dist`文件夹下就会打包好的文件`main.js`；

在需要引入的html文件`index.html`中正确引入即可；

![webpack-indexjs](<https://zsy0216.github.io/image/csdn/webpack/webpack-indexjs.png>)

---

也可以使用以下命令手动指定：
`webpack ./src/index.js --output-filename ./dist/main.js --output-path . --mode development`

或者

`webpack ./src/index.js -o ./dist/main.js --mode development` 



==注意==

最后的`--mode development` 是指定开发模式，不指定会报警告；

还有一种mode是`--mode production`指定生产模式，此模式下打包的js文件会进行压缩，前者则不会；

所以上面直接执行`webpack`会提示警告，关闭警告可通过以下命令打包：

`webpack --mode development`

或

`webpack --mode production`

---



### 通过配置文件指定入口出口

`webpack.config.js`(必须在项目根目录下)

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
      path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
```

