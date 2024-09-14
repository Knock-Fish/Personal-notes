# Vuex

> 多个组件需要共享数据时，使用Vuex

概念：在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信

原理图![](http://127.0.0.1:8888/uploads/202308191518746.png)

搭建vuex环境

1. 创建文件：src/store/index.js

   ```javascript
   // 引入Vue核心库
   import Vue from 'vue'
   // 引入Vuex
   import Vuex from 'vuex'
   // 应用Vuex插件
   Vue.ues(Vuex)
   
   // 创建actions对象——响应组件中用户的动作
   const actions = {}
   // 创建mutations对象——修改state中的数据
   const mutations = {}
   // 创建state 对象——保存具体的数据
   const state = {}
   
   // 创建并暴露 store
   export default new Vuex.Store({
       actions,
       mutations,
       state
   })
   ```

2. 在 main.js中创建vm时传入 store配置项

   ```javascript
   ......
   // 引入store
   import store from './store'
   ......
   
   // 创建vm
   new Vue({
   	el:""
   })
   ```


# 基本使用

`$store`属性在组件实例对象里

组件中读取vuex中的数据：`$store.state.sum`

组件修改vuex中的数据：`$store.dispatch('action中的方法名',数据)`或`$store.comit('mutations中的方法名',数据)`

> 若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写`dispatch`，直接编写`commit`

  ```js
// src/store/index.js文件
// 引入Vue核心库
import Vue from 'vue'
// 引入Vuex
import Vues from 'vuex'
// 引用Vuex
Vue.use(Vuex)

const actions = {
    // 响应组件中加的动作
    inSum(context,value){
        // console.log('actions中的inSum被调用了',context,value)
        context.commit('INSUM')
    }
}
const mutations = {
    // 执行加
    INSUM(state,value){
        // console.log('mutations中的INSUM被调用了',state,value)
        state.sum += value
    }
}

// 初始化数据
const state = {
    sum:0
}

// 创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state
})
  ```

# state

1. vuex 管理的状态对象

2. 它应该是唯一的

3. 示例代码

   ```vue
   const state = {
   	xxx:initValue
   }
   ```

# actions

1. 值为一个对象，包含多个响应用户动作的回调函数

2. 通过 commit( )来触发 mutation 中函数的调用, 间接更新 state

3. 如何触发 actions 中的回调?

   ```vue
   在组件中使用: $store.dispatch('对应的 action 回调名') 触发
   ```

4. 可以包含异步代码（定时器, ajax 等等）

5. 示例代码：

   ```vue
   const actions = {
   	zzz({commit, state}, data1){
   		commit('yyy',{data1})
   	}
   }
   ```

# mutations

1. 值是一个对象，包含多个直接更新 state 的方法

2. 谁能调用 mutations 中的方法？如何调用？

   ```vue
   在 action 中使用：commit('对应的 mutations 方法名') 触发
   ```

3. mutations 中方法的特点：不能写异步代码、只能单纯的操作 state

4. 示例代码

   ```vue
   const mutations = {
   	yyy(state, {data1}){
   		// 更新state的某个属性
   	}
   }
   ```

# getters配置项

概念：当state中的数据需要经过加工后再使用时，可以使用getters加工

值为一个对象，包含多个用于返回数据的函数

如何使用？—— **$store****.****getters****.****xxx**

在`store.js`中追加`getters`配置

```js
...
const getters = {
	bigSum(state){
		return state.sum * 10
	}
}
// 创建并暴露store
export default new Vuex.Store({
	...
	getters
})
// 组件中读取数据：$store.getters.bigSum
```

# 四个map方法的使用

> 首先要导入import {map方法} from 'vuex'

**mapState**方法：用于帮助我们映射`state`中的数据为计算属性

```js
computed:{
	// 借助mapState生成计算属性，sum、name、msg（对象写法）
    ...mapState({sum:'sum',name:'name',msg:'msg'}),
        
    // 借助mapState生成计算属性，sum、name、msg（数组写法）
    ...mapState(['sum','name','msg'])
}
```

**mapGetters**方法：用于帮助我们映射`getters`中的数据为计算属性

```js
computed:{
	// 借助mapGetters生成计算属性，bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),
        
    // 借助mapGetters生成计算属性，bigSum（数组写法）
    ...mapGetters(['bigSum'])
}
```

**mapActions**方法：用于帮助我们生成与`actions`对话的方法，即：包含`$store.dispatch(xxx)`的函数

```js
methods:{
    // 借助mapMutations生成对应的方法，方法中会调用commit去联系mutations（对象写法）
    ...mapMutations({increment:'INSUM',decrement:'DESUM'}),

    // 借助mapMutations生成对应的方法，方法中会调用commit去联系mutations（数组写法）
     ...mapMutations([INSUM','DESUM']),   // 记得调用要该 INSUM，DESUM
}
```

**mapMutations**方法：用于帮助我们生成与`mutations`对话的方法，即：包含`$store.commit(xxx)`的函数

```js
methods:{
	 // 借助mapActions生成对应的方法，方法中会调用dispatch去联系zctions（对象写法）
     ...mapActions({incrementOdd:'inSumOdd',incrementWait:'inSumWait'})

     // 借助mapActions 生成对应的方法，方法中会调用dispatch 去联系actions（数组写法）
     ...mapActions(['inSumOdd','inSumWait'])	
}
```

> 备注：mapActions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象

# **modules模块化+命名空间

> 目的让代码更好维护，让多种数据分类更加明确

修改store.js

```js
const countAbout = {
	namespaced:true,	// 开启命名空间
    state:{},
    mutations:{},
	actions:{},
	getters:{
        f(){
            return
        }
    }
}

const store = new Vuex.Store({
    modules:{
        countAbout
    }
})
```

开启命名空间后，组件中读取state数据

```js
// 方式一：直接读取
this.$store.state.personAbout.list
// 方式二：借助mapState读取
...mapState('countAbout',['sum','school'])
```

开启命名空间后，组件中读取getters数据

```js
// 方式一：直接读取
this.$store.getters['personAbout/firstPersonName']
// 方式二：借助mapGetters读取
...mapGetters('countAbout',['bigSum'])
```

开启命名空间后，组件中调用dispatch

```js
// 方式一：直接dispatch
this.$store.dispatch['personAbout/addPersonWang',person]
// 方式二：借助mapActions
...mapActions('countAbout',{insumOdd:'Odd',insumWait:'Wait'})
```

开启命名空间后，组件中调用commit

```js
// 方式一：自己commit
this.$store.commit('personAbout/ADD_PERSON',person)
// 方式二：借助mapMutations
...mapMutations('countAbout',{insum:'JIA',desum:'JIAN'})
```
