## JavaScript 基础知识学习及整理

1.  JS 中使用 typeof 能得到的哪些类型
    · typeof undefined // undefined
    · typeof 'abc' // string
    · typeof 123 // number
    · typeof true // boolean
    · typeof {} // object
    · typeof [] // object
    · typeof null // object
    · typeof console.log // function
    typeof 只能区分值类型的详细类型；对于引用类型只能区分是否是 function 或者 object；

2.  何时使用 === 何时使用 ==

```
 if (obj.a == null) {
   // 这里相当于 obj.a === null || obj.a === undefined ，见血形式
   // 这是 jquery 源码中推荐的写法
 }
```

  >除以上类似情况，其他全部使用 ===

3.  JS 中有哪些内置函数
    `Object Array Boolean Number String Function Date RegExp Error`

4.  JS 变量按照存储方式区分为哪些类型，并描述其特点
    · 值类型

      ```
      var a = 100
      var b = a
      a = 200
      console.log(b) // 100
      ```

        a 和 b 赋值操作，

      · 引用类型: 对象、数组、函数

      ```
        var a = {age: 20}
        var b = a
        b.age = 21
        console.log(a.age) // 21
      ```
      a 和 b 是一个对象指针，指向同一个对象 {age: 20}，更改是对象的 age 属性，

5.  如何理解 JSON
  · JSON 是一个 JS 对象，常用 api ：
    1. stringify
    2. parse
  · JSON 也是一个数据格式

6.  强制类型转换
    · 字符串拼接

    ```
      var a = 100 + 10 // 110
      var a = 100 + '10' // '10010'
    ```

    · == 运算符

    ```
      100 == '100' // true
      0 == '' // true
      null == undefined // true
    ```

    · if 语句

    ```
      var a = true
      if (a) {
        // ...
      }
      var b = 100
      if (b) {
        // ...
      }
      var c = ''
      if (c) {
        // ...
      }
    ```

    · 逻辑运输

    ```
      10 && 0 // 0
      '' || 'abc' // 'abc'
      !window.abc // true

      var a = 100
      console.log(!!a) // !! 用于变量判断
    ```
7. 如何准确判断一个变量是数组类型
  ```
    var arr = []
    arr instanceof Array // true
    typeof arr // object, typeof 是无法判断是否时数组的
  ```

8. 写一个原型链继承的例子
  ```
    // 简单例子
    function Animal() {
      this.eat = function() {
        console.log('animal eat')
      }
    }

    function Dog() {
      this.bark = function() {
        console.log('dog bark')
      }
    }

    Dog.prototype = new Animal()

    var hashiqi = new Dog()

    // 贴近实际开发原型链继承的例子
    function Elem(id) {
      this.elem = document.getElementById(id)
    }

    Elem.prototype.html = function(val) {
      var elem = this.elem
      if (val) {
        elem.innerHTML = val
        return this // 链式操作
      } else {
        return elem.innerHTML
      }
    }
    
    Elem.prototype.on = function(type, fn) {
      var elem = this.elem
      elem.addEventListener(type, fn)
    }

    var div1 = new Elem('div1')
    div1.html('<p>hello</p>')
    div1.on('click', function () {
      alert('click')
    })

  ```


9. 描述 new 一个对象的过程
  ```
    // this 指向这个新对象
    function Foo(name, age) {
      // 执行代码，对 this 赋值
      this.name = name
      this.age = age
      // 返回 this
    }
    // 1. 创建一个新对象
    var f = new Foo('Leo', 29)
  ```

10. 构造函数
  ```
    function Foo(name, age) {
      this.name = name
      this.age = age
      this.class = 'class-1'
      // return this // 默认有这一行
    }

    var f = new Foo('Leo', 29)
    // var f1 = new Foo('Zhang', 20) // 创建多个对象
  ```
  · 扩展
  ```
    var a = {} // 是 var a = new Object() 的语法糖
    var a = [] // 是 var a = new Array() 的语法糖
    function Foo() {...} // 是 var Foo = new Function(...)
    // 使用 instanceof 判断一个函数是否是一个变量的构造函数
  ```
11. 原型规则和示例
  · 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（除了 'null' 以外）
  · 所有的引用类型（数组、对象、函数），都有一个 \_\_proto\_\_ (隐式原型) 属性。属性值是一个普通的对象
  · 所有的函数，都有一个 prototype （显示原型） 属性，属性值也是一个普通的对象
  · 所有的引用类型（数组、对象、函数），\_\_proto\_\_ 属性值指向它的构造函数的 'prototype' 属性值
  · 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的\_\_proto\_\_（即它的构造函数的 prototype）中寻找。

**示例**
```
  function Foo(name, age) {
    this.name = name
  }

  Foo.prototype.alertName = function() {
    alert(this.name)
  }

  var f = new Foo('Leo')
  f.printName = function() {
    console.log(this.name)
  }

  f.printName()
  f.alertName()
  f.toString() // 要去 f.__proto__.__proto__ 中查找

  var item
  for (item in f) {
    // 高级浏览器已经在 for in 中屏蔽了来自原型的属性
    // 但是这里建议加上这个判断，保证程序的健壮性
    if (f.hasOwnProperty(item)) {
      console.log(item)
    }
  }

```

![JavaScript原型链示意图 ](https://github.com/95erlong/JavaScript-basic-knowledge/blob/master/JavaScript%E5%8E%9F%E5%9E%8B%E9%93%BE.png)


12. 说一下对变量提升的理解
  · 变量定义
  · 函数声明（注意和函数表达式的区别）
  
13. 说明 this 几种不同的使用场景
  · 作为构造函数执行
  · 作为对象属性执行
  · 作为普通函数执行
  · call apply bind

14. 创建 10 个 \<a\> 标签，点击的时候弹出来对应的序号
  ```
    // 错误写法
    var i, a
    for (i = 0; i < 10; i++) {
      a = document.createElement('a')
      a.innerHTML = i + '<br>'
      a.addEventListener('click', function (e) {
        e.preventDefault()
        alert(i)
      })
      document.body.appendChild(a)
    }

    // 正确
    var i
    for (i = 0; i < 10; i++) {
      (function (i) {
        var a = document.createElement('a')
        a.innerHTML = i + '<br>'
        a.addEventListener('click', function (e) {
          e.preventDefault()
          alert(i)
        })
        document.body.appendChild(a)
      })(i)
    }
  ```
15. 如何理解作用域
  · 自由变量
  · 作用域链，即自由变量的查找
  · 闭包的两个场景 // 函数作为返回值，函数做为参数

16. 实际开发中闭包的应用
  ```
    function isFirstLoad() {
      var _list = []
      return function (id) {
        if (_list.indexOf(id) >= 0) {
          return false
        } else {
          _list.push(id)
          return true
        }
      }
    }
    var firstLoad = isFirstLoad()
    firstLoad(10)
  ```

17. 同步和异步的区别是什么？分别举一个同步和异步的例子
  · 同步会阻塞代码执行，而异步不会
  · alert 是同步，setTimeout 是异步

18. 前端使用异步的场景
  · 定时任务：setTimeout，setInterval
  · 网络请求：ajax 请求，动态 <img> 加载
  · 事件绑定

19. 获取 2019-02-27 格式的日前
  ```
    function formatDate(dt) {
      if (!dt) {
        dt = new Date()
      }
      var year = dt.getFullYear()
      var month = dt.getMonth() + 1
      var date = dt.getDate()
      if (month < 10) {
        month = '0' + month
      }
      if (date < 10) {
        date = '0' + date
      }

      return year + '-' + month + '-' + date
    }

    var dt = new Date()
    var formatDate = formatDate(dt)
    console.log(formatDate)
  ```

20. 获取随机数。要求是长度一致的字符串格式

  ```
    var random = Math.random()
    random = random + '0000000000' 
    random = random.slice(0, 10)
    console.log(random)
  ```
21. 写一个能遍历对象和数组的 forEach 函数
  ```
    function forEach(obj, fn) {
      var key
      if (obj instanceof Array) {
        obj.forEach(function (item, index) {
          fn(index, item)
        })
      } else {
        for (key in obj) {
          fn(key, obj[key])
        }
      }
    }

    var arr = [1, 2, 3]

    forEach(arr, function (index, item) {
      console.log(index, item)
    })

    var obj = {x: 100, y: 200}
    forEach(obj, function (key, value) {
      console.log(key, value)
    })
  ```

22. DOM 结构操作

  ```
    var div1 = document.getElementById('div1')
    // 添加新节点
    var p1 = document.createElement('p')
    p1.innerHTML = 'this is p1'
    div1.appendChild(p1)
    // 移除已有节点
    var p2 = document.getElementById('p2')
    div1.appendChild(p2)

    // 获取父子元素
    var div = document.getElementById('div1')
    var parent = div.parentElement

    var child = div.childNodes

  ```
23. DOM 节点的 Attribute 和 property 有何区别
  · property 只是一个 JS 对象的属性的修改
  · Attribute 是对 html 标签属性的修改

24. BOM (Browser Object Model) 操作

25. 如何检测浏览器的类型
  ```
      var ua = navigator.userAgent
      var isChrome = ua.indexOf('Chrome')
      console.log(isChrome)
  ```

26. 拆解 URL 的个部分
  · navigator // 浏览器
    ```
      var ua = navigator.userAgent
      var isChrome = ua.indexOf('Chrome')
      console.log(isChrome)
    ```

    · screen  // 屏幕
    ```
      console.log(screen.width)
      console.log(screen.height)
    ```
    
    · location // 地址
    ```
      console.log(location.href) // 整个 URL
      console.log(location.host) // 域名
      console.log(location.protocol) // 'http:' 'https:'
      console.log(location.pathname) // '/learn/123'
      console.log(location.search) 
      console.log(location.hash)
    ```
    
    · history // 历史
      ```
        history.back()
        history.forward()
      ```

27. 编写一个通用的事件监听函数
  ```
    // 普通写法
    var btn = document.getElementById('btn1')
    btn.addEventListener('click', function (event) {
      console.log('clicked')
    })
  ```

28. 描述事件冒泡流程

29. 对于一个无限下拉加载图片的页面，如何给每个图片绑定事件