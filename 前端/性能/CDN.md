内容分发网络（ CDN ）
CDN 是一个全球分布式网络，它把网站内容更快地传递给全球范围内的一个具体位置，而往往这个具体的位置离实际的内容服务器距离很远。

举个例子，你的网站主机在爱尔兰，而你的用户则在澳大利亚访问。这时当你的用户访问你的网站的时候，延迟会很大，把你的（静态）数据用 CDN 放到澳大利亚则会很大程度上提高用户访问网站的体验。

其实 CDN 可以理解成一个普通缓存，如代理缓存（边缘缓存）。即便你并不关心用户的具体地理位置，你也应该考虑使用 CDN 的代理缓存来提高你的用户体验。

代理缓存会缓存你网站一些页面，通过缓存来传输“静态”内容非常快

请求一个网页   发起http请求  后端读取磁盘文件 操作数据库 PHP 执行都要进行 CPU、内存和 I/O 操作，对于数据库操作也是同样，然后把正确的内容返回给前端

缓存了之后   直接把操作好的页面返回

这些高度活跃的内容（如每小时或者更短时间更新的页面）不能被缓存，或者说不能在缓存中停留时间过长

### DNS 预解析
访问网页的时候，DNS需要把网页的每个域名都通过DNS解析出IP地址。为了优化体验，我们可以提前解析DNS
```
<link rel="dns-prefetch" href="//yuchengkai.cn" />
```