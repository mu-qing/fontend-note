CSS居中

**内联元素：**

- **单行文本**：1 父元素：text-align:center  垂直height=line-height    2  padding  
- **多行文字**： 1 四边padding      2 设置为一个块元素，当做块元素来定位

**块级元素：**

- margin：0 auto
- 父元素relative，自身absolute ,left:50%  margin-left：-width/2
- **calc**: position: relative;  top: calc(50% - width/2px);, calc() 函数用于动态计算长度值。**（IE9+等）**

- 父元素relative, 自身absolute,left:50%  2 自身 transform: translate(-50%,0);（**IE9+等）**

**flex【通用】**

**元素：**display: flex; align-items: center; justify-content: center;**（IE9+等）**





### CSS盒模型

CSS中盒模型包括IE盒模型和标准盒模型（W3C）。

标准盒宽度：width = 左右border+左右padding+contentIE模型宽度：width = content

在**CSS3**中引入了box-sizing属性

box-sizing:content-box;表示标准的盒子模型，

box-sizing:border-box表示的是IE盒子模型

box-sizing:padding-box,width =左右padding+content





### link标签和import标签的区别

- link属于html标签，可以引入各种资源，而import是CSS，只能导入CSS
- 页面被加载时.link被加载，@import引用的css会等到页面加载结束后加载。
- link是html标签，因此没有兼容性，而@import只有IE5以上才能识别。
- link方式样式的权重高于@import的。



###  

### 块元素和行元素

块元素：独占一行，并且有自动填满父元素，可以设置margin和pading以及高度和宽度
行元素：不会独占一行，width和height会失效，并且在垂直方向的padding和margin会失效。





### 多行元素的文本省略号  

```
display: -webkit-box
-webkit-box-orient:vertical
-web-line-clamp:3
overflow:hidden
```





### visibility:hidden, opacity:0，display:none的区别



**opacity:0**：该元素透明度为0，不会影响页面布局，绑定的事件依然可以触发。

**visibility:hidden**：该元素隐藏起来了，还在DOM树中，绑定的事件失效

**display:none**：元素直接从DOM树移除，不占任何空间。



 

### 双边距重叠问题（外边距折叠）  

  多个相邻普通流的块元素垂直方向marigin会重叠 

 

  折叠的结果为： 

 

  两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
 两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
 两个外边距一正一负时，折叠结果是两者的相加的和。





**说说rem与em的区别**

rem是根据根的font-size变化，em是根据父级的font-size变化





**css sprite是什么，有什么优缺点**

就是将多个小图标拼接在一张图片上，使用 background-size来定位到相关图片上

**优点**：减少HTTP请求数，极大地提高页面加载速度

**缺点：**当图片过多是，维护困难





###### 清除浮动的几种方式

1.clear：both，添加一个空标签div
2.父级div定义伪类:after和zoom
3.父级div定义overflow:hidden
4.父级div也浮动，需要定义宽度
5.结尾处加br标签clear:both



**有关border:none以及border:0的区别**



```
当定义了border:none，即隐藏了边框的显示，实际就是边框宽度为0
```

性能差异

border:0;浏览器对border-width、border-color进行渲染，占用内存。

border:none;浏览器不进行渲染，不占用内存。



**为什么要初始化css样式**

因为不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化





###### css的优先级算法是怎么样的

优先级为: !important > id > class > tag
important 比 内联优先级高



**浏览器标准模式和怪异模式之间的区别是什么**

所谓的标准模式是指，浏览器按W3C标准解析执行代码；怪异模式则是使用浏览器自己的方式解析执行代码，因为不同浏览器解析执行的方式不一样，所以我们称之为怪异模式。浏览器解析时到底使用标准模式还是怪异模式，与你网页中的DTD声明直接相关，DTD声明定义了标准文档的类型（标准模式解析）文档类型，会使浏览器使用相应的方式加载网页并显示，忽略DTD声明,将使网页进入怪异模式(quirks mode)。





###### 渐进增强和优雅降级

渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容



#### 怎么实现三角形，为什么会有这种现象

将元素宽高设为0，然后边框只保留一边，另外三边设为透明。画了个图说明原因。





#### 怎么对三角形设置阴影

我说用filter中的drop-shadow属性，他问为什么，还有什么做法，我想了一下说可以直接再写个三角形放在下面，还可以写个矩形然后旋转下





###### css3有哪些新特性

新增各种css选择器
圆角 border-radius
多列布局
阴影和反射
文字特效text-shadow
线性渐变
旋转transform
动画效果



**讲讲viewport和移动端布局**

可以参考我的这篇文章：

[响应式布局的常用解决方案对比(媒体查询、百分比、rem和vw/vh）](https://github.com/forthealllight/blog/issues/13)



### click在ios上有300ms延迟，原因及如何解决？  

**原因：**追溯至 2007 年初。苹果公司在发布首款 iPhone 前夕，遇到一个问题 —— 当时的网站都是为大屏幕设备所设计的。于是苹果的工程师们做了一些约定，应对 iPhone 这种小屏幕浏览桌面端站点的问题。这当中最出名的，当属双击缩放(double tap to zoom)】

so移动浏览器上支持**双击缩放**操作，以及IOS Safari 上的**双击滚动**操作，是导致300ms的点击延迟主要原因。

当用户一次点击屏幕之后，浏览器并不能立刻判断用户是要进行双击缩放，还是想要进行单击操作。因此，iOS Safari 就等待 300 毫秒，以判断用户是否再次点击了屏幕，如果没点才真正触发点击事件。

**解决：**

####   (1)粗暴型，禁用缩放  











 





```
<meta name="viewport" content="width=device-width, user-scalable=no">
```





#### (2)利用FastClick，其原理是：  

  检测到touchend事件后，会通过DOM自定义事件，立刻触发一个模拟click的事件，并把浏览器300毫秒之后真正触发的事件给阻断掉。

也就是不用原生的点击事件了，直接搞一个自定义的，然后把原生的给屏蔽掉。

**扩展解析：**在移动端，手指点击一个元素，会经过：touchstart --> touchmove -> touchend --》click。



 

###   2.画一条0.5px的线  

-   

-    

  ​    采用meta viewport的方式   

     

  ​    

-    

  ​    采用 border-image的方式   

  ​    

-    采用transform: scale()的方式   

 

###  

 

###   [4.transition和animation的区别]()   

  [Animation和transition大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是transition需要触发一个事件才能改变属性，而animation不需要触发任何事件的情况下才会随时间改变属性值，并且transition为2帧，从from .... to，而animation可以一帧一帧的。]()  

 

###   [5.Flex布局]()   

  [文章链接：
 ]()http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool（语法篇）
 http://www.ruanyifeng.com/blog/2015/07/flex-examples.html（实例篇）  

 

  Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
 布局的传统解决方案，基于盒状模型，依赖 display属性 + position属性 + float属性。它对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。 

 

  简单的分为容器属性和元素属性
 容器的属性： 

 

-   

-    flex-direction：决定主轴的方向（即子item的排列方法）
   .box {
   flex-direction: row | row-reverse | column | column-reverse;
   }    
-    flex-wrap：决定换行规则
   .box{
   flex-wrap: nowrap | wrap | wrap-reverse;
   }    
-    flex-flow：
   .box {
   flex-flow: <flex-direction> || <flex-wrap>;
   }    
-    justify-content：对其方式，水平主轴对齐方式    
-    align-items：对齐方式，竖直轴线方向   

 

  项目的属性（元素的属性）： 

 

-   

-    order属性：定义项目的排列顺序，顺序越小，排列越靠前，默认为0    
-    flex-grow属性：定义项目的放大比例，即使存在空间，也不会放大    
-    flex-shrink属性：定义了项目的缩小比例，当空间不足的情况下会等比例的缩小，如果定义个item的flow-shrink为0，则为不缩小    
-    flex-basis属性：定义了在分配多余的空间，项目占据的空间。    
-    flex：是flex-grow和flex-shrink、flex-basis的简写，默认值为0 1 auto。    
-    align-self：允许单个项目与其他项目不一样的对齐方式，可以覆盖align-items，默认属性为auto，表示继承父元素的align-items   

 

  比如说，用flex实现圣杯布局 

 

###   6.BFC（块级格式化上下文，用于清除浮动，防止margin重叠等）  

  直译成：块级格式化上下文，是一个独立的渲染区域，并且有一定的布局规则。 

 

-   

-    BFC区域不会与float box重叠    
-    BFC是页面上的一个独立容器，子元素不会影响到外面    
-    计算BFC的高度时，浮动元素也会参与计算   

 

  那些元素会生成BFC： 

 

-   

-    根元素    
-    float不为none的元素    
-    position为fixed和absolute的元素    
-    display为inline-block、table-cell、table-caption，flex，inline-flex的元素    
-    overflow不为visible的元素   

a、BFC（Block Formatting Context）即“块级格式化上下文”， IFC（Inline Formatting Context）即行内格式化上下文。常规流（也称标准流、普通流）是一个文档在被显示时最常见的布局形态。一个框在常规流中必须属于一个格式化上下文，你可以把BFC想象成一个大箱子，箱子外边的元素将不与箱子内的元素产生作用。

b、BFC是W3C CSS 2.1 规范中的一个概念，它决定了元素如何对其内容进行定位，以及与其他元素的关系和相互作用。当涉及到可视化布局的时候，Block Formatting Context提供了一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。也可以说BFC就是一个作用范围。

c、在普通流中的 Box(框) 属于一种 formatting context(格式化上下文) ，类型可以是 block ，或者是 inline ，但不能同时属于这两者。并且， Block boxes(块框) 在 block formatting context(块格式化上下文) 里格式化， Inline boxes(块内框) 则在 Inline Formatting Context(行内格式化上下文) 里格式化。

(2)、如何产生BFC
当一个HTML元素满足下面条件的任何一点，都可以产生Block Formatting Context：
a、float的值不为none
b、overflow的值不为visible
c、display的值为table-cell, table-caption, inline-block中的任何一个
d、position的值不为relative和static

CSS3触发BFC方式则可以简单描述为：在元素定位非static，relative的情况下触发，float也是一种定位方式。

(3)、BFC的作用与特点
a、不和浮动元素重叠，清除外部浮动，阻止浮动元素覆盖

如果一个浮动元素后面跟着一个非浮动的元素，那么就会产生一个重叠的现象。常规流（也称标准流、普通流）是一个文档在被显示时最常见的布局形态，当float不为none时，position为absolute、fixed时元素将脱离标准流。





 

###   8.关于js动画和css3动画的差异性  

  渲染线程分为main thread和compositor thread，如果css动画只改变transform和opacity，这时整个CSS动画得以在compositor trhead完成（而js动画则会在main thread执行，然后出发compositor thread进行下一步操作），特别注意的是如果改变transform和opacity是不会layout或者paint的。
 区别： 

 

-   

-    功能涵盖面，js比css大    
-    实现/重构难度不一，CSS3比js更加简单，性能跳优方向固定    
-    对帧速表现不好的低版本浏览器，css3可以做到自然降级    
-    css动画有天然事件支持    
-    css3有兼容性问题

 

###  

有三个元素，第一个与第三个宽度都为100px，中间元素占用剩余空间，怎么做到中间元素随着浏览器宽度的变化而变化

flex布局：父元素

display: flex;

justify-content: space-around;

一三设置：min-width: 100px;

二： width：200%



**base64的原理及优缺点**

**原理**：base64是一种用于传输8Bit字节码编码方式，基于64个可打印字符来标识二进制数据， Base64编码指的是从二进制转到字符的过程

**产生原因：**因为网络传送渠道并不支持所有的字节，例如传统的邮件只支持可见字符的传送，像ASCII码的控制字符就不能通过邮件传送，这样就受到了很大的限制，比如图片二进制流的每个字节不可能全部是可见字符，所以就传送不了，而Base64就是一种基于64个可见字符来表示二进制数据的表示方法，把不可见字符用可见字符来表示。

**扩展：不可见字符其实并不是不显示，只是这些字符在屏幕上显示不出来，比如：换行符、回车、退格......字符。**



**优点**: 可以将二进制数据转化为可打印字符，方便传输数据，对数据进行简单的加密，肉眼安全。

**缺点**：内容编码后体积变大，CPU编码和解码需要额外工作量。