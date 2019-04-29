### JS模块化

#### 没有组件化

```js
<script src="index.js"></script>
<script src="math.js"></script>


// index.js

function onpress() {
    add()
}

// math.js
function add() {
}

```

**缺点**：

- 需要注意index math导入的顺序
- 变量暴露在全局不安全
- 成员之间变量可能会冲突



#### 使用对象来模块化

```js
<script src="index.js"></script>
<script src="math.js"></script>

// index.js

function onpress() {
    math.add()
}


//  math.js

var math = {
    a: 1,
    add: function() {
       a
    }
}
```

缺点：

- 变量a外部不需要，但是也暴露在外面，容易被修改
- 引入js还是需要看顺序



#### 使用闭包

```js
<script src="index.js"></script>
<script src="math.js"></script>

// index.js

function onpress() {
    math.add()
}

// math.js

var math = (function() {
    var a = 1;
    var add = function() {
        a++
    }
    return {
    	add: add
    }
})();

```

实现math模块按需导入，把所有的模块导入一个全局变量

```js
定义全局变量
var module = {
    exports: {}
}
```

```js

// math.js
var math = (function() {
    var a = 1;
    var add = function() {
        a++
    }
    return {
    	add: add
    }
})();
module.exports.math = math;



// index.js
var math = module.exports.math;
function onpress() {
    math.add()
}


```

**问题**：
index的执行依赖math，只有math执行完在全局注册了自己,index才能用math，这就要求开发者手动维持js的加载顺序

由于加载js时网页会停止渲染 因此需要js文件的异步按需加载

命名空间： 相同的导出依旧会替换之前的内容

解决方案 维护一个文件路径---导出内容的表  根据文件路径加载



#### commonjs（node）

同步加载js脚本，因为文件存在磁盘上 速度很快而浏览器需要异步加载，否则会失去响应 



#### require.js  AMD

```js
require(['m1', 'm2']), function(m1, m2) {
    m1.printName();
    m2.printName();
}

// m1,m2是异步加载，不分顺序，但是1,2 都加载完 才会执行回调
// 这叫前置加载： 执行前必须指定所有的依赖
```



#### CMD

```js
define(function(require, exports, module){
    var foo = require('foo'); // 同步
    foo.add()
    
    require.async('math', function(){
        math.add(1, 2); 异步
    })
})

// sea.js 就近加载 遇到依赖后会去下载js,但是不会执行，而是等所有的依赖下载完，才开始执行，依赖模块的执行顺序和书写顺序一样
```



#### 模块化解决的问题

1 **防止命名冲突** 

**2 解决依赖关系**

**3 模块化代码 方便复用 以及员工之间的分工、**

**4 解决运行阻塞**

