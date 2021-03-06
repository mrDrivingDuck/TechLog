# Vue.js - Component

Created by : Mr Dk.

2019 / 03 / 16 0:15

Nanjing, Jiangsu, China

---

## Compenent Registration

关于组件的注册主要有两种：

* 全局注册
* 局部注册

全局注册的组件可以用在任何新创建的 Vue 根实例模板中（`new Vue`）：

```javascript
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```

局部注册的组件在当前组件中使用：

```javascript
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```

---

## Communication between Components

### 从父组件到子组件的通信

由 prop 实现，具体使用过程：

* 在父组件中，使用 `v-bind` 将数据属性绑定到子组件上
  * 注意，由于 HTML 大小写不敏感，需要将驼峰命名转换为对应的 kebab-case 命名
* 在子组件的 `props: []` 中声明绑定与父组件绑定的数据
* 在子组件中可以直接使用声明过的数据

prop 使得父子组件之间形成 **单向下行绑定** 的关系。

* 父级 prop 对应的数据更新将向下流动到子组件中
* 由于 Vue.js 的响应式特性，子组件的对应值将会随着父组件改变
* 反过来不行，即 **不应该在一个子组件内部改变 prop**

两种可能的需要改变 prop 的场景：

1. prop 用于传递初始值，子组件接下来将其作为一个本地的数据使用

   ```javascript
   props: ['initialCounter'],
   data: function () {
     return {
       counter: this.initialCounter
     }
   }
   ```

2. prop 以原始值传入，子组件需要进行运算转换

   ```javascript
   props: ['size'],
   computed: {
     normalizedSize: function () {
       return this.size.trim().toLowerCase()
     }
   }
   ```

### 从子组件到父组件的通信

实现过程：

* 在父组件中自定义一个事件，并通过 `v-on` 绑定到子组件
* 在子组件中调用 `this.$emit('event-name', params)` 触发父组件绑定的事件，并传递参数
* 父组件执行触发函数

---

## Watch

用于观察和响应 Vue 实例上的数据变动。当然，也可以使用 `computed` 属性。

```javascript
data: {
  firstName: 'Foo',
  lastName: 'Bar',
  fullName: 'Foo Bar'
},
watch: {
  firstName: function (val) {
    this.fullName = val + ' ' + this.lastName
  },
  lastName: function (val) {
    this.fullName = this.firstName + ' ' + val
  }
}
```

---

