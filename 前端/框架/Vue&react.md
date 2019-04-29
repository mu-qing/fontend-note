# React 和 Vue 两大框架之间的相爱相杀

>  首先先申明一点，不管是 React 还是 Vue，它们都不是 MVVM 框架，只是有借鉴 MVVM 的思路。文中拿 Vue 举例也是为了更好地理解 MVVM 的概念。

- View 很简单，就是用户看到的视图
- Model 同样很简单，一般就是本地数据和数据库中的数据

基本上，我们写的产品就是通过接口从数据库中读取Model ，然后将数据经过处理展现到用户看到的View 上。但是，如何将Model 展示到View 上，然后又如何将用户的输入写入到Model 中，不同的人就产生了不同的看法，从此出现了很多种架构设计。

## MVC 

controller: 控制层,拿到Model 来更新 View ，当用户有输入时，会通过更新Model，并且通知View 进行更新。

![](https://user-gold-cdn.xitu.io/2018/12/20/167cad938817eb7e?w=1341&h=408&f=png&s=40126 'MVC')

##### 缺陷

- 控制器承担的责任太大，随着项目愈加复杂，控制器中的代码会越来越**臃肿**，导致出现不利于**维护**的情况。

- 大量的DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。
- 当 Model 频繁发生变化，开发者需要主动更新到View ；当用户的操作导致 Model 发生变化，开发者同样需要将变化的数据同步到Model 中，这样的工作不仅繁琐，而且很难维护复杂多变的数据状态。

## MVVM

**ViewModel** ：连接Model和View。保证View 和Model 的一致性ViewModel层通过双向数据绑定将View层和Model层连接了起来，使得View层和Model层的同步工作完全是自动的。因此开发者只需关注业务逻辑，无需手动操作DOM，复杂的数据状态维护交给MVVM统一来管理。

对于 MVVM 来说，其实最重要的并不是通过双向绑定或者其他的方式将 View 与 ViewModel 绑定起来，**而是通过 ViewModel 将视图中的状态和用户的行为分离出一个抽象，这才是 MVVM 的精髓**。

![img](https://user-gold-cdn.xitu.io/2018/6/24/1643060c121f7361?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/6/24/16430751f343e886?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### Vue 双向绑定原理

Vue.js 是采用[ Object.defineProperty ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)的 getter 和 setter，并结合观察者模式来实现数据绑定的。当把一个普通 Javascript 对象传给 Vue 实例来作为它的 data 选项时，Vue 将遍历它的属性，用 Object.defineProperty 将它们转为 getter/setter。用户看不到 getter/setter，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化。

![img](https://box.kancloud.cn/04772e55186c0805646fe1bd1696d5d3_647x334.png)

当执行 new Vue() 时，Vue 就进入了初始化阶段，

一方面Vue 会遍历 data 选项中的属性，并用 Object.defineProperty 将它们转为 getter/setter，实现数据变化监听功能；

另一方面，Vue 的指令编译器Compile 对元素节点的指令进行扫描和解析，初始化**视图**，**并订阅Watcher** 来更新View ， 此时Wather 会将自己添加到消息订阅器中(Dep),初始化完毕。

当数据发生变化时，Observer 中的 setter 方法被触发，setter 会立即调用Dep.notify()，Dep 开始遍历所有的订阅者，并**调用订阅者的 update 方法，订阅者收到通知后对View 进行相应的更新。**

## Virtual DOM

运行js的速度是很快的，大量的操作DOM就会很慢，时常在更新数据后会重新渲染页面，这样造成在没有改变数据的地方也重新渲染了DOM 节点，这样就造成了很大成都上的资源浪费。

利用在内存中生成与真实DOM与之对应的JS数据结构，这个在内存中生成的JS结构称之为虚拟DOM 。当数据发生变化时，能够智能地计算出重新喧嚷组件的最小代价并应用到DOM操作上。

那么相较于 DOM 来说，操作 JS 对象会快很多，并且我们也可以通过 JS 来模拟 DOM 

```js
const ul = {
  tag: 'ul',
  props: {
    class: 'list'
  },
  children: {
    tag: 'li',
    children: '1'
  }
}
```

上述代码对应的 DOM 就是

```html
<ul class='list'>
  <li>1</li>
</ul>
```

那么既然 DOM 可以通过 JS 对象来模拟，反之也可以通过 JS 对象来渲染出对应的 DOM。当然了，通过 JS 来模拟 DOM 并且渲染对应的 DOM 只是第一步，难点在于如何判断新旧两个 JS 对象的**最小差异**并且实现**局部更新** DOM。

首先 DOM 是一个多叉树的结构，如果需要完整的对比两颗树的差异，那么需要的时间复杂度会是 O(n ^ 3)，这个复杂度肯定是不能接受的。于是 React 团队优化了算法，实现了 O(n) 的复杂度来对比差异。
实现 O(n) 复杂度的关键就是只对比同层的节点，而不是跨层对比，这也是考虑到在实际业务中很少会去跨层的移动 DOM 元素。
所以判断差异的算法就分为了两步

- 首先从上至下，从左往右遍历对象，也就是树的深度遍历，这一步中会给每个节点添加索引，便于最后渲染差异
- 一旦节点有子元素，就去判断子元素是否有不同

在第一步算法中我们需要判断新旧节点的 `tagName` 是否相同，如果不相同的话就代表节点被替换了。如果没有更改 `tagName` 的话，就需要判断是否有子元素，有的话就进行第二步算法。

在第二步算法中，我们需要判断原本的列表中是否有节点被移除，在新的列表中需要判断是否有新的节点加入，还需要判断节点是否有移动。

举个例子来说，假设页面中只有一个列表，我们对列表中的元素进行了变更

```js
// 假设这里模拟一个 ul，其中包含了 5 个 li
[1, 2, 3, 4, 5]
// 这里替换上面的 li
[1, 2, 5, 4]
```

从上述例子中，我们一眼就可以看出先前的 `ul` 中的第三个 `li` 被移除了，四五替换了位置。

那么在实际的算法中，我们如何去识别改动的是哪个节点呢？这就引入了 `key` 这个属性，想必大家在 Vue 或者 React 的列表中都用过这个属性。这个属性是用来给每一个节点打标志的，用于判断是否是同一个节点。

当然在判断以上差异的过程中，我们还需要判断节点的属性是否有变化等等。

当我们判断出以上的差异后，就可以把这些差异记录下来。当对比完两棵树以后，就可以通过差异去局部更新 DOM，实现性能的最优化。

当然了 Virtual DOM 提高性能是其中一个优势，其实**最大的优势**还是在于：

1. 将 Virtual DOM 作为一个兼容层，让我们还能对接非 Web 端的系统，实现跨端开发。
2. 同样的，通过 Virtual DOM 我们可以渲染到其他的平台，比如实现 SSR、同构渲染等等。
3. 实现组件的高度抽象化

## 前端路由

> 随着 ajax 的流行，异步数据请求交互运行在不刷新浏览器的情况下进行。而异步交互体验的更高级版本就是 SPA —— 单页应用。单页应用不仅仅是在页面交互是无刷新的，连页面跳转都是无刷新的，为了实现单页应用，所以就有了前端路由。本质就是**监听 URL 的变化，然后匹配路由规则，显示相应的页面，并且无须刷新页面。**目前前端使用的路由就只有两种实现方式.

那解决问题的思路便是在改变 url 的情况下，保证页面的不刷新。

- Hash 模式
- History 模式

### Hash 模式

在 2014 年之前，大家是通过 hash 来实现路由, hash 值的变化，并不会导致浏览器向服务器发出请求，浏览器不发出请求，也就不会刷新页面。每次 hash 值的变化，还会触发`hashchange` 这个事件，通过这个事件我们就可以知道 hash 值发生了哪些变化。然后我们便可以监听`hashchange`来更新页面部分内容

### History 模式

History 模式是 14年后HTML5 新推出的功能，主要使用 `history.pushState` 和 `history.replaceState` 改变 URL。通过 History 模式改变 URL 同样不会引起页面的刷新，只会更新浏览器的历史记录。

```js
function matchAndUpdate () {
   // todo 匹配 hash 做 dom 更新操作
}

window.addEventListener('hashchange', matchAndUpdate)
```

用了 HTML5 的实现，单页路由的 url 就不会多出一个#，变得更加美观。但因为没有 # 号，所以当用户刷新页面之类的操作时，浏览器还是会给服务器发送请求。为了避免出现这种情况，所以这个实现需要服务器的支持，需要把所有路由都重定向到根页面。

```js
// 新增历史记录
history.pushState(stateObject, title, URL)
// 替换当前历史记录
history.replaceState(stateObject, title, URL)
```

当用户做出浏览器动作时，比如点击后退按钮时会触发 `popState` 事件

```js
window.addEventListener('popstate', e => {
  // e.state 就是 pushState(stateObject) 中的 stateObject
  console.log(e.state)
})
```

### 两种模式对比

- Hash 模式只可以更改 `#` 后面的内容，History 模式可以通过 API 设置任意的同源 URL
- History 模式可以通过 API 添加任意类型的数据到历史记录中，Hash 模式只能更改哈希值，也就是字符串
- Hash 模式无需后端配置，并且兼容性好。History 模式在用户手动输入地址或者刷新页面的时候会发起 URL 请求，后端需要配置 `index.html` 页面用于匹配不到静态资源的时候

## Vue 和 React 之间的区别

Vue 的表单可以使用 `v-model` 支持双向绑定，相比于 React 来说开发上更加方便，当然了 `v-model` 其实就是个语法糖，本质上和 React 写表单的方式没什么区别。

改变数据方式不同，Vue 修改状态相比来说要简单许多，React 需要使用 `setState` 来改变状态，并且使用这个 API 也有一些坑点。并且 Vue 的底层使用了依赖追踪，页面更新渲染已经是最优的了，但是 React 还是需要用户手动去优化这方面的问题。

React 16以后，有些钩子函数会执行多次，这是因为引入 Fiber 的原因，这在后续的章节中会讲到。

React 需要使用 JSX，有一定的上手成本，并且需要一整套的工具链支持，但是完全可以通过 JS 来控制页面，更加的灵活。Vue 使用了模板语法，相比于 JSX 来说没有那么灵活，但是完全可以脱离工具链，通过直接编写 `render` 函数就能在浏览器中运行。

在生态上来说，两者其实没多大的差距，当然 React 的用户是远远高于 Vue 的。

在上手成本上来说，Vue 一开始的定位就是尽可能的降低前端开发的门槛，然而 React 更多的是去改变用户去接受它的概念和思想，相较于 Vue 来说上手成本略高。
