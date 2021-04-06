# 初识VUE

2021.4.6

## 安装VUE

vue有两个版本：一种是完整版，另一种是非完整版,两者有40%左右的代码体积差别。

**区别：**

+ 文件名：

  完整版：vue.js。

  非完整版：vue.runtime.js。
  >投入生产时使用*.min.js后缀的文件。

+ 编译：
  
  完整版：有complier,可以将html或template直接转为视图。

  非完整版：没有complier,需要使用vue-loader配合vue文件转为视图。

  ``` javascript
    // 需要编译器
    new Vue({
        template: '<div>{{ hi }}</div>'
    })

    // 不需要编译器
    new Vue({
        render (h) {
            return h('div', this.hi)
        }
    })
  ```

## 使用codesandbox创建项目

1. 进入[codesandbox首页](https://codesandbox.io/)。
2. 点击页面中间的`Create Sandbox`按钮。
3. 在创建列表中选择VUE的图标，点击该图标，就会自动创建项目
4. 编写代码
