###css相关基础知识整理

1. css 盒模型
  * 基本概念：标准模型 + IE 模型
    标准模型：margin、border、padding、content，height 和 width 是 content 的区域

    IE 模型： margin、border、padding、content，height 和 width 是 content + padding + border 的区域

  * 标准模型和 IE 模型区别
    区别是 height 和 width 计算方式不同

  * css 如何设置这两种模型
    box-sizing: content-box(标准模型)、border-box(IE6 之前的版本模型)

  * JS 如何设置获取盒模型对应的宽盒高
    dom.style.height/width 只能取出内联样式的宽和高

    dom.currentStyle.width/height 得到的是渲染以后的宽和高 (只有 IE 支持)
    
    window.getComputedStyle(dom).width/height 通用性更好

    dom.getBoundingClientRect().width/height 

    getBoundingClientRect 用于获取某个元素相对于视窗的位置集合，{top, right, bottom, left, height, width}等参数


  * 根据盒模型解释边距重叠
    · BFC 的基本概念
    父子元素的上下边距会重叠
    兄弟元素的上下边距会重叠

    · BFC 的原理
    1. BFC 的垂直方向元素边距会发生重叠
    2. BFC 的区域不会与浮动元素的 box 重叠 （清除浮动）
    3. BFC 在页面上是一个独立的容器，外面的元素不会影响里面的元素，反过来也一样
    4. 计算 BFC 高度的时候，浮动元素也会参与计算

    · 如何创建 BFC
    1. float 值不为 none
    2. position 值不是 static 或 relative
    3. display 值 table、table-cell、table-caption、inline-block
    4. overflow 值 hidden、visible 

    · BFC 的使用场景


  * BFC(边距重叠解决方案)
    demo: box.html
  
2. DOM 事件类
  * 基本概念：DOM 事件的级别
  
  |DOM事件类|事件级别|
  |:---|:---|
  |DOM0|element.onclick=function(){}|
  |DOM2|element.addEventListener('click', function(){}, false)|
  |DOM3|element.addEventListener('keyup',function(){},false)|

  * DOM 事件模型 (冒泡和捕获)
    捕获是从上往下
    冒泡是从下往上

  * DOM 事件流
    · 第一阶段是捕获
    · 第二阶段是目标阶段
    · 第三阶段是冒泡

  * 描述 DOM 事件捕获的具体流程
    · 捕获：window => document => html => body => ... => 目标元素

    · 冒泡：目标元素 => ... => body => html => document => window

  * Event 对象的常见应用
    * event.preventDefault() 阻止默认事件
    * event.stopPropagation() 阻止冒泡
    * event.stopImmediatePropagation()  如果有多个相同类型事件的事件监听函数绑定到同一个元素，当该类型的事件触发时，它们会按照被添加的顺序执行。如果其中某个监听函数执行了 event.stopImmediatePropagation() 方法，则当前元素剩下的监听函数将不会被执行。
    * event.currentTarget 是指定的父级元素
    * event.target

  * 自定义事件
    ```
      var eve = new Event('custome);
      ev.addEventListener('custome', function() {
        console.log('custome');
      })
      ev.dispatchEvent(eve);

      // CustomEvent
    ```
  
3. 类型转换 
  * 数据类型
    原始类型
       Boolean Null Undefined Number String Symbol
    对象
      Object
  * 显式类型转换
    Number 函数

      原始类型转换
        数值：转换后还是原来的值
        字符串：如果可以被解析为数值，则转换为相应的数值，否则得到NaN。空字符串转为 0
        布尔值：true 转成1，false 转成0。
        undefined：转成 NaN。
        null：转成0。

      对象类型转换
        先调用对象自身的 valueOf 方法，如果该方法返回原始类型的值(数值、字符串和布尔值)，则直接对该值使用 Number 方法，不再进行后续步骤。
        如果 valueOf 方法返回复合类型的值，再调用对象自身的 toString 方法，如果 toString 方法返回原始类型的值，则对该值使用 Number 方法，不再进行后续步骤。
        如果 toString 方法返回的是复合类型的值，则报错。

    String 函数
      原始类型转换
        数值：转为相应的字符串。
        字符串：转换后还是原来的值。
        布尔值：true 转为 'true'，false 转为 'false'。
        undefined：转为 'undefined'。
         null：转为 'null'。
      
      对象类型转换
        先调用 toString 方法，如果 toString 方法返回的是原始类型的值，则对该值使用 String 方法，不再进行以下步骤。
        如果 toString 方法返回的是复合类型的值，再调用 valueOf 方法，如果 valueOf 方法返回的是原始类型的值，则对该值使用 String 方法，不再进行以下步骤。
        如果 valueOf 方法返回的是复合类型的值，则报错。

    Boolean 函数
      原始类型转换
        undefined、null、-0、+0、NaN、'' 都转成 false

  隐式类型转换：
    四则运算
    判断语句
    Native 调用 (alert, log)
  
  常见题目
  [] + [] => ''
  [] + {} => '[object Object]'
  {} + [] => 0
  {} + {} => 1. chrome =>'[object Object][object Object]
  ' 2.  firefox => NaN
  true + true => 2
  1 + {a: 1} => '1[object Object]'

  typeof 
  undefined => 'undefined'
  Null => 'object'
  Boolean => 'boolean'
  Number => 'number'
  String => 'string'
  Symbol(ECMAScript 2015) => 'symbol'
  Host object(provided by the JS environment) => implementation-dependent
  Function object => 'function'
  Any other object => 'object'

4. HTTP 协议类
  * HTTP 协议的主要特点
    * 简单快速：每个 uri 资源是固定的，
    * 灵活：通过一个 HTTP 协议可以完成不同类型的数据传输，在 header 里设置
    * 无连接：连接一次就会断开
    * 无状态：不能区分两次连接者的身份

  * HTTP 报文的组成部分
    请求报文 => 请求行、请求头、空行、请求体
    响应报文 => 状态行、响应头、空行、响应体
  * HTTP 方法
    GET => 获取资源
    POST => 传输资源
    PUT => 更新资源
    DELETE => 删除资源
    HEAD => 获得报文首部

  * POST 和 GET 的区别
    GET 在浏览器回退时是无害的，而 POST 会再次提交请求
    GET 产生的 URL 地址可以被收藏，而 POST 不可以
    GET 请求会被浏览器主动缓存，而 POST 不会，除非手动设置
    GET 请求只能进行 URL 编码，而 POST 支持多种编码方式
    GET 请求参数会被完整保留在浏览器历史记录里，而 POST 中的参数不会被保留
    GET 请求在 URL 中传送的参数是有长度限制的，而 POST 没有限制
    对参数的数据类型，GET 只接受ASCII 字符，而 POST 没有限制
    GET 比 POST 更不安全，因为参数直接暴露在 URL 上，所以不能用来传递敏感信息
    GET 参数通过 URL 传递，POST 放在 Request body
    中

  * HTTP 状态码
    1xx：指示信息-表示请求已接收，继续处理
    2xx：成功-表示请求已被成功接收
    3xx：重定向-要完成请求必须进行更进一步的操作
    4xx：客户端错误-请求有语法错误或请求无法实现
    5xx：服务器错误-服务器未能实现合法的请求

    状态码例子
    200 OK：客户端请求成功
    206 Partial Content：客户发送一个带有 Range 头的 GET 请求，服务器完成了它
    301 Moved Permanently：所请求的页面已经转移至新的 URL
    302 Found：所请求的页面已经临时转移至新的 URL
    304 Not Modified：客户端有缓冲的文档并发出了一个条件性的请求，服务器告诉客户，原来缓冲的文档还可以继续使用
    400 Bad Request：客户端请求有语法错误，不能被服务器所理解
    401 Unauthorized：请求未经授权，这个状态代码必须和 WWW-Authenticate 报头域一起使用
    403 Forbidden：对被请求页面的访问被禁止
    404 Not Found：请求资源不存在
    500 Internal Server Error：服务器发生不可预期的错误原来缓冲的文档还可以继续使用
    503 Server Unavailable：请求未完成，服务器临时过载或当机，一段时间后可能恢复正常


  * 什么是持久连接
    HTTP 协议采用"请求-应答"模式，当使用普通模式，即非 Keep-Alive 模式时，每个请求/应答客户和服务器都要新建一个连接，完成之后立即断开连接(HTTP 协议为无连接的协议)

    当使用 Keep-Alive 模式(又称为持久连接、连接重用)时，Keep-Alive 功能使客户端到服务器端的连接持续有效，当出现对服务器的后续请求时，Keep-Alive 功能避免了建立或者重新建立连接 (HTTP 1.1 支持)
  * 什么是管线化
    在使用持久连接的情况下，某个连接上消息的传递类似于
    请求1 -> 响应1 -> 请求2 -> 相应2 -> 请求3 -> 响应3

    某个连接上的消息变成了类似这样
    请求1 -> 请求2 -> 请求3 -> 响应1 -> 响应2 -> 响应3

    特点：
    * 管线化机制通过持久连接完成，仅 HTTP/1.1 支持此技术
    * 只有 GET 和 HEAD 请求可以进行管线化，而 POST 则有所限制
    * 初次创建连接时不应启动管线机制，因为对方(服务器)不一定支持 HTTP/1.1 版本的协议
    * 管线化不会影响响应到来的顺序，如上面的例子所示，响应返回的顺序并未改变
    * HTTP/1.1 要求服务器端支持管线化，但并不要求服务器端也对响应进行管线化处理，只是要求对于管线化的请求不失败即可
    * 由于上面提到的服务器端问题，开启管线化很可能并不会带来大幅度的性能提升，而且很多服务器端和代理程序对管线化的支持并不好，因此现代浏览器如 Chrome 和 Firefox 默认并未开启管线化支持

5.原型链类
  * 创建对象有几种方法
    ```
      var o1 = {name: 'o1'}
      var o11 = new Object({name: '011'})

      var M = function() {
        this.name = '02'
      }
      var o2 = new M()

      var P = {name: 'o3'}
      var o3 = Object.create(P)
      
    ```
  * 原型、构造函数、实例、原型链
  * instanceof 的原理
  * new 运算符
    一个新对象被创建。它继承自 foo.prototype ->构造函数 foo 被执行。执行的时候，相应的传参会被传入，同时上下文(this)会被指定为这个新实例。new foo 等同于 new foo()，只能用在不传递任何参数的情况 -> 如果构造函数返回一个"对象"，那么这个对象会取代整个 new 出来的结果。如果构造函数没有返回对象，那么 new 出来的结果为步骤 1 创建的对象

6. 面向对象类
  * 类与实例
    * 类的声明
    * 生成实例
  
  * 类与继承
    * 如何实现继承
    * 继承的几种方式

  oop.html

7.通信类

  * 什么是同源策略及限制
    同源指的是：同协议，同域名和同端口
    同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。
    这是一个用于隔离潜在恶意文件的关键的安全机制。
    非同源会有以下行为受到限制：
      * Cookie、LocalStorage和IndexDB 无法读取。
      * DOM 无法获得
      * AJAX 请求不能发送
      * 浏览器中不同域的框架之间不能进行 JS 的交互操作。
      * 不同的框架之间是可以获取 window 对象的，但却无法获取相应的属性和方法。
  * 前后端如何通信
    * Ajax 
    * WebSocket
    * CORS
  * 如何创建 AJAX  
    * XMLHttpRequest 对象的工作流程
    * 兼容性处理
    * 事件的触发条件
    * 事件的触发顺序

    ```
      function(options) {
        var xhr = XMLHttpRequest ? new XMLHttpRequest() : new window.ActiveXObject('Microsoft.XMLHTTP')
        
        xhr.open(type, url, true)
        xhr.send()
        xhr.onload = function() {
          if (xhr.status === 200 || xhr.status === 304) {
            var res;
            res = xhr.responseText
            // 成功
          } else {
            // 失败
          }
        }
      }
    ```

  * 跨域通信的几种方式
    * JSONP 运行一个 script 标签 window 挂载一个 callback 函数，script 加载完成以后会执行 callback 函数
    * Hash
    利用 hash，场景是当前页面 A 通过 iframe 或 frame 嵌入了跨域的页面 B
    ```
      // 在 A 中伪代码如下：
      var B = document.getElementsByTagName('iframe')
      B.src = B.src + '#' + 'data';
      // 在 B 中的伪代码如下
      window.onhashchange = function() {
        var data = window.location.hash
      }
    ```
    * postMessage
    ```
      // 窗口A(http://A.com) 向跨域的窗口 B (http://B.com)发送信息
      Bwindow.postMessage('data', 'http://B.com')
      // 在窗口 B 中监听
      window.addEventListener('message', function(event) {
        console.log(event.origin); // http://A.com
        console.log(event.source); // Awindow
        console.log(event.data); // data!
      }, false)
    ```

    * WebSocket
    ```
      var ws = new WebSocket('wss://echo.websocket.org');

      ws.onopen = function(evt) {
        console.log('connection open ...');
        ws.send('hello websockets');
      }

      ws.onmessage = function(evt) {
        console.log('Received Message:' + evt.data);
        ws.close();
      }

      ws.onclose = function(evt) {
        console.log('connection closed');
      }
    ```
    * CORS

    ```
      // url (必须)，options (可选)
      fetch('/some/url', {
        method: 'get',
      }).then(function (res) {

      }).catch(function (err) {

      })
    ```
8.安全类
  * CSRF
    * 基本概念和缩写 称为跨站请求伪造，英文名 Cross-site request forgery 缩写 CSRF
    * 攻击原理 用户登陆网站 A，网站 A 下发 cookie，用户访问网站 B 的时候，网站 B 引诱点击去请求网站 A 的同时带着网站 A 的 cookie，做一些攻击操作
    能够造成攻击的原理：1.网站中某一个接口存在漏洞 2.这个用户在那个网站确实登陆过
    * 防御措施
      1. token 验证
      2. Referer 验证 (页面来源)
      3. 隐藏令牌 (隐藏到 HTTP header 头中)
  * XSS
    * 基本概念和缩写 XSS (cross-site scripting 跨域脚本攻击)
    * 攻击原理 向页面注入 js 脚本
    * 防御措施 宗旨就是让注入的 js 脚本不能执行

9. 算法类
  * 排序 快速排序、选择排序、希尔排序、冒泡排序
  * 堆栈、队列、链表
  * 递归
    https://segmentfault.com/a/1190000009857470
  * 波兰式和逆波兰式
    理论 http://www.cnblogs.com/chenying99/p/3675876.html
    源码 https://github.com/Tairraos/rpn.js/blob/master/rpn.js

10. 渲染机制
   * 什么是 DOCTYPE 及作用
    DTD(document type difinition，文档类型定义)是一系列的语法规则，用来定义 XML 或 (X)HTML 的文件类型。浏览器会使用它来判断文档类型，决定使用何种协议来解析，以及切换浏览器模式。

    DOCTYPE 是用来声明文档类型和 DTD 规范的，一个主要的用途便是文件的合法性验证。如果文件代码不合法，那么浏览器解析时便会出一些差错。
    * HTML 5
      <!DOCTYPE html>
    * HTML 4.01 Strict
    * HTML 4.01 Transitional


   * 浏览器渲染过程
    ![浏览器渲染过程](https://github.com/95erlong/JavaScript-basic-knowledge/blob/master/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%B8%B2%E6%9F%93%E8%BF%87%E7%A8%8B.png)
    
   * 重排 Reflow
   * 重绘 Repaint
   * 布局 Layout

