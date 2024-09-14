# 创建Vue3.0工程

## 使用vue.cli创建

- 使用命令

  ```
  ## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
  vue --version
  ## 或
  vue -V
  
  ## 安装或者升级你的@vue/cli
  npm install -g @vue/cli
  
  ## 创建
  vue create vue_test
  
  ## 启动（在脚手架的目录下）
  npm run serve
  ```

## 使用 vite 创建

- vlte官网：https://vitejs.cn

  - vite：下一代前端构建工具

    > 现在一代是webpack

  - 优势如下

    - 开发环境中，无需打包操作，可快速的冷启动
    - 轻量快速的热重载（HMR）
    - 真正的按需编译，不再等待整个应用编译完成

  - 使用命令

    ```
    ## 创建工程	（<project-name>为创建工程文件夹名）
    npm init vite-app <project-name>
    
    ## 进入工程目录
    cd <project-name>
    
    ## 安装依赖（）
    npm install
    ## 或
    npm i
    
    ## 运行
    npm run dev
    ```


# 常用Composition API

## setup

> Vue3.0中一个新的配置项，值为一个函数

1. 组件中所用到的：数据、方法等等，均要配置在setup中
2. setup函数的两种返回值：
   - 若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用
   - 若返回一个渲染函数：则可以自定义渲染内容
3. 说明
   1. 尽量不要与Vue2.x配置混用
      - Vue2x配置（data、methos、computed...）中可以访问到setup中的属性、方法
      - 但在setup中不能访问到Vue2.x配置（data、methos、computed...）
      - 如果有重名，setup优先
   2. setup不能是一个async函数，因为返回值不再是return的对象，而是promise，模板看不到return对象中的属性
4. setup的两个注意点
   - setup执行的时机
     - 在beforeCreate之前执行一次，this是undefined
   - setup的参数
     - props：值为对象，包含：组件外部传递过来的，且组件内部声明接收了的属性
     - context：上下文对象
       - attrs：值为对象，包含：组件外部传递过来的，但没有在props配置中声明的属性，相当于`this.$attrs`
       - slots：收到的插槽内容，相当于`this.$slots`
         - 在vue3中，插槽语法要使用 `v-solt:值`

## ref函数

- 作用：定义一个响应式的数据
- 语法：`const xxx = ref(initValue)`
  - 创建一个包含响应式数据的**引用对象（reference对象，简称ref对象）**
  - JS中操作数据：`xxx.value`
  - 模板中读取数据：不需要.value，直接：`<div>{{xxx}}</div>`
- 备注
  - 接收的数据可以是：基本类型、也可以是对象类型
  - 基本类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`完成的
  - 对象类型的数据：内部使用了Vue3.0中的一个新函数——`reactive`函数

## reactive函数

- 作用：定义一个**对象类型**的响应式数据（基本类型别用它，用`ref`函数）
- 语法：`const 代理对象 = reactive(被代理对象)`接收一个对象（或数组），返回一个**代理器对象（Proxy的实例对象，简称proxy对象）**
- reactive定义的响应式数据是“深层次的”
- 内部基于ES6 的 Proxy 实现，通过代理对象操作源对象内部数据都是响应式的

## Vue3.0中的响应式原理

### Vue2.x的响应式

- 实现原理

  - 对象类型：通过`Object.defineProperty()`对属性的读取、修改进行拦截（数据劫持）

  - 数组类型：通过重写更新数组的一系列方法来实现拦截（对数组的变更方法进行了包裹）

    ```javascript
    Object.defineProperty(data,'count',{
        get(){},
        set(){}
    })
    ```

- 存在问题

  - 新增属性、删除属性，界面不会更新
  - 直接通过下标修改数组，界面不会自动更新

### Vue3.0的响应式

- 实现原理

  - 通过Proxy（代理）：拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等
  - 通过Reflect（反射）：对被代理对象的属性进行操作

- 示例

  ```javascript
  new Proxy(person,{
      // 拦截读取属性值
      get(target,propName){
          console.log(`有人读取p身上的${propName}属性`)
          // return target[propName]
          return Reflect.get(target,propName)
      },
      // 拦截设置属性值或添加新属性
      set(target,propName,value){
          console.log(`有人修改了p身上的${propName}属性，修改后的值为${value}`)
          // target[propName] = value
          Reflect.set(target,propName,value)
      },
      // 拦截删除属性
      deleteProperty(target,propName){
          console.log(`有人删除了p身上的${propName}属性`)
          // return delete target[propName]
          return Reflect.deleteProperty(target,propName)
      }
  })
  ```

## reactive对比ref

- 从定义数据角度对比：
  - ref用来定义：<span style="color:red">**基本类型数据**</span>
  - reactive用来定义：<span style="color:red">**对象（或数组）类型数据**</span>
  - 备注：ref也可以用来定义<span style="color:red">**对象（或数组）类型数据**</span>，它内部会自动通过`reactive`转为<span style="color:red">**代理对象**</span>

- 从原理角度对比
  - ref通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）
  - reactive通过使用<span style="color:red">**Proxy**</span>来实现响应式（数据劫持），并通过<span style="color:red">**Reflect**</span>操作**源对象**内部的数据
- 从使用角度对比
  - ref定义的数据：操作数据**需要**`.value`，读取数据时模板中直接读取**不需要**`.value`
  - reactive定义的数据：操作数据与读取数据：均**不需要**`.value`

## 计算属性与监视

### computed函数

- 与Vue2.x中computed配置功能一致

- 写法

  ```javascript
  import {computed} from 'vue'
  setip(){
  	...
  	// 计算属性----简写	(不考虑计算属性被修改的情况)
  	let fullName = computed(()=>{
  		return person.firstName + '-' + person.lastName
  	})
  
  	// 计算属性----完整
  	let fullName = computed({
  		get(){
  			return person.firstName + '-' + person.lastName
  		},
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
  	})
  }
  ```

### watch函数

- 与Vue2.x中watch配置功能一致

- Vue3中watch语法

  ```
  watch(target,(newValue,oldValue)=>{})
  ```

- 注意

  - 监视reactive定义的响应式数据时：oldValue无法正确获取，强制开启了深度监视（deep配置失效）
  - 监视reactive定义的响应式数据中某个属性时：deep配置有效

- 示例

  ```javascript
  setup(){
      let sum = ref(0)
      let msg = ref('你好啊')
      let person = reactive({
        name:'张三',
        age:18,
        job:{
          j1:{
            salary:20
          }
        }
      })
  // 情况一：监视ref所定义的一个响应式数据
      // watch(sum,(nawValue,oldValue)=>{		// sum里面存着基本的值，不需要.value（如果写了.value，相当于 0）
      //   console.log('(sum变化了',nawValue,oldValue)
      // })
  
      // 情况二：监视ref所定义的多个响应式数据
      // watch([sum,msg],(nawValue,oldValue)=>{
      //   console.log('person变化了',nawValue,oldValue)
      // },{immediate:true})
  
      // 情况三：监视reactive所定义的一个响应式数据的全部属性，
      /*  注意：
          若监视的是reactive定义的响应式数据，则无法正确的获取oldValue
          若监视的是reactive定义的响应式数据，强制开启了深度监视（depp配置无效）
      */
        // watch(person.value,(newValue,oldValue)=>{
        //   console.log('person变化了',newValue,oldValue)
        // },{deep:false})  //此处depp配置无效
  
      // 情况四：监视reactive所定义的一个响应式数据中的某个属性
      // watch(()=>person.name,(newValue,oldValue)=>{   // ()=>person.name为基本字符串，可以拿到oldValue
      //   console.log('person的name变化了',newValue,oldValue)
      // })
  
      // 情况五：监视reactive所定义的一个响应式数据中的某些属性
      // watch([()=>person.name,()=>person.age],(newValue,oldValue)=>{
      //   console.log('person的name或age变化了',newValue,oldValue)
      // })
  
      // 特殊情况
      watch(()=>person.job,(newValue,oldValue)=>{
        console.log('person的name或age变化了',newValue,oldValue)  // 此次拿不到oblValue
      },{deep:true})  // 此次由于监视的是reactive所定义的对象中的某个属性，所以deep配置有效
  
  ```


### watchEffect函数

- watch的套路是：既要指明监视的属性，也要指明监视的回调
- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性
- watchEffect有点像computed:
  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值

- 示例

  ```vue
  // watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调
  watchEffect(()=>{
  	const x1 = sum.value
  	const x2 = person.age
  	console.log('watchEffect配置的回调执行了')
  })
  ```


## 生命周期

- Vue3.0提供了Composition API 形式的生命周期钩子，与Vue2.x中的钩子对应关系如下
  - beforeCreate和created 跟 setup() 平级
  - setup钩子的配置项
    - beforeMount => onBeforeMount
    - mountd => onMounted
    - beforeUpdate => onBeforeUpdate
    - updated => onUpdated
    - beforeUnmount => onBeforeUnmount
    - unmounted => onUnmounted

- 组合式API形式的生命周期钩子的执行时机，要比配置项形式快

## 自定义hook函数

- 什么是hook
  - 本质是一个函数，把setup函数中使用的Composition API进行了封装
  - 类似于vue2.x中的mixin
  - 自定义hook的优势：复用代码，让setup中的逻辑更清楚易懂
- 区别在于：js文件里不能使用vue2.x的api，而vue3的api（ref、reactive、钩子函数等等）可以使用

## toRef

- 作用：创建一个ref对象，其value值指向另一个对象中的某个属性值
- 语法：`const name = toRef(目标对象,'属性')`
- 应用：要将响应式对象中的某个属性单独提供给外部使用时
- 扩展：`toRefs`与`toRef`功能一致，但可以批量创建多个ref对象，语法：`toRef(person)`

# 其他Composition API

## shallowReactive 与 shallowRef

- shallowReactive：只处理最外层属性的响应式（浅响应式）
- shallowRef：只处理基本数据类型的响应式，不进行对象的响应式处理
- 什么时候使用
  - 如果有一个对象数据，结构比较深，但变化时只是外层属性变化 => shallowReative
  - 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生成新的对象来替换 => shallowRef

## readonly 与 shallowReaonly

- readonly：让一个响应式数据变为只读的（深只读）
- shallowReadonly：让一个响应式数据变为只读的（浅只读）
- 应用场景：不希望数据被修改时

## toRaw 与 markRaw

- toRaw
  - 作用
    - 将一个由`reacive`生成的**响应式对象**转成**普通对象**
    - 对`ref`无任何操作，返回ref基本数据
  - 使用场景：用于读取响应式对象对应的普通对象或ref定义的基本数据类型，对这个普通对象的所有操作，不会引起页面更新
- markRaw
  - 作用：标记一个对象，使其永远不会成为响应式对象
  - 应用场景
    1. 有些值不应设置为响应式的，例如复杂的第三方类库等
    2. 当渲染具有不可变数据源的大列表是，跳过响应式转换可以提高性能

## customRef

- 作用：创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显式控制

## provide和inject

- 作用：实现**祖与后代组件间**通信

- 说明：父组件有一个`provide`选项来提供数据，后代组件有一个`inject`选项来开始使用这些数据

- 具体写法

  1. 祖组件中

     ```javascript
     setup(){
     	let user = reactive({name:'张三',age:18})
         provide('name',name)
     }
     ```

  2. 后代组件中

     ```javascript
     setup(props,context){
     	const user = inject('uesr')
         return{user}
     }
     ```

## 响应式数据的判断

- isRef：检查一个值是否一个ref对象
- isReactive：检查一个对象是否由`reactive`创建的响应式代理
- isReadonly：检查一个对象是否由`readonly`创建的只读代理
- inProxy：检查一个对象是否是由`reactive`或者`readonly`方法创建的代理

# 新的组件

## Fragment

- 在Vue2中：组件必须有一个根标签
- 在Vue3中，组件可以没有根标签，内部会将多个标签含在一个Fragment虚拟元素中
- 好处：减少标签层级，减小内存占用

## Teleport

- 什么是Teleport

  - `Teleport`是一种能够将我们的组件html结构移动到指定位置的技术

    ```html
    <teleport to="移动位置">
        <div v-if="isShow" class="mask">
            <div class="dialog">
                <h3>我是一个弹窗</h3>
                <button @click="isShow = false">关闭弹窗</button>
            </div>
        </div>
    </teleport>
    ```

    - 移动位置：参数可以是html标签、CSS选择器（类名标签、id标签）

## Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```javascript
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用`Suspense`包裹组件，并配置好`default`与`fallback`

    ```html
    <template>
        <div class="app">
            <h3>我是App组件</h3>
            <Suspense>
                <template v-solt:default>
                	<Child/>
                </template>
                <template v-solt:fallback>
                    <h3>加载中......</h3>
                </template>
            </Suspense>
        </div>
    </template>
    ```

    