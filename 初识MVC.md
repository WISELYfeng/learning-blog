# 初识MVC

2021.3.31

1. MVC是开发程序中一种程序组织结构，M是Model(模型)、V是View(视图)、C是Controller(控制器)，即每个程序可分为这三层结构。

2. Model层负责数据的所有操作，包括数据的增删改查。

    ``` vim
        // 伪代码
        model = {
            1.声明所有数据属性
            2.声明增删改查方法
        }
    ```

3. View层负责更新界面

   ``` vim
        // 伪代码
        view = {
            1.声明一个视图容器属性
            2.声明容器初始化方法
            3.声明容器的渲染方法，当model中的数据改变时调用该渲染方法
        }
   ```

4. Controller层负责剩下的其他事情，需要根据另外两层的变化作整体控制。

    ``` vim
        Controller = {
          1.声明程序各功能方法，调用model更新数据
          2.声明通知view更新视图的方法
          3.声明事件的处理方法  
        }
    ```

5. 不同对象之间可以使用`EventBus`进行通信，`EventBus`实际上是利用原型链上的`EventTarget`。EventTarget的API：

   ``` vim
        addEventListener：
        元素使用addEventListener监听事件，需要传入监听的事件类型和事件发生时使用的回调函数
   ```

   ``` vim
        removeEventListener：
        元素使用removeEventListener移除监听事件，需要传入被移除的监听事件类型和移除的回调函数
   ```

6. 表驱动编程：表驱动法就是一种编程模式——从表里面查找信息而不使用逻辑语句(if 和switch)。凡是能通过逻辑语句来选择的事物，都可以通过查表来选择。对简单的情况而言，使用逻辑语句更为容易和直白。但随着逻辑链复杂度增加，使用表驱动编程能将复杂度控制在可控范围内。

    ``` javascript
        // 使用表驱动编程完成事件绑定
        const elements = document.querySelector('div')
        const events= {
            'click #add1': 'add',
            'click #minus1': 'minus',
            'click #mul2': 'mul',
            'click #divide2': 'div',
        }
        const autoBindEvents= ()=> {
            for (let key in events) {
                const value = events[key]
                const spaceIndex = key.indexOf(' ')
                const part1 = key.slice(0, spaceIndex)
                const part2 = key.slice(spaceIndex + 1)
                elements.on(part1, part2, value)
            }
        }
    ```

7. 关于模块化，我个人认为模块化是一种组织程序功能的方式，通过抽象使用场景简化方法，让方法能够以最简方式被调用。模块化的程序就像一块砖，可以被放到任何需要的地方使用。模块化也意味着解耦，只有尽量减少耦合，才能最大程度避免程序重构的风险。
