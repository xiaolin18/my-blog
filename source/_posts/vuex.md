title: 我眼中的Vuex
---

## vuex是做什么的？
Official：一个专为Vue.js程序开发的状态管理模式
Myself：Vue的状态树

## 有哪些优势？
- 方便的数据管理
  1. vue的数据流：单向数据流
    ```JavaScript
      new Vue({
        // state
        data () {
          return {
            count: 0
          }
        },
        // view
        template: `
          <div>{{ count }}</div>
        `,
        // actions
        methods: {
          increment () {
            this.count++
          }
        }
      })  

    ```
    <img src="../../../.././images/flow.png" alt="单向数据流" style="height: 400px;width: 700px;">
    2. 那么问题来了
      - 多个视图依赖于同一个状态：多层嵌套，对兄弟组件间状态的传递无能为力
      - 来自不同视图的行为需要变更同一个状态：父子组件直接引用，或者通过事件来变更和同步状态的多分拷贝
      一个状态被传来传去，破坏了Vue数据流的简洁

      结论：
          把组件的共享状态抽取出来，以一个全局单例模式管理，组件在任意时刻可以获取任意数据

- 配合devTools进行数据追踪
- 组件间的通信
  1. 父子组件通信
  2. 非父子组件间的通信

## 如何使用Vuex以及核心概念
- 官网图
  <img src="../../../../../images/vuex-offical.png">

- 自己总结的图
  <img src="../../../../../images/vuex.png">

  <img src="../../../../../images/vuex-new.png">


## 如何在项目中合理的使用vuex
- 基础用法
  ```JavaScript
      Vue.use(vuex)

      const app = new Vue({
        el: '#app',
        // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
        store,
        components: { Counter },
        template: `
          <div class="app">
            <counter></counter>
          </div>
        `
      })
  ```

  ```JavaScript
    state: {
      count: 0
    },
    mutations: {
      increment (state) {
        state.count++
      }
    },
    actions: {
      increment (context) {
        context.commit('increment')
      }
    }
  
  ```
- 方案一
- 方案二

