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