# this的指向

当函数执行时，`this`的指向由函数被调用的形式决定。

比如 ：

``` javascript
var obj = {
    func: function(){
        console.log(this)
    }
}

var test = obj.func
obj.func()  // 打印obj
test()  // 打印 window
```


为了避免混淆`this`的指向，使用`call()`显示地指定`this`的指向。

`call()`接受的是参数列表：
> function.call(thisArg, arg1, arg2, ...)

thisArg:function运行时使用的this值。

改写之前的代码：

```javascript
var obj = {
    func: function(){
        console.log(this)
    }
}

var test = obj.func
obj.func.call(obj)  // 打印 obj
test.call(test)  // 打印 obj里func的内容
```

通过使用`call()`的方式可以指定`this`,这也是一种良好的编程习惯
