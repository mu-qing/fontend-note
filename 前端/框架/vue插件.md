插件通常会为 Vue 添加全局功能。插件的范围没有限制——一般有下面几种：

1. 添加全局方法或者属性，如: [vue-custom-element](https://github.com/karol-f/vue-custom-element)
2. 添加全局资源：指令/过滤器/过渡等，如 [vue-touch](https://github.com/vuejs/vue-touch)
3. 通过全局 mixin 方法添加一些组件选项，如: [vue-router](https://github.com/vuejs/vue-router)
4. 添加 Vue 实例方法，通过把它们添加到 Vue.prototype 上实现。
5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能，如 [vue-router](https://github.com/vuejs/vue-router)



### 开发插件

```js
MyPlugin.install = function (Vue, options) {  
  // 1. 添加全局方法或属性  调用Vue.myGlobalMethod()
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }
	
   Vue.$myName = '劳卜';
   // 访问 console.log(Vue.$myName)

  // 2. 添加全局资源 包含了添加全局的指令／过滤器／过渡等
  Vue.directive('focus', {
    bind (el, binding, vnode, oldVnode) {
     el: 指令所绑定的元素，可以用来直接操作 DOM，就是放置指令的那个元素。
     binding: 一个对象
     vnode：Vue 编译生成的虚拟节点。
     oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。
    },
      // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  })

   // <input v-focus>  自动聚焦
    
    
  // 3. 注入组件 全局混入 注册到了每个单一组件上。
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
    
 // 在组件页面访问 this.$myMethod
  // Vue.prototype.$myMethod()
}
```

### 使用插件

```js
import MyPlugin
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

```

```
 Vue.directive
 钩子函数
 bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置

inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 。

componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。

unbind：只调用一次，指令与元素解绑时调用


 
 
 
 
```



# Vue 组件

我们开发的之后期望的结果是支持 import、require 或者直接使用 script 标签的形式引入，就像这样：

**import** CustomUI **from** 'custom-ui';

Vue.use(CustomUI);



# 构建一个 Vue 项目

vue init webpack-simple <project-name>

```
├── src/                           // 源码目录
│   ├── packages/                  // 组件目录
│   │   ├── switch/                // 组件（以switch为例）
│   │   ├── moor-switch.vue        // 组件代码
│   │   ├── index.js               // 挂载插件
│   ├── App.vue                    // 页面入口
│   ├── main.js                    // 程序入口
│   ├── index.js                   // （所有）插件入口
├── index.html                     // 入口html文件
.

```









