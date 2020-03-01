# 1. webpack

>webpack 是一个流行的前端项目构建工具(打包工具), 可以解决当前web开发中所面临的的困境.
>
>webpack 提供友好的模块化支持, 以及代码压缩混淆, 处理 js 兼容问题, 性能优化等强大的功能, 从而让程序员把工作的重心放到具体的功能实现上, 提高了开发效率和项目的可维护性. 

![image-20200228103507184](.\img\image-20200228103507184.png)

## 1.1 基本使用

### 	1.1.1 在项目中安装和配置webpack

- 运行`npm install webpack webpack-cli -D` 命令, 安装 webpack相关的包

- 在项目根目录中, 创建名为 `webpack.config.js` 的webpack 配置文件

- 在webpack 的配置文件中, 初始化如下基本配置

  ```java
  module.exports = {
      mode: 'development' // mode 用来指定构建模式
  }
  // development 模式表示打包的文件不会被压缩 (用于开发环境)
  // production  模式会将打包文件进行压缩 (用于生成环境)
  ```

- 在 package.json 配置文件中 的scripts 节点下, 新增 dev 脚本如下

  ```java
  "scripts":{
      "dev": "webpack" // script 节点下的脚本, 可以通过npm run dev执行
  }
  ```

- 通过 `npm run dev` 执行打包命令

### 1.1.2 配置打包文件的入口和出口

webpack 的 4.X版本中默认约定: 

- 打包的入口文件为 src -> index.js
- 打包的输出文件为 dist -> main.js

```javascript
const path = require('path') // 导入node.js 中专门操作路径的模块
module.exports = {
    entry: path.join(__dirname,'./src/index.js'), // 打包入口文件的路径
    output: {
        path: path.join(__dirname,'./dist'), // 输出文件的存放路径
        filename: 'bundle.js' //输出文件名称
    }
}
```

### 1.1.3 配置webpack 的自动打包功能

- 运行 `npm install webpack-dev-server -D` 命令, 安装支持项目自动打包的工具

- 修改 package.json -> scripts 中的 dev 命令如下:

  ```javascript
  "scripts":{
  	"dev": "webpack-dev-server" // script 节点下的脚本, 可以通过npm run 执行
  }
  ```

- 将 src -> index.html 中, script 脚本的引用路径, 修改为 "/bundle.js"
- 运行 `npm run dev` 命令, 进行重新打包.
- 打开 浏览器, http://localhost:8080/

注意：

- webpack-dev-server 会启动一个实时打包的http 服务器
- webpack-dev-server 打包生成的输出文件, 默认放到了项目根目录中, 而且是虚拟的,看不见的. 

### 1.1.4 配置  html-webpack-plugin 生成预览页面

- 运行 `npm install html-webpack-plugin -D` 命令, 安装生成预览页面的插件

- 修改 `webpack.config.js` 文件头部区域, 添加以下配置信息: 

  ```java
  // 导入生成预览页面的插件, 得到一个构造函数
  const HtmlWebpackPlugin = require('html-webpack-plugin')
  const htmlPlugin = new HtmlWebpackPlugin({ // 创建插件的实例对象
      template:'./src/index.html', // 指定要用到的模板文件
      filename:'index.html' //指定生成的文件的名称, 该文件存在于内存中, 在目录中不显示
  })
  ```

- 修改 `webpack.config.js`文件中向外暴露的配置对象, 新增如下配置节点:

  ```javascript
  module.exports = {
      plugins: [htmlPlugin] // plugin 数组是webpack 打包期间会用到的一些插件列表
  }
  ```

  

### 1.1.5 配置自动打包相关的参数

```javascript
// package.json中的配置
// --open 打包完成后自动打开浏览器界面
// --host 配置IP地址
// --post 配置端口
"scripts":{
    "dev":"webpack-dev-server --open --host 127.0.0.1 --port 8888"
}
```



## 1.2 webpack中的加载器

1. 通过 loader 打包非 js 模块

   在实际项目开发过程中, webpack 默认只能打包处理以 .js 后缀名结尾的模块, 其他非 .js 后缀名结尾的模块, webpack默认处理不了, 需要调用loader 加载器才可以正常打包, 否则会报错! 

	loader 加载器可以协助 webpack 打包处理特定的文件模块, 比如: 
	
	- less-loader 可以打包处理 .less 相关的文件
	- sass-loader 可以打包处理 .scss 相关的文件
	- url-loader 可以打包处理 css 中与 url 路径相关的文件

2. loader 的调用过程

   ![image-20200228153720059](.\img\image-20200228153720059.png)

### 1.2.1 webpack 中加载器的基本使用

1. 打包处理 css 文件

   - 运行 `npm i style-loader css-loader -D` 命令, 安装处理 css 文件的 loader

   - 在 webpack.config.js 的module -> rules 数组中, 添加 loader 规则如下

     ```javascript
     //所有第三方文件模块的匹配规则
     modules: {
         rules:{
             // test表示匹配的文件类型, use 表示对应要调用的loader
             {test: /\.css$/,use:['style-loader','css-loader']}
         }
     }
     ```

     - 注意: use 数组中指定的 loader 顺序是固定的
     - 多个loader 的调用顺序是: 从后往前调用

2. 打包处理 less 文件

   - 运行 `npm i less-loader less -D` 命令, 安装处理 css 文件的 loader

   - 在 webpack.config.js 的 module -> rules 数组中, 添加 loader 规则如下

     ```javascript
     //所有第三方文件模块的匹配规则
     modules: {
         rules:{
             // test表示匹配的文件类型, use 表示对应要调用的loader
             {test: /\.less$/,use:['style-loader','css-loader','less-loader']}
         }
     }
     ```

3. 打包处理 scss 文件

   - 运行 `npm i sass-loader node-sass -D` 命令, 安装处理 scss 文件的 loader

   - 在 webpack.config.js 的 module -> rules 数组中, 添加 loader 规则如下

     ```javascript
     //所有第三方文件模块的匹配规则
     modules: {
         rules:{
             // test表示匹配的文件类型, use 表示对应要调用的loader
             {test: /\.scss$/,use:['style-loader','css-loader','sass-loader']}
         }
     }
     ```

     

4. 配置 postCss 自动添加 css 的兼容前缀

   - 运行 `npm i postcss-loader autoprefixer -D` 命令, 安装处理 scss 文件的 loader

   - 在项目根目录中创建 postcss 的配置文件 postcss.config.js ,并初始化如下配置：

     ```javascript
     const autoprefixer = require('autoprefixer')// 导入自动添加前缀的插件
     module.exports ={
         plugins: [autoprefixer] //挂载插件
     }
     ```

   - 在 webpack.config.js 的 module -> rules 数组中, 修改css 的 loader 规则如下

     ```javascript
     //所有第三方文件模块的匹配规则
     modules: {
         rules:{
             // test表示匹配的文件类型, use 表示对应要调用的loader
             {test: /\.scss$/,use:['style-loader','css-loader','postcss-loader']}
         }
     }
     ```

     

5. 打包样式表中的图片和字体文件

   - 运行 `npm i url-loader file-loader -D` 命令, 安装处理 scss 文件的 loader

   - 在 webpack.config.js 的 module -> rules 数组中, 添加 loader 规则如下

     ```javascript
     modules: {
         rules:{
             {
                 test: /\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
              	use:'url-loader?limit=16940'
             }
         }
     }
     ```

     注意:

     - // 其中 ? 之后的是 loader 的 参数项

     - limit 用来指定图片的大小, 单位是字节(byte), 只有小于limit大小的图片, 才会被转为 base64 图片

       

6. 打包处理 js 文件中的高级语法

   - 安装babel 转换器相关的包: `npm i babel-loader @babel/core @babel/runtime -D`

   - 安装 babel 语法插件相关的包: `npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D`

   - 在项目根目录中, 创建babel 配置文件 babel.config.js 并初始化基本配置如下: 

     ```javascript
     module.exports = {
         presets: ['@babel/preset-env'],
         plugins: ['@babel/plugin-transform-runtime','@babel/plugin-proposal-class-properties']
     }
     ```

   - 在 webpack.config.js 的 module -> rules 数组中, 添加 loader 规则如下:

     ```javascript
     //exclude 为排除项, 表示babel-loader不需要处理 node_modules 中的js
     modules: {
         rules:{
             {
                 test: /\.js$/,use:'babel-loader',exclude: /node_modules/
             }
         }
     }
     ```

     

# 2. Vue单文件组件

## 2.1概述

1. **问题:**
   1. 全局定义的组件必须保证组件的名称不重复
   2. 字符串模板缺乏语法高亮, 在HTML有多行时, 需要用到丑陋的 \
   3. 不支持CSS 意味着当HTML和JavaScript组件化时, CSS明显被遗漏
   4. 没有构建步骤限制, 只能使用HTML和ES5 JavaScript, 而不能使用预处理器 (如:Babel)

2. **解决方案**

   针对传统组件的问题, Vue 提供了一个解决方案 ——使用Vue单文件组件

## 2.2 基本使用

单文件组件的基本组成结构

- template 组件的模板区域
- script 业务逻辑组件
- style 样式区域

```html
<template>
	<!-- 这里用于定义Vue组件的模板内容 -->
</template>
<script>
	// 这里用于定义Vue组件的业务逻辑
</script>
<style scoped>
	/* 这里用于定义组件的样式 */
</style>
```

### 2.2.1 webpack 中配置 vue 组件的加载器

- 运行 `npm i vue-loader vue-template-compiler -D`命令

- 在 webpack.config.js 配置文件中, 添加 vue-loader 的配置项如下: 

  ```javascript
  const VueLoaderPlugin = require('vue-loader/lib/plugin')
  module.exports = {
      module: {
          rules: [
              // ... 其他规则
              {test: /\.vue$/, loader: 'vue-loader'}
          ]
      },
      plugins: [
          // ...其他插件
          new VueLoaderPlugin() // 请确保引入这个插件!
      ]
  }
  ```

  

### 2.2.2 在webpack 项目中使用 vue

1. 运行 `npm i vue -s` 安装 vue

2. 在 src -> index.js 入口文件中, 通过 import Vue from 'vue' 来导入 vue 构造函数

3. 创建 vue 的实例对象, 并指定要控制的 el 区域

4. 通过 render 函数渲染 App 根组件

   ```javascript
   // 1. 导入 Vue 构造函数
   import Vue from 'vue'
   // 2. 导入 App 根组件
   import App from './components/App.vue'
   
   const vm = new Vue({
       // 3. 指定 vm 实例要控制的页面区域
       el: '#app',
       // 4. 通过 render 函数, 把指定的组件渲染到 el 区域中
       render: h => h(App)
   })
   ```

   

### 2.2.3 webpack 打包发布

上线之前要通过webpack将应用进行整体打包, 可以通过 package.json 文件配置打包命令

```javascript
// 在package.json 文件中配置 webpack 打包命令
// 该命令默认加载项目根目录中的 webpack.config.js 配置文件
"scripts": {
    // 用于打包的命令
    "build": "webpack -p",
    // 用于调试的命令
    "dev":"webpack-dev-server --open --host 127.0.0.1 --port 3000"
}
```

