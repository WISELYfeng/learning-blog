# 《你不知道的JavaScript》 学习笔记

2022.8.23

## 第一章 作用域

### 1.1LHS/RHS

LHS和RHS分别表示赋值操作的左侧和右侧，赋值操作不仅是`=`，比如函数传参也是赋值操作，赋值操作可以理解
为"赋值操作的目标是谁（LHS）"以及"谁是赋值操作的源头（RHS）"。

`console.log(a)`, `a`的引用是RHS，因为没有对`a`的赋值操作，而是需要查找`a`的值传递给`console.log()`。

`a = 2`则是LHS引用，因为我们并不关心当前的值，而是需要为 =2 这个操作找到目标。

### 1.2作用域嵌套

不论是哪种查询，引擎都会从当前执行的作用域开始查找，若没找到则向外遍历直到全局作用域，无论是否找到，
查找过程都会停止。

### 1.3异常

执行LHS查询时无法查询到变量时，非严格模式下会在全局作用域下创建变量，执行RHS查询失败时，会抛出ReferenceError,
对变量对值进行不合理操作，比如对非函数类型的值进行函数调用，会抛出TypeError。

## 第二章 词法作用域

### 2.1词法阶段

定义在词法阶段的作用域是词法作用域，是由书写代码时变量和块作用域在哪决定的，大部分情况下词法分析器处理
代码时会保持作用域不变（大部分情况，严格模式另说）

### 2.2欺骗词法

`eval()`能够接收字符串并生成对应代码。
`with`根据传递的对象凭空创建一个全新的词法作用域。

### 2.3小结

两种修改词法作用域的方式都不推荐使用，一是容易降低代码的可读性，二是影响js引擎性能，因为引擎不能
在编译时对作用域查找进行优化。

## 第三章 函数作用域和块作用域

### 3.1隐藏内部实现

出于软件设计中的最小授权或最小暴露原则，不需要在全局作用域中能访问到所有变量和函数，正确的代码需要隐藏内部细节，只对外暴露必需的部分。另外，避免把
变量都放到全局作用域中，放到函数内部可以避免变量值被覆盖。

### 3.2隐藏的方式
+ 全局命名空间：比如第三方库会在全局作用域中声明一个名字独特的变量，该变量通常是一个对象，即第三方库的命名空间，所有对外暴露的功能都是该对象的属性。
+ 模块管理： 通过依赖管理器的机制将库的标识符显式地导入到特定的作用域中。在第五章会详细介绍。

### 3.3匿名函数和具名函数

匿名例子：
```javascript
setTimeout(function(){
    console.log("I waited 1 second")
}, 1000);
```

具名函数：   
```javascript
setTimeout(function timeoutHandler(){
    console.log("I waited 1 second")
}, 1000)
```

这两种方式的功能是一样的，不过出于调试和便于理解的角度，推荐具名函数的形式。

### 3.4立即执行函数

```javascript
// 这两种书写方式效果是相同的
(function func(global){
    // do something
})(window)

(function(window){
    // do something
}(window))
```

立即执行函数是一种保护私有变量的方式。

### 3.5块作用域

由于`var`定义的变量会被提升到作用域顶部，因此有必要限制变量的作用范围。使用`let`和`const`隐式地将变量作用域转为块作用域，这样变量将不会影响到
块作用域外的代码。

处于块作用域中的代码会在被执行后销毁，有利于垃圾回收：
```javascript
function process(data){
    // do something
}
{  // 对引擎来说，能够很清楚地知道块作用域中的someData不需要保留，使用后即销毁
    let someData = {}
    process(someData)
}
```

可以解决`for`循环打印变量的问题：

```javascript
// let将i绑定到了for循环的块中，并重新绑定到每一个迭代中，确保使用上一个循环迭代结束时的值重新进行赋值
for(let i=0; i<10; i++){ 
    console.log(i)
}
```

本章还有一些有趣的技巧，详见书本内容。

## 第四章 提升

### 4.1 声明

声明操作会被提到最前，比如:

```javascript
a = 2
var a
console.log(a)  // 输出2
```

另外就是，函数也是，函数声明会提升，函数表达式的赋值操作不会，比如：
```javascript
foo()  // 输出 foo
function foo(){
    console.log('foo')
}
```

```javascript
foo()  // TypeError
var foo = function(){
    console.log('foo')
}
```

由于变量标识符被提升并分配给所在作用域，因此`foo()`不会导致ReferenceError，但是`foo`并没有赋值，相当于对undefined值进行调用，因此抛出TypeError异常。

这就是js引擎的编译逻辑，声明会提升，而赋值或其它操作的位置在原地，注意避免提升影响代码执行顺序。


### 变量与函数提升的顺序

函数会被首先提升，然后是变量。重复声明的函数会被覆盖。

## 第五章 作用域闭包

### 5.1闭包的形式

闭包是在定义时发生的。

函数内部定义的函数，持有所有作用域的值，并且该内部函数在定义时的环境不会被销毁，就称之为闭包。

使用回调函数，实际上就是在使用闭包。

当函数可以记住并访问所在的词法作用域，即使函数是在当前词法作用域之外执行，这时就产生了闭包。

### 5.2 模块

模块模式：
1.必须有外部的封闭函数，该函数至少被调用一次（每次调用都会创建一个新的模块实例）。
2.封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域中形成闭包，并且可以访问或者修改私有的状态。

### 5.2 现代的模块机制

一个封装的模块案例：
```javascript
var MyMouldes = (function Manager(){
    var modules = {}
    function define(name, deps, impl) {
        for(var i=0; i<deps.length; i++){
            deps[i] = modules[deps[i]]
        }
        modules[name] = impl.apply(impl, deps)
    }
    
    function get(name) {
        return modules[name]
    }
     return {
         define: define,
         get: get
     }
})()
```

这段代码的核心是`modules[name] = impl.apply(impl, deps)`，为模块的定义引入了包装函数，并且将模块的API存储在一个根据名字管理的模块列表中。
使用这段代码来定义模块：

```javascript
MyModules.define("bar", [], function(){
    function hello(who){
        return "Let me introduce:" + who 
    }
    return {
        hello: hello
    }
})

MyModules.define("foo", ["bar"], function(bar){
    var hungry = "hippo"
    function awesome(){
        console.log(bar.hello(hungry).toUpperCase())
    }
    return {
        awsome: awesome
    }
})

var bar = MyModules.get("bar")
var foo = MyModules.get("foo")

console.log( bar.hello("hippo")) // Let me introduce: hippo
foo.awsome() // LET ME INTRODUCE: HIPPO
```

foo和bar都是通过返回公共API定义的模块，并且foo可以接受bar的实例作为参数并使用。

模块模式的特点：（1）为创建内部作用域而调用了一个包装函数 （2）包装函数的返回值必须至少包括一个对内部函数的引用，这样就会创建涵盖整个包装
函数内部作用域的闭包。

### 5.3 模块的导出机制

+ import可以将一个模块中的一个或多个API导入到当前作用域中，并分别绑定在一个变量上。
+ module会将整个模块的API导入并绑定到一个变量上。
+ export会将当前模块的一个标识符（变量、函数）导出为公共API。

## 第一章 this

### 1.1 关于this的误解

+ this不指向函数自身。
+ this不指向函数的作用域。

### 1.2 this到底上什么

this是在运行时进行绑定的，当函数被调用时，会创建一个活动记录（或称为执行上下文），this是这个记录的一个属性。因此，this的指向取决于函数的调用
位置。

## 第二章 this全面解析                                                                                                                                                                                                                                         

### 1.1 this的绑定规则

this的指向取决于函数被调用的位置。

#### 1.1.1默认绑定

函数独立调用时，this指向全局对象，在严格模式下指向undefined。

#### 1.1.2隐式绑定

当函数有上下文对象时，this会指向上一层或者说最后一层，比如`obj.func()`的this指向obj。要注意，被隐式绑定的函数会丢失绑定对象，转为
默认绑定。见书上本章节例子，当接收一个函数作为参数并调用这个函数时，函数中的this指向属于默认绑定情况。

#### 1.1.3显示绑定

