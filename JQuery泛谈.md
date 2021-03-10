# Jquery泛谈

2021.3.10

自2006年发布以来，JQuery至今仍然是许多网站会采用的开发工具库，这主要是由于JQuery提供了强大的DOM操作功能和简单的API，相比使用原生API要更容易上手。本篇博客主要是记录一些JQuery常用功能。

## 目录

+ 选择网页元素
+ 链式操作
+ 元素的操作：读值和赋值
+ 元素的操作：复制、删除和创建
+ 元素的操作：移动
+ JQuery事件
+ JQuery特效

## 选择网页元素

jQuery的基本设计思想和主要用法，就是"选择某个网页元素，然后对其进行某种操作"。

jQuery的构造函数`jQuery()`接收选择表达式，选择表达式可以是css选择器、字符串、存储值的变量，这样可以得到jQuery构造的对象，可以调用该对象上的方法去操作对应的DOM元素。

``` javascript
    $('div')  // 选择所有div元素
    $('.className') // 选择class为className的元素
    $('div:visible') //选择可见的div元素
```

## 链式操作

JQuery最重要的功能，即可以将一系列操作连接在一起，像链条一样的形式：

` $('div').find('h3').fadeIn(500).html('Hello') `

分解以上代码：

``` javascript
    $('div')   // 找到div元素
    .find('h3') // 找到其中的h3元素
    .fadeIn(500) // 元素在500ms内淡入显示
    .html('Hello') // 修改内容为Hello
```

因为JQuery每个API都会返回JQuery对象，就可以连续操作。

## 元素的操作：读值和赋值

JQuery的设计思想之一，通过同一个函数实现“读值”和“赋值”,具体是哪一种，由函数的参数决定。

``` javascript
    $('p').html(); //html()没有参数，表示取出p的值

　　$('p').html('Hello'); //html()有参数Hello，表示对p进行赋值
```

常用的操作函数：

``` javascript
    .html() // 取出或设置html内容

　　.text() // 取出或设置text内容

　　.attr() // 取出或设置某个属性的值

　　.width() // 取出或设置某个元素的宽度

　　.height() // 取出或设置某个元素的高度

　　.val() // 取出某个表单元素的值
```

需要注意的是，如果结果集包含多个元素，那么赋值的时候，将对其中所有的元素赋值；取值的时候，则是只取出第一个元素的值（.text()例外，它取出所有元素的text内容）。

## 元素的操作：复制、删除和创建

复制元素：`.clone()`得到元素深拷贝副本，包括元素内所有后代元素和节点。

删除元素:`.remove()`将匹配元素集合从DOM中删除,包括元素事件和JQuery数据。

清空元素：`.empty()`清空元素的内容，但是元素本身不会删除。

创建元素：使用构造函数`$('<p>hi</p>')`即可。

## 元素的操作：移动

JQuery提供移动元素位置的方法，可以移动元素在网页中的位置，通过这些方法的组合，能最终使目标元素移动到指定位置。

注意操作的元素不同。

``` javascript
   .after() // 在每个匹配元素的后面插入元素

　　.before() // 在每个匹配元素的前面插入内容

    // $('.inner').after('<p>Test</p>') 在.inner元素外部，跟着创建一个元素
```

``` javascript
    .append() // 在每个匹配元素里面的末尾处插入参数内容
    $('.inner').append('<p>Test</p>')

    .appendTo() // 将匹配的元素插入到目标元素的最后面
    $('<p>Test</p>').appendTo('.inner')
```

使用以上方法时要小心分辨是对哪个元素进行操作。

## JQuery事件

JQuery直接将事件绑定在网页元素上。

``` javascript
    $('p').click(function(){

　　　　alert('Hello');

　　});
```

JQuery支持的事件参照手册，根据需求使用。在开发中通常使用`on()`方法以更灵活地控制事件:

``` javascript
    $('选择器表达式').on('事件名',function(){
        // 事件处理代码
    })
```

使用`off()`解除事件绑定：

``` javascript
    $('选择器表达式').off('事件名','被解除的函数')  
```

## JQuery特效

JQuery允许给要操作的元素添加动画效果：

``` javascript
    .fadeIn() // 淡入

　　.fadeOut() // 淡出

　　.fadeTo() // 调整透明度

　　.hide() // 隐藏元素

　　.show() // 显示元素

　　.slideDown() // 向下展开

　　.slideUp() // 向上卷起

　　.slideToggle() // 依次展开或卷起某个元素

　　.toggle() // 依次展示或隐藏某个元素
```

使用时可以通过`$('div').fadeIn(400)`的方式指定动画时间。
