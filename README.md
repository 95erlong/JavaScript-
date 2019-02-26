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

// 除以上类似情况，其他全部使用 ===

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
  变量 instanceof Array 

8. 写一个原型链继承的例子
9. 描述 new 一个对象的过程
10. zepto (或其他框架) 源码中如何使用原型链 
11. 构造函数
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
12. 原型规则和示例
  · 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（除了 'null' 以外）
  · 所有的引用类型（数组、对象、函数），都有一个 \_\_proto\_\_ (隐式原型) 属性。属性值是一个普通的对象
  · 所有的函数，都有一个 prototype （显示原型） 属性，属性值也是一个普通的对象
  · 所有的引用类型（数组、对象、函数），\_\_proto\_\_ 属性值指向它的构造函数的 'prototype' 属性值
  · 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的\_\_proto\_\_（即它的构造函数的 prototype）中寻找。
示例
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

```