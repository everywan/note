- [TODO](#todo)
- [FE基础](#fe%E5%9F%BA%E7%A1%80)
  - [源码转换](#%E6%BA%90%E7%A0%81%E8%BD%AC%E6%8D%A2)
  - [Source Map](#source-map)
- [浏览器](#%E6%B5%8F%E8%A7%88%E5%99%A8)
  - [HTML渲染流程](#html%E6%B8%B2%E6%9F%93%E6%B5%81%E7%A8%8B)
- [HTTP协议](#http%E5%8D%8F%E8%AE%AE)
  - [Request/Response](#requestresponse)
    - [Content-Type](#content-type)

## TODO
1. `require()`: 
    - http://www.ruanyifeng.com/blog/2015/05/require.html

## FE基础
### 源码转换
- 相关[Source map](#Source-map)
JS变得越来越复杂, 大部分源码(尤其是各种函数库和框架)都需要经过转换, 才能投入生产环境.

常见的源码转换, 主要是以下三种情况:
1. 压缩/减小体积: 比如jQuery 1.9的源码, 压缩前是252KB, 压缩后是32KB.
2. 多个文件合并, 减少HTTP请求数.
3. 其他语言编译成JavaScript. 最常见的例子就是CoffeeScript.

这三种情况, 都使的浏览器运行的代码与开发代码不同, 从而使debug更麻烦. 通常, JavaScript的解释器会告诉你, 第几行第几列代码出错. 但是, 这对于转换后的代码毫无用处. 举例来说，jQuery 1.9压缩后只有3行, 每行3万个字符, 所有内部变量都改了名字. 你看着报错信息, 感到毫无头绪, 根本不知道它所对应的原始位置.

Source map可以解决此类问题.

### Source Map
- 预先了解 [源码转换](#源码转换)
- [摘抄自: JS Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)

Source Map 是为了解决 [源码转换](#源码转换) 带来的问题.

Source Map 是一个存储源代码与编译代码对应位置映射的信息文件. 当启用 Source Map 时, 除错工具将会直接显示源代码, 而不是转换后的代码.
- 目前只有chrome支持 Source Map.

Source Map 的生成/结构介绍参考[Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)


## 浏览器
### HTML渲染流程
浏览器进程按顺序, 单线程解析HTML, 所以JS执行/下载越耗时间, GUI渲染等待越久. 解决办法：
- `<script defer>` : defer属性：解析到此标签时立即下载, 延迟执行
- 打包下载：每次下载都有HTTP请求开销. （可使用打包工具或者网上打包处理器如yahoo的）
- 动态加载脚本技术：提示：script标签有一个load事件/readystart属性, 可用来检测是否下载完成
    - 动态创建`<script>`标签加载JS
    - AJAX动态加载JS
    - 开源库：Lazyload,  LABjs等等

## HTTP协议
基础: 
1. 一般情况下, query 是指 `url?query`拼接的内容. data/form 是 请求体(body) 里的内容
2. 一般由浏览器发送请求, 浏览器解释 HTML/JS/CSS, 服务端只负责响应请求.(部分爬虫可能需要)

名词解释
1. orgin 指示 request 来自于哪个站点. 该字段仅指示服务器域名, 不包含任何路径信息.

### Request/Response
#### Content-Type
Content-Type: Content-Type 属性指定请求和响应的HTTP内容类型, 服务端根据不同的类型使用不同的方式在不同的位置(data/query)取数据,常见类型如下
- text/html
- text/plain
- text/css
- text/javascript
- application/x-www-form-urlencoded
- multipart/form-data
- application/json
- application/xml
- ...

application/x-www-form-urlencoded
- 表单提交类型, 后台可以从 from 中提取内容. 
- 后台获取的 raw body 是 `name=homeway&key=nokey`

multipart/form-data
- 当文件太长, HTTP 无法在一个包之内发送完毕, 就需要分割数据, 分割成一个一个 chunk 发送给服务端, 这时就是 multipart/form-data 的类型
