https://juejin.im/post/5cbfdd8ae51d456e714085e3?tdsourcetag=s_pctim_aiomsg

平时怎么学习新知识，从什么渠道 【博客、掘金、GitHub、StackOverflow等】

开发项目中遇到最困难、最有挑战的事 【PicGo的插件系统】

动态数组如何实现，查找和插入哪个代价大【当时不会】？？？

HTTPS与TLS或者SSL有了解么，加密是对称加密还是非对称加密 【当时不是特别了解 | 二者都有】



算法题2，单链表是否交叉，如何算重叠个数【说了一下思路，但是不是最优解】

算法题1，求数组里最大和的子数组 【思路说了，没写出来】

数组和链表区别和应用场景 【查找-操作】



markdown渲染原理 【正则匹配】

JS内存管机制 【标记-清除】

Vue的原理，简单描述 【Object.defineProperty + Dep + Watcher】

对HTTP/2有没有了解，以及QUIC协议【都了解过，稍微说了一下我的认知】

HTTP缓存，304状态码如何而来【常规题】



- 对前端框架的想法 【从开发效率到后期维护还有工程化角度说了自己的认知】
- 有什么想问的 【问了以后要做啥】





MVVM与Vue的理解 【数据驱动】

vue的双向绑定的原理 【Object.defineProperty + Dep + Watcher】

vue的生命周期以及做了啥，用来干嘛的 【beforeCreate、created、mounted等】

讲讲Virtual Dom 【稍微说了一下理解】

写过render函数么，跟template有啥区别 【写过，说了一下区别】

vue的服务端渲染和客户端渲染区别是啥，服务端渲染作用是啥 【SEO友好，首屏渲染速度等】

简单说说Vue的响应式原理 【Object.defineProperty + Dep + Watcher】

如果Vue2没有实现VirtualDOM，可以做到服务端渲染吗 【可以】

Vue的diff算法如何实现 【说了一下之前自己看过的实现】







讲讲this指针和箭头函数 【常规题】

const和let与var的区别 【常规题】

说说webpack、rollup的tree shanking 【说了tree shanking是啥以及如何实现的】

webpack的loader和plugin区别是啥 【loader处理某一类文件而plugin可以做「任何」事】

promise的finally如何实现 【说了一下我的想法，但是后来想想有点不对】

浏览器和Node端的事件循环的区别 【说了一下我的印象，与setTimeout有关】



算法题：m*n的矩阵，只有0、1，找出最大的只包含1的矩形面积。【说了最蠢的解法...面试官一直引导我也没想出怎么实现更优解】



前端工程化的理解 【流程+规范+自动化等】

对Webpack做了哪些配置来提速 【很多，具体可以参考我[这篇文章](https://link.juejin.im?target=https%3A%2F%2Fmolunerfinn.com%2FWebpack-Optimize%2F)】

一段代码输入babel，把结果再输入babel，结果一样么 【我说应该不一样，但是没说出为什么】

配置过babel哪些属性 【presets，plugins，env等】



说说什么是服务端渲染以及Vue的服务端渲染如何实现 【直出HTML，通过render函数将VirtualDom渲染模板】



【算法题】求两个序列里的最长公共子序列 【稀里糊涂说了一通，好像没错，后来想想其实不对】





学前端的经历？ 【15年开始自学，简单说了一下】

对计算机的体系结构的认知 【懵了，不知道说啥】

有没有经历过jQuery时代 【有】

Webpack优化是怎么做的 【跟一面说的差不多】

上述的优化是基于什么方向去做的 【从cache、减少文件搜索路径、多进程优化等做的】

上述的优化有没有量化出问题（比如看看每块耗时多久等等）再针对性地做优化 【用了profile查看了开发阶段的编译耗时，做了一个简单的[插件](https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2FMolunerfinn%2Fwebpack-dev-compile-optimize)做了开发阶段的速度提升，但是原理我也没说地太清楚】

vue-hot-reload原理是啥 【我只打上来websocket+jsonp做的更新，但是实际上更复杂】

在vue项目里如果我更新了一个js脚本但是页面不更新，我要怎么让vue-hot-reload去更新 【真不会】

在vue项目里如果我更新了一个js，但是不想让页面重新刷新，而只是更新我js的执行部分，我要怎么让vue-hot-reload去更新【真不会】

vue的template是如何转换成render functions的 【说了一下正则匹配，AST，但是不知道是如何有机串起来的】

接上一问，光是正则匹配是无法解决所有问题的，还需要啥，然后怎么做，要哪些阶段？（AST）【说了大概，但是不知道是如何有机串起来的】

AST相关知识掌握程度是多少，去哪里了解的 【不多，相关博客，跑了一些DEMO】

写Electron的时候遇到了哪些问题（解决的或者没解决的都说说）【系统级别的右键菜单实现、插件系统等】

base64怎么编码的 【常规题】

从输入一个地址到浏览器展现网页的过程 【常规题】

DNS查询用TCP来做可以么 【可以，但是慢】

HTTPS握手加密过程 【常规题，这次会了，面头条的时候还不全会】

setTimeout和Promise的异步的区别，在浏览器和Node下的区别 【事件循环的区别，我说了具体的例子】

如何用Jest做的Koa和Vue的测试 【对Koa做了api的测试，对Vue做了界面的单元测试】

CSS和JS哪个更熟悉？【JS】

接上问，CSS的transform有哪些属性 【rotate，translate等】

接上问如何实现一个div既平移又变色？transform的矩阵有了解过么 【没答出transform可以带多个属性，知道矩阵，没写过】

Vue的响应式原理，以及如果一个变量不在页面上出现过（或者使用过），响应式系统是怎么应对的 【render Watcher没有get到这个变量就不会收集它的依赖】

父子组件如何分开收集依赖，或者说父子组件如何确保父组件只收集自己的依赖，子组件只收集自己的依赖 【父子组件有自己的生命周期】

在Watcher内再new一个Watcher后，如何保证依赖收集不会出错 【同一时刻只有一个Watcher在工作】

问一下算法和数据结构掌握程度 → 说一下快排吧 【说了一下快排原理】

你上面跟前端无关的知识都是从哪里获取的 【实验室项目、同学、自己捣鼓、博客等】

你有什么要问我的吗 【这个组杭州和北京的部门做的东西一样么，做什么】



【算法+前端】给定一定数目的粒子，每个粒子有4个属性【位置坐标，半径，速度，加速度】。求问在如下数量下，用什么方式绘制这些运动粒子，用什么数据结构来存储。 

1. 20个 【DOM，Canvas】                                   
2. 500个 【DOM，Canvas】
3. 20000个 → 200w个 【Canvas，但是不够，因为没有必要把200w个点都渲染出来，只需要渲染可视区的。所以问题的关键是如何找到只在可视区出现的圆，这是一道数据结构+算法题。】

接上一题，如果是500个用DOM来绘制的粒子，请问使用Vue或者React的VirtualDom技术来实现，对比用原生操作DOM（假设极致优化）来实现，哪种方案的性能更好。【我说了原生操作，并给出VirtualDom不适合这个例子的理由】

给定一个APP内的营销页面，用户可能在离线状态下打开APP。如果营销页面的图片已经过期了，应该要被撤下，否则会引起歧义。请问用什么办法能够撤下。如果不能用JS，如果用户修改了客户端时间呢？【问题难度一步步加深，先问常规离线模式实现，然后开始不让用JS，并且客户端时间不准确怎么做。没答全。】









#### 第一题

```
实现一个 HardMan:
HardMan("jack") 输出:
I am jack

HardMan("jack").rest(10).learn("computer") 输出
I am jack
//等待10秒
Start learning after 10 seconds
Learning computer

HardMan("jack").restFirst(5).learn("chinese") 输出
//等待5秒
Start learning after 5 seconds
I am jack
Learning chinese
复制代码
```

不难，主要是链式调用要处理好`this`以及用一个`setTimeout`做异步调用任务队列。我没有用ES6的Class实现，用了常规的funciton实现如下：

```
const HardMan = function (name) {
  this.queueList = [() => console.log(`I am ${name}`)]
  this.learn = function (subject) {
    this.queueList.push(() => console.log(`Learning ${subject}`))
    return this
  }
  this.handleTime = function (time) {
    return () => new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log(`Start learning after ${time} second`)
        resolve()
      }, time * 1000)
    })
  }
  this.rest = function (time) {
    this.queueList.push(this.handleTime(time))
    return this
  }
  this.restFirst = function (time) {
    this.queueList.unshift(this.handleTime(time))
    return this
  }
  setTimeout(async () => {
    for (let todo of this.queueList) {
      await todo()
    }
  }, 0)
  return this
}
复制代码
```

#### 第二题

微信小程序团队一共有 n 名成员，决定出去秋游，在海边遇到出租摩托艇的杰克马，马先生手上有 m 辆待出租的摩托艇，价格分别是 b1 、b2 ... bm; 由于习惯了微信支付，团队中每个人身上的现金都有限，分别是 a1 a2 ... an，对了，一起出门的老板还带有 S 元的团队经费，这个经费是每个人都可以使用的

那么考虑以下两个场景

场景1 团队成员都很有爱，都愿意借钱给其他同事，那么这时候团队最多能租到多少摩托艇

```
function max( Array n, Array m, S) {
  return num;
}
复制代码
```

我的答案：

```
// 能借钱说明可以把钱汇总起来从而算出能接多少摩托艇
function max(n, m, S) {
  let sum = n.reduce((a, b) => a + b, 0) + S
  m = m.sort((a,b) => a - b)
  let num = 0
  m.forEach(item => {
    sum -= item
    if (sum >= 0) {
      num++
    }
  })
  return num
}
复制代码
```

场景2 团队成员都十分小气，是不愿意借钱给别人的,那么请考虑以下两个问题

```
//问题一 老板是否能想到一个策略，使得所有人都能租到摩托艇？
function isAll(Array n, Array m, S){
  return bool;
}
复制代码
```

我的答案：

```
// 将摩托艇的费用和个人费用排序使得钱最少的人租最便宜的车，一一对应
// 如果钱不够再向老板借钱
// 直到老板的钱S被借完为止
function isAll (n, m, S) {
  if (m.length < n.length) { // 摩托艇不够
    return false
  }
  m = m.sort((a, b) => a - b)
  n = n.sort((a, b) => a - b)
  let length = n.length
  for (let i = 0; i < length; i++) {
    let diff = n[i] - m[i]
    if (diff < 0) {
      S += diff
      if (S < 0) {
        return false
      }
    }
  }
  return true
}
复制代码
//问题二 请问给出一个策略
// - 使得整个团队租到最多的摩托艇
// - 在租到最多摩托艇的情况下，整体的支出尽量的少
function max( Array n, Array m, S) {
// 采用动态规划解，
return {
  num,// 多少摩托艇
  cost // 总体资金支出
}
复制代码
```

因为时间关系我没有写完。所以跟就把大概写了，这是道背包问题，后面跟面试官说了一下思路。

```
// 一个背包问题，不过我没写出来
// 以dp[i][j]代表第i个人要不要买第j辆车
// 有两种情况
// 1. 第i个人没租车，那么意味着第i - 1个人可能租这辆车
// 2. 第i个人租了车，那么意味着是在i - 1个人租了j - 1辆车的情况下租了j这辆车
// 统计最高的S不为负数的J即为买了最多的车。
// 当J相同的情况下比较S的大小，S越大说明越省钱
// 优化思路：1. 当S小于0的时候就没必要继续算了 2. 一开始两个数组依然像第二题一样排序
复制代码
```

这两个笔试题做完，面试官电话就过来了，简单问了一些问题：

1. 对着笔试题的一些提问，比如第一题的this指针问题，第二题的思路问题 【一遍过】
2. HTTPS建立连接过程 【常规题】
3. 前端缓存的认知 【常规题，缓存的类型，不同缓存的作用等等】
4. 前端安全的认知 【XSS，CSRF等】
5. 有什么想问的吗 【为啥小程序开发者工具用NW.js而不是Electron】

面试官问了大概半小时，就说之后二面的leader会联系我。由于笔试题都做出来，所以感觉还是比较良好的。只是不知道二面来得这么快。



