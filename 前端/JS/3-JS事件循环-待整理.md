### 为什么JavaScript是单线程？

    众所周知，JavaScript是单线程，单线程就意味着，同一个时间只做一件事。但是why?

JavaScript最初设计是作为浏览器脚本语言，主要用途是与用户交互，以及操作DOM，如果JavaScript同时有两个线程，一个线程在某个DOM节点上添加了内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准，因此，这会带来很复杂的同步问题。

所以，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征。

为了利用多核CPU的计算能力，HTML5提出WebWorker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

#### JavaScript线程如何执行？
JavaScript执行代码是运用call Stack，什么是call Stack,也就是函数栈，具有先进后出的特性，如下：
![](https://user-gold-cdn.xitu.io/2019/3/5/1694cf7211a891de?w=541&h=347&f=gif&s=754382)

#### 栈溢出
![](https://user-gold-cdn.xitu.io/2019/3/5/1694cfbaa84c2f85?w=532&h=299&f=gif&s=338476)


#### JavaScript线程如何异步代码？

我们知道单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。这样用户体验很差。

如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。如下：
![](https://user-gold-cdn.xitu.io/2019/3/5/1694d11021f8aa4b?w=532&h=299&f=gif&s=2345661)


于是JavaScript语言就把任务分类，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。


（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。


但js他如何在单线程的条件下实现的呢？
这就要借助我们的浏览器了，js是单线程的，但是浏览器是多线程的，可以提供很多功能。
#### 浏览器中的 Event Loop


![](https://user-gold-cdn.xitu.io/2019/3/5/1694d1d87b0e839a?w=601&h=527&f=png&s=67266)

heap(堆)：对象会被分配在堆内存中
stack：执行代码的地方
callback:待执行的任务
webAPI: 浏览器提供的一系列功能。

例子：

![](https://user-gold-cdn.xitu.io/2019/3/5/1694d2747d1423ba?w=513&h=296&f=gif&s=1360359)

- 执行console.log('hi'); 遇到请求，将请求事件传给webapi，执行console.log('JSConfEU');
- webapi中的请求结束后，webapi将回调事件放入 taskqueue，等待主线程的执行。
- 主线程执行查看任务队列，将任务入栈，执行回调事件，然后清空栈。- 

更多例子：

![](https://user-gold-cdn.xitu.io/2019/3/5/1694d2d098a479a4?w=643&h=348&f=gif&s=1593248)
![](https://user-gold-cdn.xitu.io/2019/3/5/1694d2f5a65ca62b?w=643&h=348&f=gif&s=2195978)

![](https://user-gold-cdn.xitu.io/2019/3/5/1694d308092359df?w=923&h=505&f=gif&s=2838665)
![](https://user-gold-cdn.xitu.io/2019/3/5/1694d32179d59347?w=923&h=505&f=gif&s=3282193)
![](https://user-gold-cdn.xitu.io/2019/3/5/1694d33901ec8f0d?w=923&h=505&f=gif&s=3292412)

Actually, if video's more your thing, Philip Roberts gave a great talk at JSConf on the event loop 

资料来源：[Philip Roberts: Help, I’m stuck in an event-loop](https://user-gold-cdn.xitu.io/2019/3/5/1694d371eff3c3b1)


##### Micro-Task 与 Macro-Task
浏览器端事件循环中的异步队列有两种：macro（宏任务）队列和 micro（微任务）队列。宏任务队列可以有多个，微任务队列只有一个。

常见的 macro-task 比如：setTimeout、setInterval、script（整体代码）、 I/O 操作、UI 渲染等。
常见的 micro-task 比如: new Promise().then(回调)、MutationObserver(html5新特性) 等。

##### 一个完整的 Event Loop 过程，可以概括为以下阶段：

- 一开始执行栈空,我们可以把执行栈认为是一个存储函数调用的栈结构，遵循先进后出的原则。micro 队列空，macro 队列里有且只有一个 script 脚本（整体代码）

- 全局上下文（script 标签）被推入执行栈，同步代码执行。在执行的过程中，会判断是同步任务还是异步任务，通过对一些接口的调用，可以产生新的 macro-task 与 micro-task，它们会分别被推入各自的任务队列里。同步代码执行完了，script 脚本会被移出 macro 队列，这个过程本质上是队列的 macro-task 的执行和出队的过程。

- 上一步我们出队的是一个 macro-task，这一步我们处理的是 micro-task。但需要注意的是：当 macro-task 出队时，任务是一个一个执行的；而 micro-task 出队时，任务是一队一队执行的。因此，我们处理 micro 队列这一步，会逐个执行队列中的任务并把它出队，直到队列被清空。

- 执行渲染操作，更新界面
- 检查是否存在 Web worker 任务，如果有，则对其进行处理
- 上述过程循环往复，直到两个队列都清空

我们总结一下，每一次循环都是一个这样的过程：

![](https://user-gold-cdn.xitu.io/2019/3/5/1694c4379947b9bc?w=628&h=132&f=png&s=63385)

**当某个宏任务执行完后,会查看是否有微任务队列。如果有，先执行微任务队列中的所有任务，如果没有，会读取宏任务队列中排在最前的任务，执行宏任务的过程中，遇到微任务，依次加入微任务队列。栈空后，再次读取微任务队列里的任务，依次类推。**

例子：

```
Promise.resolve().then(()=>{
  console.log('Promise1')  
  setTimeout(()=>{
    console.log('setTimeout2')
  },0)
})
setTimeout(()=>{
  console.log('setTimeout1')
  Promise.resolve().then(()=>{
    console.log('Promise2')    
  })
},0)
Promise1，setTimeout1，Promise2，setTimeout2
```
- 执行栈的同步任务（这属于宏任务）
- 查看是否有微任务队列，执行微任务队列中的所有任务，目前只有一个，输出Promise1，同时会生成一个宏任务 setTimeout2
- 然后去查看宏任务队列，宏任务 setTimeout1 在 setTimeout2 之前，先执行宏任务 setTimeout1
- 在执行宏任务setTimeout1时会生成微任务Promise2 ，放入微任务队列中，接着先去清空微任务队列中的所有任务，输出 Promise2
- 清空完微任务队列中的所有任务后，就又会去宏任务队列取一个，这回执行的是 setTimeout2

![](https://user-gold-cdn.xitu.io/2019/3/5/1694bd72d2a1d894?w=294&h=270&f=png&s=7828)


```
console.log('script start')

setTimeout(function() {
  console.log('setTimeout')
}, 0)
// setTimeout 延时为 0，其实还是异步。这是因为 HTML5 标准规定这个函数第二个参数不得小于 4 毫秒，不足会自动增加。

console.log('script end')
```
1 执行同步代码  script start  script end  将setTimeout交给定时器线程。
2 执行异步任务setTimeout

```
console.log('script start')
new Promise((resolve, reject) => {
    console.log('async2 end')
    resolve(Promise.resolve())
}).then(() => {
    console.log('async1 end')
})

setTimeout(function() {
    console.log('setTimeout')
}, 0)

new Promise(resolve => {
    console.log('Promise')
    resolve()
})
.then(function() {
    console.log('promise1')
})

console.log('script end')
```
script start => async2 end => Promise =>script end
=> promise1
=> async1 end
=> setTimeout

- 
```
console.log('script start')

setTimeout(function() {
  console.log('setTimeout')
}, 0)

new Promise(resolve => {
  console.log('Promise')
  resolve()
})
  .then(function() {
    console.log('promise1')
  })
  .then(function() {
    console.log('promise2')
  })

console.log('script end')
// 
script start => Promise => script end 
=> promise1 => promise2 
=> setTimeout
```


### 进程与线程
CPU
CPU是计算机的核心，其负责承担计算机的计算任务。

进程
CPU资源分配的最小单位；它代表CPU所能处理的单个任务。任一时刻，CPU总是运行一个进程，其他进程处于非运行状态。
线程是 CPU调度的最小单位。

线程
在早期的操作系统中并没有线程的概念，进程是能拥有资源和独立运行的最小单位，也是程序执行的最小单位。任务调度采用的是时间片轮转的抢占式调度方式，而进程是任务调度的最小单位，每个进程有各自独立的一块内存，使得各个进程之间内存地址相互隔离。后来，随着计算机的发展，对CPU的要求越来越高，进程之间的切换开销较大，已经无法满足越来越复杂的程序的要求了。于是就发明了线程，线程是程序执行中一个单一的顺序控制流程，是程序执行流的最小单元。

进程是操作系统分配资源的最小单位，线程是程序执行的最小单位。
一个进程由一个或多个线程组成，线程是一个进程中代码的不同执行路线；
进程之间相互独立，但同一进程下的各个线程之间共享程序的内存空间(包括代码段、数据集、堆等)及一些进程级的资源(如打开文件和信号)。
调度和切换：线程上下文切换比进程上下文切换要快得多。

**多进程**：在同一个时间里，同一个计算机系统中如果允许两个或两个以上的进程处于运行状态。多进程带来的好处是明显的，比如你可以听歌的同时，打开编辑器敲代码，编辑器和听歌软件的进程之间丝毫不会相互干扰。

**多线程**：程序中包含多个执行流，即在一个程序中可以同时运行多个不同的线程来执行不同的任务，也就是说允许单个程序创建多个并行执行的线程来完成各自的任务。拿到浏览器中来说，当你打开⼀个Tab⻚时，其实就是创建了⼀个进程，`⼀个进程中可以有多个线程`，⽐如渲染线程、JS引擎线程、HTTP请求线程等等。当你发起⼀个请求时，其实就是创建了⼀个线程，当请求结束后，该线程可能就会被销毁




#### 浏览器线程
浏览器是多线程，在内核控制下各线程相互配合以保持同步，一个浏览器通常由以下常驻线程组成：

- GUI 渲染线程，呈现引擎，又称渲染引擎，也被称为浏览器内核，在线程方面又称为 UI 线程，
- JavaScript引擎线程
- 定时触发器线程
- 事件触发线程
- 异步http请求线程

##### GUI渲染线程
- 主要负责页面的渲染，解析HTML、CSS，构建DOM树，布局和绘制等。当界面需要重绘或者由于某种操作引发回流时，将执行该线程。
- 该线程与JS引擎线程互斥，当执行JS引擎线程时，GUI渲染会被挂起，当任务队列空闲时，主线程才会去执行GUI渲染。
- 
  所以，当你需要考虑性能优化时就可以从如上的原因出发，大致有以下几个努力的方面：
  减少 JavaScript 加载对 DOM 渲染的影响（将 JavaScript 代码的加载逻辑放在 HTML 文件的尾部，减少对渲染引擎呈现工作的影响）；
  避免重排，减少重绘（避免白屏，或者交互过程中的卡顿）；
  减少 DOM 的层级（可以减少渲染引擎工作过程中的计算量）；
  使用 requestAnimationFrame 来实现视觉变化（一般来说我们会使用 setTimeout 或 setInterval 来执行动画之类的视觉变化，但这种做法的问题是，回调将在帧中的某个时点运行，可能刚好在末尾，而这可能经常会使我们丢失帧，导致卡顿）；

##### JS引擎线程
- 该线程当然是主要负责处理 JavaScript脚本，执行代码。
- 该线程与 GUI渲染线程互斥，当 JS引擎线程执行 JavaScript脚本时间过长，将导致页面渲染的阻塞。

##### 定时器触发线程
浏览器定时计数器并不是由 JavaScript 引擎计数的, 因为 JavaScript 引擎是单线程的, 如果处于阻塞线程状态就会影响记计时的准确, 因此通过单独线程来计时并触发定时是更为合理的方案；
- 负责执行异步定时器一类的函数的线程，如： setTimeout，setInterval。
- 主线程依次执行代码时，遇到定时器，会将定时器交给该线程处理，当计数完毕后，`事件触发线程`会将计数完毕后的事件加入到任务队列的尾部，等待JS引擎线程执行。

##### 事件触发线程
- 主要负责将准备好的事件交给 JS引擎线程执行。
  比如 setTimeout定时器计数结束， ajax等异步请求成功并触发回调函数，或者用户触发点击事件时，该线程会将整装待发的事件依次加入到任务队列的队尾，等待 JS引擎线程的执行。

##### 异步http请求线程

- 负责执行异步请求一类的函数的线程，如： Promise，axios，ajax等。
- 主线程依次执行代码时，遇到异步请求，会将函数交给该线程处理，当监听到状态码变更，如果有回调函数，`事件触发线程`会将回调函数加入到任务队列的尾部，等待JS引擎线程执行。