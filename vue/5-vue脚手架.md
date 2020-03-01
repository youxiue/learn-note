# Vue 脚手架

> Vue 脚手架用于快速生成Vue 项目基础架构, 其官网地址为: https://cli.vuejs.org/zh/

## vue脚手架的基本使用

1. 安装 3.x 版本的 Vue 脚手架

   ```powershell
   npm install -g @vue/cli
   ```

2. 基于3.x 版本的脚手架创建vue 项目

   ```powershell
   // 1. 基于 交互式命令行 的方式 ,创建新版 vue 项目
   vue create my-project
   
   // 2. 基于 图形化界面的方式, 创建新版 vue 项目
   vue ui
   
   // 3. 基于 2.x 的旧模板, 创建 旧版 vue 项目
   npm install -g @vue/cli-init
   vue init webpack my-project
   ```

   

## Vue 脚手架的自定义配置

1. 通过package.json 配置项目

   ```javascript
   "vue":{
       "devServer":{
           "port": "8888",
           "open": true
       }
   }
   ```

   不推荐使用这种配置方式. 因为package.json 主要用来管理包的配置信息; 为了方便维护, 推荐将 vue 脚手架相关的配置, 单独定义到 vue.config.js 配置文件中.



2. 通过单独的配置文件配置项目

   - 在项目的根目录创建文件 vue.config.js

   - 在该文件中进行相关配置, 从而覆盖默认配置. 

     ```javascript
     module.exports = {
       devServer: {
         open: true,
         port: 8888
       }
     }
     ```

     

# Element-UI

> 一套为开发者. 设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库.
>
> 官网地址为: https://element.eleme.cn/#/zh-CN

1. 基于命令行方式手动安装

   - 安装依赖包 `npm i element-ui -S`

   - 在 main.js 中导入 Element-UI 相关资源

     ```javascript
     // 导入组件库
     import ElementUI from 'element-ui';
     // 导入组件相关样式
     import 'element-ui/lib/theme-chalk/index.css';
     // 配置 Vue 插件
     Vue.use(ElementUI);
     ```

     

2. 基于图形化界面自动安装
   - 运行 `vue ui` 命令, 打开图形化界面
   - 通过  ==Vue 项目管理器== , 进入具体的项目配置面板
   - 点击 ==插件 -> 添加插件== , 进入插件查询面板
   - 搜索 `vue-cli-plugin-element` 并安装
   - 配置插件, 实现按需导入, 从而减少打包后项目的体积. 