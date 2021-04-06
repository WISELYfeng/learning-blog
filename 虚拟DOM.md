# 虚拟 DOM 和 DOM diff

## 一

## 虚拟DOM是什么

虚拟DOM是用JS对象模拟DOM结构的一种数据类型。

``` javascript
    const node = {
        key:null,
        props:{  // 属性
            children:[ // 子元素
                {type:'span',...}
            ]
        },
        ref:null,
        type:'div'
        ...
    }
```

使用模板语法创建虚拟DOM，以Vue为例：

``` vim
    <div class="red" @click="fn">
        <span>span1</span>
        <span>span2</span>
    </div>
```

## 虚拟DOM的优点

+ 减少DOM操作
  虚拟DOM可以将多次操作合并为一次操作。
  虚拟DOM可以使用DOM diff省去多余的操作。

+ 跨平台
  由于虚拟DOM本身是一个JS对象，因此可以在小程序、IOS应用、安卓应用上使用。

## 虚拟DOM的缺点

需要额外的创建函数，如 `createElement` 或 `h`，但可以通过 `JSX`来简化成 `XML` 写法。

## 二

## DOM diff是什么

DOM diff即虚拟DOM的对比算法。

``` javascript
    // 就是一个函数，我们称之为 patch
    patches = patch(oldVNode, newVNode)
    // patches 就是要运行的 DOM 操作
```

+ Tree diff
    将新旧两棵树逐层对比，找出哪些节点需要更新。
    如果节点是组件就看 Component diff。
    如果节点是标签就看 Element diff。

+ Component diff
    如果节点是组件，就先看组件类型。
    类型不同直接替换（删除旧的）。
    类型相同则只更新属性。
    然后深入组件做 Tree diff（递归）。

+ Element diff
    如果节点是原生标签，则看标签名
    标签名不同直接替换，相同则只更新属性
    然后进入标签后代做 Tree diff（递归）

## DOM diff 的优点

当虚拟DOM里的数据发生变化，diff算法会比较新旧对象的变化，只更新有变化的部分，可以节省浏览器渲染的时间。

## DOM diff 的问题

同级节点比较时，会出现识别错误的问题。要避免识别错误，每个节点需要绑定一个唯一标识`key`。

见此文章:[key的作用](https://www.zhihu.com/question/61064119/answer/766607894)。
