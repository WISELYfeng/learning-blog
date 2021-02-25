# JS对象基本用法

2021.2.25

## 声明对象的两种语法

+ 方式一：
  
    ``` javascript
        let obj = {
            key1 : value1,
            key2 : value2,
            ...
        }
    ```

+ 方式二：
  
    ``` javascript
        let obj = new Object({key:value,...})
    ```

> 使用 '' 包住 `key`，因为对象的键名都是字符串。如果要用变量表示键名，则需要用[]包住变量。

## 删除对象的属性

+ 方式一：
  
``` javascript
    obj.key = undefined
```

这种方式会删掉key对应的值，但key还存在，也就是留下了一个“空槽”。

+ 方式二：

``` javascript
    delete obj.key
```

使用`delete`关键字会删掉整个键值对，即键名和值都将不存在。

> 使用`key in obj`检查对象是否含有键名，使用`key in obj && obj.key`检查键名和value是否都存在。

## 查看对象属性

+ 查看对象单个属性：
  
``` javascript
    obj.key
    obj['key']
    obj[key]
```

注意`obj[key]`是用变量作为键名。

+ 查看对象所有属性：
  
``` javascript
    Object.keys(obj) // 返回对象的所有自身属性
    console.dir(obj) // 打印对象的所有属性以及共有属性
    obj.__proto__ // 返回对象的共有属性
```

## 修改或增加对象属性

+ 直接赋值： `let obj.name = xxx`。
+ 批量赋值： `Object.assign(obj,{key:value,...})`。
+ 直接修改属性： `obj.name = xxx`

## 修改对象的共有属性

不是常用功能，只作为备忘记录。

+ `obj.__proto__.fn = xxx`。修改原型的方法
+ `Object.prototype.fn = xxx`。修改原型的方法，推荐使用。
+ `let obj2 = Object.create(obj)`。以obj为原型生成obj2，则obj2的__proto__指向obj，obj的__proto__再指向对象原型。

## 'name' in obj和obj.hasOwnProperty('name') 的区别

`in`可以检查某个属性是否存在于对象中，但不能看出该属性是对象实例的自有属性还是原型上的共有属性。`hasOwnProperty()`只看对象的自有属性中是否有该属性。
