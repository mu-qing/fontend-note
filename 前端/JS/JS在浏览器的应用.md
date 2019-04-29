### JavaScript在浏览器中在应用

>  JavaScript本身是一门编程语言，一门语言本身能干什么事，一般取决于它在什么环境，在浏览器中的JS和在node环境中的JS一般有不同的用途，我们今天来探讨一下在浏览器中的JS。

　　浏览器中的页面常常是由HTM，CSS，JavaScript三种语言来构建的。

  - 首先HTML，它并不是一个编程语言，而是一种标记语言，什么标记语言，也就是指定一个字符代表一个意思。并不像编程语言有各种循环，条件等代码逻辑。用不同的标记来区分 文字 图片 等不同的文本。
  -  CSS也不是编程语言，主要是通过选中HTML的选择器属性。给页面添加样式。
  - 　JavaScript有很多功能，比如动态更新内容，比如操作缓存，操作DOM更新CSS样式等等。但是其实，JavaScript本身只有基于ESCAScript标准的语法， 其他的功能，都主要是通过外界提供的API来实现的。

### 什么是API

>  简单来说，就是别人暴露了一个接口，让你可以得到某种服务或者去操作它，比如服务器给前端一个接口，我们就可以通过这个接口拿到数据，这个就是API。在web浏览器中JavaScript一般操作的API主要有二种：**一种是浏览器API**，**第三方 API**



#### 浏览器API

　**内置于Web浏览器中，能从浏览器和电脑周边环境中提取数据，并用来做有用的复杂的事情 。**

##### 常见的浏览器API

- **操作文档的API**: 最明显的例子是[DOM（文档对象模型）](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)API，它允许您操作HTML和CSS — 创建、移除以及修改HTML，动态地将新样式应用到您的页面，等等。
- **从服务器获取数据的API** : 比如**Ajax**
- **用于绘制和操作图形的API**:目前已被浏览器广泛支持 — 最流行的是允许您以编程方式更新包含在HTML [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas) 元素中的像素数据以创建2D和3D场景的[Canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)和[WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API)。

- **音频和视频API**例如[`HTMLMediaElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement)

- **设备API**基本上是以对网络应用程序有用的方式操作和检索现代设备硬件中的数据的API。
- **客户端存储API**：例如使用[Web Storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API)。



#### 第三方 API

**第三方 API** 并没有默认嵌入浏览器中，一般要从网上取得它们的代码和信息。比如：

- [谷歌地图 API](https://developers.google.com/maps/) 和 [高德地图 API](https://lbs.amap.com/) 可以在网站嵌入定制的地图等等。

  

### JavaScript，API和其他JavaScript工具之间的关系

- JavaScript — 一种内置于浏览器的高级脚本语言
- 客户端API — 内置于浏览器的结构程序，位于JavaScript语言顶部，使您可以更容易的实现功能。
- 第三方API — 置于第三方普通的结构程序（例如Twitter，Facebook），使您可以在自己的Web页面中使用那些平台的某些功能（例如在您的Web页面显示最新的Tweets）。
- JavaScript库 — 通常是包含具有[特定功能](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Functions#Custom_functions)的一个或多个JavaScript文件，把这些文件关联到您的Web页以快速或授权编写常见的功能。例如包含jQuery和Mootools
- JavaScript框架 — 从库开始的下一步，JavaScript框架视图把HTML、CSS、JavaScript和其他安装的技术打包在一起，然后用来从头编写一个完整的Web应用。

github地址，持续更新，欢迎star：<https://github.com/mu-qing/JavaScript-advanced/blob/master/src/%E4%BB%80%E4%B9%88%E6%98%AFJS.md>