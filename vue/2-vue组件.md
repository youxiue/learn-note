# vue组件

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

![Component Tree](https://cn.vuejs.org/images/components.png)

## 1.基础使用

##### 1.1 组件注册

```java
Vue.component('button-counter',{
            data: function () {
                return{
                    count: 0
                }
            },
            template: '<button @click="handle()">点击了{{count}}</button>',
            methods: {
                handle: function () {
                    this.count+=2
                }
            }
        })
```

##### 1.2 组件使用

```java
// 需要被vue 管理
<div id="app">
    <button-counter></button-counter>
</div>
```

##### 1.3 注意事项

1. data必须是一个函数

   ```java
   // 正确写法
   data:function(){ return{ count:0 } }
   // 错误写法
   data:{ count: 0 }
   ```

2. 组件模板内容必须是单个根元素, 不可以是兄弟元素

   ```java
   // 错误写法
   template: '<button @click="handle()">点击了{{count}}</button><button>测试</button>'
   ```

3. 组件模板内容可以是模板字符串(ES6语法)

   ```java
   template: `
                   <div>
                       <button @click="handle()">点击了{{count}}</button>
                       <button>测试</button>
                   </div>
               `
   ```

   



















































