# Vue响应式

2021.4.2
> Vue实现响应式的基本原理：当Vue实例接收一个data对象时，实例会对这个data作一个改造，使用`Object.defineProperty`将data的每个属性改造为计算属性，再通过实例代理data，于是对data的更改将被实例监听，数据产生变化即会通知视图更新。

## 计算属性

+ getter

    `get`语法将对象属性绑定到查询该属性时将被调用的函数。

+ setter

    当尝试设置属性时，`set`语法将对象属性绑定到要调用的函数。
    
``` javascript
    let obj1 = {
        name: "jellyfish",
        get myName() {
            return this.name;
        },
        set myName(xxx){
            this.name = xxx
            
        },
    };

    obj1.myName = '高媛媛'

    console.log(`名字： ${obj1.name}`) // 输出: "名字： 高媛媛"
```

使用`set`和`get`定义的属性实际不存在，但是可以通过属性名实现读取或修改实例的属性值。

## Object.defineProperty()

`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

结合计算属性，利用`Object.defineProperty()`实现监听。

``` javascript
let myData1 = {n:0}
let data1 = proxy2({ data:myData1 }) // 括号里是匿名对象，无法访问

function proxy2({data}){
  let value = data.n
  Object.defineProperty(data, 'n', {
    get(){
      return value
    },
    set(newValue){
      if(newValue<0)return
      value = newValue
    }
  })
  // 就加了上面几句，这几句话会监听 data

  const obj = {}
  Object.defineProperty(obj, 'n', {
    get(){
      return data.n
    },
    set(value){
      if(value<0)return// 这句话多余了
      data.n = value
    }
  })
  
  return obj // obj 就是代理
}

console.log(`需求：${data1.n}`)  // 输出：需求：0 
myData1.n = -1
console.log(`需求：${data1.n}，设置为 -1 失败了`) // 输出：需求：0，设置为 -1 失败了 失败了 
myData1.n = 1
console.log(`需求：${data1.n}，设置为 1 成功了`) // 输出：需求：1，设置为 1 成功了 
```

通过代理实现监听数据的修改，能够保证数据不会被随意篡改。