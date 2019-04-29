#### npm run dev

```js
// package.json
"dev": "rollup -w -c scripts/config.js --environment TARGET:web-full-dev"
```

#### web-full-dev配置

```js
// 在script/config.js  web-full-dev

'web-full-dev': {
    entry: resolve('web/entry-runtime-with-compiler.js'),
    dest: resolve('dist/vue.js'),
    format: 'umd',
    env: 'development',
    alias: { he: './entity-decoder' },
    banner
}
// 入口文件为 web/entry-runtime-with-compiler.js，最终输出 dist/vue.js
```

#### web关键词  scripts/alias.js

```js
const path = require('path')
const resolve = p => path.resolve(__dirname, '../', p)

module.exports = {
  vue: resolve('src/platforms/web/entry-runtime-with-compiler'),
  compiler: resolve('src/compiler'),
  core: resolve('src/core'),
  shared: resolve('src/shared'),
  web: resolve('src/platforms/web'),
  weex: resolve('src/platforms/weex'),
  server: resolve('src/server'),
  entries: resolve('src/entries'),
  sfc: resolve('src/sfc')
}

web/entry-runtime-with-compiler.js === src/platforms/web/entry-runtime-with-compiler.js

```

#### src/platforms/web/entry-runtime-with-compiler.js   vue的引用文件3

```js
import Vue from './runtime/index' // 导入 运行时 的 Vue
import { compileToFunctions } from './compiler/index' // compiler文件

//这个函数的作用是：获取拥有指定 id 属性的元素的 innerHTML。
const idToTemplate = cached(id => {
  const el = query(id)
  return el && el.innerHTML
})

// 使用 mount 变量缓存 Vue.prototype.$mount 方法
const mount = Vue.prototype.$mount
// 重写 Vue.prototype.$mount 方法
Vue.prototype.$mount = function (){}

/* 获取元素的 outerHTML*/
function getOuterHTML(el){}

// 在 Vue 上添加一个全局API `Vue.compile` 其值为上面导入进来的 compileToFunctions
Vue.compile = compileToFunctions

// 导出 Vue
export default Vue


这个文件运行下来，对 Vue 的影响有两个，
1 重写了 Vue.prototype.$mount 方法；
2 添加了 Vue.compile 全局API，
```

####  platforms/web/runtime/index.js   vue的引用文件2

```js
import Vue from 'core/index'

// install platform specific utils  覆盖默认的 config 对象的属性 vue.config
Vue.config.mustUseProp = mustUseProp
Vue.config.isReservedTag = isReservedTag // 赋值所有的保留标签  /src/platforms/web/util/element.js
Vue.config.isReservedAttr = isReservedAttr
Vue.config.getTagNamespace = getTagNamespace
Vue.config.isUnknownElement = isUnknownElement

在 Vue.options 上添加 web 平台运行时的特定组件和指令。

import platformDirectives from './directives/index'
import platformComponents from './components/index'
extend(Vue.options.directives, platformDirectives)
extend(Vue.options.components, platformComponents)

/*
Vue.options = {
	components: {
		KeepAlive,
		Transition,
		TransitionGroup
	},
	directives: {
		model,
		show
	},
	filters: Object.create(null),
	_base: Vue
}
*/

// 如果在浏览器环境运行的话，这个方法的值为 patch 函数，否则是一个空函数 noop。
Vue.prototype.__patch__ = inBrowser ? patch : noop   
Vue.prototype.$mount = function() {} 在 Vue.prototype 上添加了 $mount 方法


设置平台化的 Vue.config。
在 Vue.options 上混合了两个指令(directives)，分别是 model 和 show。
在 Vue.options 上混合了两个组件(components)，分别是 Transition 和 TransitionGroup。
Vue.prototype.__patch__
Vue.prototype.$mount

```

#### src/core/index.js     vue的引用文件1   添加全局的API

```js
// 为 Vue 添加全局的API，也就是静态的方法和属性。
import Vue from './instance/index'
import { initGlobalAPI } from './global-api/index'

// 将 Vue 构造函数作为参数，传递给 initGlobalAPI 方法，该方法来自 ./global-api/index.js 文件
initGlobalAPI(Vue)  // 挂载全局API

/* initGlobalAPI(Vue)
Vue.config
// 这里有一段注释，大概意思是 Vue.util 以及 util 下的四个方法都不被认为是公共API的一部分，要避免依赖他们
Vue.util = {
	warn,
	extend,
	mergeOptions,
	defineReactive
}
Vue.set = set
Vue.delete = del
Vue.nextTick = nextTick
Vue.observable = <T>(obj: T): T => {
    observe(obj)
    return obj
}
Vue.options = {
	components: {
		KeepAlive
		// Transition 和 TransitionGroup 组件在 runtime/index.js 文件中被添加
		// Transition,
    	// TransitionGroup
	},
	directives: Object.create(null),
	// 在 runtime/index.js 文件中，为 directives 添加了两个平台化的指令 model 和 show
	// directives:{
	//	model,
    //	show
	// },
	filters: Object.create(null),
	_base: Vue
}

// initUse ***************** global-api/use.js
Vue.use = function (plugin: Function | Object) {}

// initMixin ***************** global-api/mixin.js
Vue.mixin = function (mixin: Object) {}

// initExtend ***************** global-api/extend.js
Vue.cid = 0
Vue.extend = function (extendOptions: Object): Function {}

// initAssetRegisters ***************** global-api/assets.js
global-api/assets.js 文件，找到 initAssetRegisters

Vue.component =
Vue.directive =
Vue.filter = function (){}

// expose FunctionalRenderContext for ssr runtime helper installation
Object.defineProperty(Vue, 'FunctionalRenderContext', {
  value: FunctionalRenderContext
})

Vue.version = '__VERSION__'
    
  */

Vue.prototype.$isServer
Vue.prototype.$ssrContext

Vue.FunctionalRenderContext
Vue.__VERSION__

// 导出 Vue
export default Vue
```

#### src/core/instance/index.js   vue的出生文件1    Vue.prototype挂载属性和方法

```js

// 定义 Vue 构造函数   
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

我们大概了解了每个 `*Mixin` 方法的作用其实就是包装 `Vue.prototype`，在其上挂载一些属性和方法
initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

// initMixin(Vue)
Vue.prototype._init = function (options?: Object) {};

 // stateMixin(Vue)
Vue.prototype.$data
Vue.prototype.$props
Vue.prototype.$set = set
Vue.prototype.$delete = del
Vue.prototype.$watch = function (
  expOrFn: string | Function,
  cb: any,
  options?: Object
): Function {}

// eventsMixin(Vue)
Vue.prototype.$on = function (){}
Vue.prototype.$once = function (){}
Vue.prototype.$off = function (){}
Vue.prototype.$emit = function (){}

// lifecycleMixin

Vue.prototype._update = function (){}
Vue.prototype.$forceUpdate = function () {}
Vue.prototype.$destroy = function () {}

// renderMixin
Vue.prototype._o = markOnce
Vue.prototype._n = toNumber
Vue.prototype._s = toString
Vue.prototype._l = renderList
Vue.prototype._t = renderSlot
Vue.prototype._q = looseEqual
Vue.prototype._i = looseIndexOf
Vue.prototype._m = renderStatic
Vue.prototype._f = resolveFilter
Vue.prototype._k = checkKeyCodes
Vue.prototype._b = bindObjectProps
Vue.prototype._v = createTextVNode
Vue.prototype._e = createEmptyVNode
Vue.prototype._u = resolveScopedSlots
Vue.prototype._g = bindObjectListeners

Vue.prototype.$nextTick = function (fn: Function) {}
Vue.prototype._render = function (): VNode {}
// renderMixin
```



### 以一个例子为线索

```js
<div id="app">{{test}}</div>

var vm = new Vue({
    el: '#app',
    data: {
        test: 1
    }
})
```

这段代码的最终效果就是在页面中渲染为如下 `DOM`：

```html
<div id="app">1</div>
```

其中 `{{ test }}` 被替换成了 `1`，并且当我们尝试修改 `data.test` 的值的时候

```js
vm.$data.test = 2
// 或
vm.test = 2
```

那么页面的 `DOM` 也会随之变化为：

```html
<div id="app">2</div>
```



`new` 操作符调用 `Vue` 的时候，第一句执行的代码就是 `this._init(options)`方法，其中 `options` 是我们调用 `Vue` 构造函数时透传过来的，也就是说：

```js
在调用this._init的时候Vue 以及Vue挂载的各种东西 以及挂载好

options = {
    el: '#app',
    data: {
        test: 1
    }
}
```

```js
// _init   this._init(options)  initMixin  instance/init.js
const vm: Component = this   // 声明了常量 vm，其值为 this 是当前的 Vue 实例
vm._uid = uid++  //在实例上添加了一个唯一标示：_uid，可以看到每次实例化一个 Vue 实例之后，uid 的值都会 ++

let startTag, endTag

/* 在非生产环境下，并且 config.performance 和 mark 都为真，那么才执行里面的代码 */
if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
    startTag = `vue-perf-start:${vm._uid}`
    endTag = `vue-perf-end:${vm._uid}`
    mark(startTag)
}

// 中间的代码省略...

/* 在非生产环境下，并且 config.performance 和 mark 都为真，那么才执行里面的代码 */
if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
    vm._name = formatComponentName(vm, false)
    mark(endTag)
    measure(`vue ${vm._name} init`, startTag, endTag)
}

/*
在 `Vue` 的官方文档中可以看到如下内容：`Vue` 提供了全局配置 `Vue.config.performance`，我们通过将其设置为 `true`，即可开启性能追踪，你可以追踪四个场景的性能：

- 1、组件初始化(`component init`)
- 2、编译(`compile`)，将模板(`template`)编译成渲染函数
- 3、渲染(`render`)，其实就是渲染函数的性能，或者说渲染函数执行且生成虚拟DOM(`vnode`)的性能
- 4、打补丁(`patch`)，将虚拟DOM渲染为真实DOM的性能

其中*组件初始化*的性能追踪就是我们在 `_init` 方法中看到的那样去实现的，其实现的方式就是在初始化的代码的开头和结尾分别使用 `mark` 函数打上两个标记，然后通过 `measure` 函数对这两个标记点进行性能计算。`mark` 和 `measure` 这两个函数可以在附录 [core/util 目录下的工具方法全解](http://hcysun.me/vue-design/appendix/core-util.html) 中查看其作用和实现方式。

通过 `core/util/perf.js` 文件的代码我们可知，只有在非生产环境，且浏览器必须支持 `window.performance` API的情况下才会导出有用的 `mark` 和 `measure` 函数
*/

vm._isVue = true   // 标识一个对象是 Vue 实例，这样可以避免该对象被响应系统观测

// merge options 合并options
if (options && options._isComponent) { // _isComponent是在 Vue 创建组件的时候才会有的
    // optimize internal component instantiation
    // since dynamic options merging is pretty slow, and none of the
    // internal component options needs special treatment.
    initInternalComponent(vm, options)
} else { 
    // Vue 实例上添加 $options 属性
    vm.$options = mergeOptions(
    	resolveConstructorOptions(vm.constructor), //Vue.options
    	options || {},  // 我们传进来的对象 
    	vm	// Vue 实例
    )
    /*
    	vm.$options = mergeOptions(
    	
          // resolveConstructorOptions(vm.constructor)
          {
            components: {
              KeepAlive
              Transition,
              TransitionGroup
            },
            directives:{
              model,
              show
            },
            filters: Object.create(null),
            _base: Vue
          },
          
          // options || {}
          {
            el: '#app',
            data: {
              test: 1
            }
          },
          vm
        )

    */
  
    /*
    resolveConstructorOptions(Vue) { //解析构造者的 options Vue.option
        return  Vue.options
    }
    */
    
   /*  mergeOptions   core/util/options.js    
这个函数不仅仅在实例化对象(即_init方法中)的时候用到，在继承(Vue.extend)中也有用到，所以这个函数应该是一个用来合并两个选项对象为一个新对象的通用程序。

		Vue.options = {
            components: {
                KeepAlive
                Transition,
                TransitionGroup
            },
            directives:{
                model,
                show
            },
            filters: Object.create(null),
            _base: Vue
        }



	  */
}

/* 合并之后，我们发现 el 确实还是原来的值，而 data 也确实变成了一个函数，并且这个函数就是我们之前遇到过的 mergedInstanceDataFn，除此之外我们还能看到其他合并后的选项，其中 components、directives、filters 以及 _base 是存在于 Vue.options 中的  */


/* istanbul ignore else */
if (process.env.NODE_ENV !== 'production') {
    initProxy(vm)    // 在vm 上添加 _renderProxy 属性 代理
} else {
    vm._renderProxy = vm
}
// expose real self
vm._self = vm
initLifecycle(vm) // 
initEvents(vm) //   vm 实例对象上添加两个实例属性 _events 和 _hasHookEvent
initRender(vm) //core/instance/render.js 文件 vm.$vnode vm.$slots vm.$scopedSlots


callHook(vm, 'beforeCreate')
initInjections(vm) // resolve injections before data/props

initState(vm) // initState 包括了：initProps、initMethods、initData、initComputed 以及 initWatch。

initProvide(vm) // resolve provide after data/props
callHook(vm, 'created')
```

```js
resolveConstructorOptions  解析构造者的 options

这个函数的作用是用来获取当前实例构造者的 options 属性
export function resolveConstructorOptions (Ctor: Class<Component>) {
   // Ctor 即传递进来的参数 vm.constructor 这个例子中是Vue.options  也不一定都是
  let options = Ctor.options
  if (Ctor.super) { 
     ```super 这个属性是与 Vue.extend 有关系的,这是子类才有的属性
	const Sub = Vue.extend()
	console.log(Sub.super)  // Vue
```

#### mergeOptions core/util/options.js

```js
合并两个选项对象为一个新的对象，这个函数在实例化和继承的时候都有用到，
这里要注意两点：
第一，这个函数将会产生一个新的对象；
第二，这个函数不仅仅在实例化对象(即_init方法中)的时候用到，在继承(Vue.extend)中也有用到，所以这个函数应该是一个用来合并两个选项对象为一个新对象的通用程序。

 if (process.env.NODE_ENV !== 'production') {
    checkComponents(child)
  }


# checkComponents 校验组件
/*
// 校验组件的名字是否符合要求的
function checkComponents (options: Object) {
  for (const key in options.components) {
    validateComponentName(key)
  }
}
export function validateComponentName (name: string) {
  if (!/^[a-zA-Z][\w-]*$/.test(name)) {
    warn(
      'Invalid component name: "' + name + '". Component names ' +
      'can only contain alphanumeric characters and the hyphen, ' +
      'and must start with a letter.'
    )
  }
  if (isBuiltInTag(name) || config.isReservedTag(name)) {
    warn(
      'Do not use built-in or reserved HTML elements as component ' +
      'id: ' + name
    )
  }
}

①：组件的名字要满足正则表达式：/^[a-zA-Z][\w-]*$/
②：要满足：条件 isBuiltInTag(name) || config.isReservedTag(name) 不成立
config.isReservedTag  检测是否是保留标签  在platforms/web/runtime/index.js的时候config.isReservedTag被赋值的。

*/


/* child： option
我们自定义的options = {
    el: '#app',
    data: {
        test: 1
    }
}
说明 child 参数除了是普通的选项对象外，还可以是一个函数，如果是函数的话就取该函数的 options 静态属性作为新的 child,什么样的函数具有 options 静态属性呢？现在我们知道 Vue 构造函数本身就拥有这个属性，其实通过 Vue.extend 创造出来的子类也是拥有这个属性的。
*/

if (typeof child === 'function') {
  child = child.options
}


# 规范化 props（normalizeProps）
normalizeProps(child, vm)

/*
我们在使用 props 的时候有两种写法，这给开发者提供了非常灵活且便利的选择，但是对于 Vue 来讲，这并不是一件好事儿，因为 Vue 要对选项进行处理，这个时候好的做法就是，无论开发者使用哪一种写法，在内部都将其规范成同一种方式，这样在选项合并的时候就能够统一处理

比如如果你的 props 是一个字符串数组：props: ["someData"]
那么经过这个函数，props 将被规范为：
props: {
  someData:{
    type: null
  }

如果你的 props 是对象如下：
props: {
  someData1: Number,
  someData2: {
    type: String,
    default: ''
  }
}

将被规范化为：
props: {
  someData1: {
    type: Number
  },
  someData2: {
    type: String,
    default: ''
  }
}
*/

normalizeInject(child, vm)
/*
inject 选项是这样写的：

['data1', 'data2']
那么将被规范化为：

{
  'data1': { from: 'data1' },
  'data2': { from: 'data2' }
}
*/
normalizeDirectives(child)


# Vue 选项的合并  将相同的key 合并到一起 mergeOptions
const strats = config.optionMergeStrategies
/* config.optionMergeStrategies 还只是一个空的对象 来自于 core/config.js 处理如何将父选项值和子选项值合并到最终值的函数。
也就是说 config.optionMergeStrategies 是一个合并选项的策略对象，这个对象下包含很多函数，这样不同的选项使用不同的合并策略
*/

/* 非生产环境下在 strats 策略对象上添加两个策略(两个属性)分别是 el 和 propsData 两个策略函数是用来合并 el 选项和 propsData 选项的。与其说“合并”不如说“处理” */
if (process.env.NODE_ENV !== 'production') { // 添加合并策略
  strats.el = strats.propsData = function (parent, child, vm, key) {
    if (!vm) { // 判断实例是否存在 如果策略函数中拿不到 vm 参数，那么处理的就是子组件的选项，花了大量的口舌解释了策略函数中判断 vm 的意义，实际上这些解释是必要的。
      warn(
        `option "${key}" can only be used during instance ` +
        'creation with the `new` keyword.'
      )
    }
    return defaultStrat(parent, child)
  }
}


/**
 *它是一个默认的策略，当一个选项不需要特殊处理的时候就使用默认的合并策略
 */
const defaultStrat = function (parentVal: any, childVal: any): any {
  return childVal === undefined
    ? parentVal
    : childVal
}



const options = {}
let key
for (key in parent) {
  mergeField(key)
}
for (key in child) {
  if (!hasOwn(parent, key)) {  
 // 如果 child 对象的键也在 parent 上出现，那么就不要再调用 mergeField 了，因为在上一个 for in 循环中已经调用过了，这就避免了重复调用。
    mergeField(key)
  }
}
### mergeField 
function mergeField (key) {
  const strat = strats[key] || defaultStrat // 没有合并策略的时候使用默认策略
  options[key] = strat(parent[key], child[key], vm, key)
}
return options



### mergeData 

strats.data = function (
  parentVal: any,
  childVal: any,
  vm?: Component
): ?Function {
  if (!vm) {
    if (childVal && typeof childVal !== 'function') {
      process.env.NODE_ENV !== 'production' && warn(
        'The "data" option should be a function ' +
        'that returns a per-instance value in component ' +
        'definitions.',
        vm
      )

      return parentVal
    }
    return mergeDataOrFn(parentVal, childVal)
  }

  return mergeDataOrFn(parentVal, childVal, vm)
}

# mergeDataOrFn
export function mergeDataOrFn (
  parentVal: any,
  childVal: any,
  vm?: Component
): ?Function {
  if (!vm) {
    // in a Vue.extend merge, both should be functions
    if (!childVal) {
      return parentVal
    }
    if (!parentVal) {
      return childVal
    }
    // when parentVal & childVal are both present,
    // we need to return a function that returns the
    // merged result of both functions... no need to
    // check if parentVal is a function here because
    // it has to be a function to pass previous merges.
    return function mergedDataFn () {
      return mergeData(
        typeof childVal === 'function' ? childVal.call(this, this) : childVal,
        typeof parentVal === 'function' ? parentVal.call(this, this) : parentVal
      )
    }
  } else {
    return function mergedInstanceDataFn () {
      // instance merge
      const instanceData = typeof childVal === 'function'
        ? childVal.call(vm, vm)
        : childVal
      const defaultData = typeof parentVal === 'function'
        ? parentVal.call(vm, vm)
        : parentVal
      if (instanceData) {
        return mergeData(instanceData, defaultData)
      } else {
        return defaultData
      }
    }
  }
}





```

#### 



#### initProxy core/instance/proxy.js

```js
initProxy = function initProxy (vm) {
    if (hasProxy) { // 用来判断宿主环境是否支持 js 原生的 Proxy 特性的，如果发现 Proxy 存在，则执行：
        // determine which proxy handler to use
        const options = vm.$options
        const handlers = options.render && options.render._withStripped
        ? getHandler
        : hasHandler
        vm._renderProxy = new Proxy(vm, handlers)
    } else {
        vm._renderProxy = vm
    }
}

initProxy 的作用实际上就是对实例对象 vm 的代理，通过原生的 Proxy 实现。
```

#### initState

```js
export function initState (vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props)
  if (opts.methods) initMethods(vm, opts.methods)
  if (opts.data) {
    initData(vm)  vm._data
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  if (opts.computed) initComputed(vm, opts.computed)
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }
}
```