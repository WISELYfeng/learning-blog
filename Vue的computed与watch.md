# Vue的computed与watch

## computed

+ computed：计算属性，用来处理需要通过计算得到结果的数据，数据有缓存，不变化则不计算
  
computed 应用例子：

``` vue
<template>
  <div class="hello">
      {{fullName}}
  </div>
</template>

<script>
export default {
    data() {
        return {
            firstName: '阮',
            lastName: "一峰"
        }
    },
    props: {
      msg: String
    },
    computed: {
        fullName() {
            return this.firstName + ' ' + this.lastName
        }
    }
}
</script>
```

## watch

+ watch：监听，用于在数据发生变化时调用回调处理变化,接收`newVal`和`oldVal`。

watch实例：

``` vue
<template>
  <div class="hello">
      {{fullName}}
      <button @click="setName">click</button>
  </div>
</template>

<script>
export default {
    data() {
        return {
            firstName: '阮',
            lastName: "一峰"
        }
    },
    methods: {
        setName() {
            this.firstName = "张";
        }
    },
    watch: {
        firstName(newVal,oldVal) {
          console.log(newVal)
          console.log(oldVal)
        }
    }
}
</script>
```

watch可以配置options,有三种选项：

+ immediate:表示是否在第一次渲染的时候执行这个函数，为true时则立即触发回调函数；如果为false，不会立即执行回调。
+ deep:设置为true时，当监听的数据是引用类型，会比对内部数据变化，设置为false，只对比最外层变化。
+ handler：watch中需要具体执行的方法。
  