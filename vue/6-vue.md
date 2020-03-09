# Vue 常用组件



## 路由导航守卫

```javascript
const router = new VueRouter({
  routes
})
// 挂载路由导航守卫
router.beforeEach((to, from, next) => {
  // to 表示将要访问的路径
  // from 表示从哪里来
  // next 是一个函数 表示放行
  // next() 放行    next('/login') 强制跳转
  if (to.path === '/login') return next()
  // 获取token
  const tokenStr = window.sessionStorage.getItem('token')
  // 如果没有token 表示未登录 跳转到登录页面
  if (!tokenStr) return next('/login')
  // 有token 则放行
  next()
})

export default router
```



## axios 请求拦截

> 通过axios 请求拦截器添加token , 保证拥有获取数据的权限

```javascript
// 在main.js 中导入 axios
import axios from 'axios'
axios.defaults.baseURL = 'http://127.0.0.1:8888/api/private/v1/'
axios.interceptors.request.use(config => {
  // 在请求头上添加 Authorization 里面放入 token 值
  config.headers.Authorization = window.sessionStorage.getItem('token')
  // 在最后必须return
  return config
})
Vue.prototype.$http = axios
// 添加token的方法
window.sessionStorage.setItem('token', res.data.token)
```



## 使用Element-ui 的Message消息提醒

```js
// 引入element-ui 的 消息组件
import { Message } from 'element-ui'
// 将 Element-ui 的消息组件赋值给 Vue的消息组件
Vue.prototype.$message = Message
// 调用方式
this.$message.success('success')
```



## 获取表单对象

```html
// 1. 在表单上添加 ref 属性
<el-form :model="addForm" :rules="addFormRules" ref="addFormRef"></el-form>
// 2. 获取该表单对象
this.$refs.addFormRef
```



## 对表单就行预校验

```javascript
addUser () {
      this.$refs.addFormRef.validate(async valid => {
        if (!valid) return
        // 如果通过检验 可以发起添加请求
        const { data: res } = await this.$http.post('users', this.addForm)
        if (res.meta.status !== 201) {
          this.$message.error('添加用户失败!')
        }
        this.$message.success('添加用户成功!')
          // 关闭表单
        this.addDialogVisible = false
          // 重新获取用户列表
        this.getUserList()
      })
```



## 在列表中获取当前行的对象

> 使用作用域插槽的方式

```html
<!-- 使用 slot-scope 传递作用域, 然后通过 scope.row 获取当前行对象 -->
<template slot-scope="scope">
    <!-- pre 标签会格式化文本 -->
	<pre>{{scope.row}}</pre>
    <el-button type="primary" @click="showEditDialog(scope.row.id)">修改</el-button>
</template>
```

