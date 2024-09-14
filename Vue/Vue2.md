# Vue模板

## 创建模板

使用Vue，首先导入Vue库文件，就能使用Vue构造函数

创建Vue模板

```vue
<body>
    <!-- 在页面中声明一个将要被 vue 所控制的 DOM 区域-->
    <div id="app"></div>
</body>

<script>
    // 创建 Vue 的实例对象
    new Vue({
        // el属性是固定的写法，表示当前 vm 实例要控制页面上的哪个区域
        el:'#app',
        // data 对象就是要渲染到页面上的数据
        data:{}
    })
</script>
```

## 插值语法（插值表达式）

插值语法：用于解析标签体内容（内容的占位符，不会覆盖原有的内容）

写法：`{{ 表达式 }}`

`{{ }}`里面是js表达式，且可以直接读取到 data 中的所有属性

```vue
<body>
    <div id="app">{{ uname }}</div>
</body>

<script>        
    new Vue({
        el:'#app',	// 指定id为app的容器
        data:{
            uname:'张三'
        }
    })
</script>
```

# 数据绑定

## 单向数据绑定 v-bind

`v-bind`：数据只能从data流向页面

指令作用：把属性与数据值绑定在一起

```vue
<标签名 v-bind:属性> </标签名>
<!-- 或 -->
<标签名 :属性> </标签名>
```

```vue
<body>
    <!-- v-bind 会从data数据中找到表达式对应的值 -->
    <div id="app">
        <a v-bind:href="url"></a>	<!-- 此时url是一个链接 -->
        <a :href="url"></a>
    </div>
</body>

<script>
    new Vue({
        el:"#app",
        data:{
            // 定义url数据
            url:'http://www.baidu.com'
        }
    })
</script>
```

## 双向数据绑定 v-model

`v-model`：数据不仅能从data流向页面，还可以从页面流向data

双向绑定一般都应用在表单类元素上（如：input、select等）

```vue
<表单标签 v-model:value="表达式">
<!-- 或 -->
<表单标签 v-model="表达式">
```

`v-model:value` 可以简写为 `v-model`，因为 `v-model` 默认收集的就是 value值

```vue
<body>
    <div id="app">
        <input type="text" v-model:value="name">	<!-- 此时输入框会有name值-->
        <input type="text" v-model="name">
        <!-- <h1 v-model:x="name"></h1>	此写法会报错！ -->
    </div>
</body>

<script>
	new Vue({
        el:"#app",
        data:{
            name:'你好'
        }
    })
</script>
```

v-model指令的修饰符

| 修饰符  | 说明                                             |
| ------- | ------------------------------------------------ |
| .number | 自动将用户的输入值转为数值类型                   |
| .trim   | 自动过滤用户输入的首尾空白字符（中间则无法去除） |
| .lazy   | 在”change“是而非”input“时更新（防抖）            |

# el和data的两种写法

## el

new Vue 时候配置 el 属性

先创建Vue实例，随后在通过`vm.$mount('#app')`指定el的值

```vue
<script>
    const v = new Vue({
        // el:"#app",		// 第一种写法
        data:{

        }
    })
    v.$mount("#app")		// 第二种写法
</script>
```

## data

data可以写成 对象式 或 函数式（**组件data必须使用函数式**）

由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了

```vue
<script>
    new Vue({
        el:'#app',
        // 1、data 的第一种写法：对象式
        data:{
            name:'zs'
        }

        // 2、data的第二种写法：函数式
        data:function(){
            console.log(this);		// 此次的this是Vue实例对象
            return{
                name:'zs'
            }
        }
        
        // 3、函数式简写
        data(){
            console.log(this);		// 此次的this是Vue实例对象
            return{
                name:'zs'
            }
        }
    })
</script>
```

# MVVM模型

1. M：模型（Model）：data中的数据
2. V：视图（View）：模板代码
3. VM：视图模型（ViewModel）：Vue实例
4. 发现
   - data中所有的属性，最后都出现在了 vm 身上（数据代理）
   - vm 身上所有属性，及 Vue原型上所有属性，在Vue模板中都可以直接使用

基本代码与MVVM的对应关系![](http://127.0.0.1:8888/uploads/202302231445888.jpg)

# Vue中的数据代理

- vm

  ```
  vm.data == vm._data;	// true
  ```

# 事件处理

## 事件的基本使用

> 原生DOM对象有onclick、oninput、onkeyup等原生事件，替换为vue的事件绑定形式后，**分别为：v-on:click、v-on:input、v-on:keyup**

v-on指令

```vue
v-on: 事件名
// 或
@事件名	// v-on可以简写为："@"
```

事件的回调需要配置在 <span style="color:red"><b>methods</b></span> 对象中，最终会在 vm 上

> 1. methods 中配置的函数，不要用箭头函数，否则 this 就不是 vm 了
>2. methods 中配置的函数，都是被Vue所管理的函数，this的指向是 vm 或 组件实例对象

**事件对象$event**

1. \$event的应用场景：如果**默认的事件对象 e** 被覆盖了，则可以手动传递一个 $event

2. 例如 @click = "demo" 和 @click="demo($event)" 效果一样，但后者可以转参

```vue
<body>
    <div id="app">
        <button v-on:click="showInfo1">点击弹出信息1（不传参）</button>
        <!-- $event位置任意，一般放于最前面 -->
        <button @click="showInfo2($event,99)">点击弹出信息2（事件对象和传参）</button>
    </div>
</body>

<script>
    new Vue({
        el: "#app",
        methods: {
            showInfo1() {
                alert('你好')
            },
            showInfo2(e, num) {
                console.log(e)
                console.log(num)
            }
        }
    });
</script>
```

## 事件修饰符

> 在事件处理函数中调用 `e.preventDefault()` 或 `e.stopPropagation() `是非常常见的需求。 可以调用vue提供了**事件修饰符**对事件的触发进行控制

Vue中的事件修饰符

| 修饰符   | 说明                                                 |
| -------- | ---------------------------------------------------- |
| .prevent | 阻止默认事件                                         |
| .stop    | 阻止事件冒泡                                         |
| .once    | 事件只触发一次                                       |
| .capture | 使用事件的捕获模式                                   |
| .self    | 只有在 event.target 是当前元素自身是触发事件处理函数 |
| .passive | 事件默认行为立即执行，无需等待事件回调执行完毕       |

## 键盘事件（按键修饰符）

在监听**键盘事件**时，可以为**键盘相关的事件**添加**按键修饰符**，来**判断详细的按键**

Vue中常用的按键别名

| 按键 | 别名                               |
| ---- | ---------------------------------- |
| 回车 | enter                              |
| 删除 | delete（捕获”删除“和“退格”键）     |
| 退出 | esc                                |
| 换行 | tab（特殊，必须配合keydown去使用） |
| 上   | up                                 |
| 下   | down                               |
| 左   | left                               |
| 右   | right                              |

1. Vue 未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为 kebab-case（短横线命名）
     - 如CapsLock（大小写），要写成caps-lock

2. 系统修饰符（用法特殊）：ctrl、alt、shift、meta

     - 配合keup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发

     - 配合keydown使用：正常触发条件

3. 也可以使用keyCode去指定具体的按键（不推荐，vue3已废除）

4. `Vue.config.keyCodes.自定义键名 = 键码`，可以去定制按键别名

   ```vue
   <body>
       <div id="app">
           <!--只有在‘key’是'Enter'时调用‘vm.submit() 或 是 'Esc' 时调用‘vm.clearInput()’-->
           <input @keyup.enter="submir" @keyup.esc="clearInput">
       </div>
   </body>
   
   <script>
   	new Vue({
           el: '#app',
           methods: {
               submir(){
                   console.log('按下了enter键')
               },
               clearInput(e){
                   console.log(`按下了esc键，值是${e.target.value}`)
                   e.target.value = ''
               }
           }
       });
   </script>
   ```

# 计算属性

定义：计算属性指的是已有属性**通过一系列运算**之后，最终得到一个**属性值**

原理：底层借助了 `Objcet.defineproperty` 方法提供的 `getter` 和 `setter`

> get函数的执行时机
>
> 1. 初次读取时会执行一次
> 2. 当依赖的数据方式改变时会被再次调用

优势：

1. 与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
2. 只要计算属性中依赖的数据源变化了，则计算属性会自动重新求值

备注：

1. 计算属性最终会出现在 vm上，直接读取使用即可
2. 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发送改变
3. computed定义的方法是以属性访问的形式调用的，methods定义的方法是以函数访问的形式调用的
4. 这个动态计算出来的属性值可以被模板结构 methods 方法使用

```vue
<body>
    <div id="app">
        <input type="text" v-model="name">
        <input type="text" v-model="age">
        <div>{{fullName}}</div>
    </div>
</body>

<script>
	new Vue({
        el: "#app",
        data: {
            name: 'zs',
            age: 18
        },
        computed: {
            fullName: {
                // get的作用：当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                // get的调用：初次读取fullName时，所依赖的数据发生变化时
                get() {
                    console.log('get被调用了')
                    return this.name + '-' + this.age
                },
                // 当fullName被修改时会调用set
                set(value) {
                    console.log('set:', value)
                }
            }
        }
    })
</script>
```

计算属性简写形式

```vue
<script>
    new Vue({
         el: '#app',
         data: {

         },
         computed: {
             // 简写为函数形式（相当于get）
             // 考虑读取不考虑修改的时候可以使用简写形式（不使用set）
             pon() {
                 console.log('get被调用了')
                 return this.name + '-' + this.age
             }
         }
     });
</script>
```

# 监视属性watch

## watch基本使用

当被监视的属性变化时，回调函数自动调用，进行相关操作

监视的属性必须存在，才能进行监视

### watch的两种写法

1. 通过 vm.$watch 监视

   ```vue
   <script>
       vm.$watch('sum',{
           immediate:true,		// 控制侦听器是否自动触发
           deep:true,			// 开启深度监视
           handler(newValue,oldValue){
               console.log('num被修改了', newValue,oldValue)
           }
       })
   </script>
   ```

2. new Vue 时传入watch 配置

   ```vue
   <body>
       <div id="app">
           <h2>今天天气是{{info}}</h2>
           <button @click="isWeather = !isWeather">切换天气</button>
       </div>
   </body>
   
   <script>
   	new Vue({
           el: '#app',
           data: {
               isWeather: true,
           },
           computed: {
               info() {
                   return this.isWeather ? '炎热的' : '凉爽的'
               }
           },
           watch: {
               // 监视info属性的变化
               info: {
                   handler(newVal, oldVal) {
                       console.log(`新：${newVal}，旧：${oldVal}`)
                   }
               },
               // 监视isWeather属性的变化
               isWeather: {
                   handler(newVal, oldVal) {
                       console.log(`新：${newVal}，旧：${oldVal}`)
                   }
               }
           }
       });
   </script>
   ```

### watch的 简写方式

1. new Vue 时传入watch 配置（简写形式）

   ```vue
   watch:{
      // 简写（不需要开启配件时可以简写）
       sum(newValue,oldValue){
           console.log(newValue,oldValue)
       }
   }
   ```

2. vm.$watch（简写形式）

   ```vue
   // 使用$watch
   // $watch 简写方式（不需要开启配件监视时可以简写）
   vm.$watch('sum',function(newValue,oldValue){
       console.log(newValue,oldValue)
   })
   ```

## 控制侦听器是否自动触发

默认情况下，组件在初次加载完毕后不会调用 watch 侦听器，如果想让 watch 侦听器立即被调用，则需要使用 `immediate`选项

> 使用侦听器的格式
>
> 1. 使用**方法（函数）格式**的侦听器无法在刚进入页面的时候，自动触发
> 2. 使用**对象**的侦听器可以通过 immediate 选项，进入页面让侦听器自动触发

immediate属性值

| 值    | 说明                                                   |
| ----- | ------------------------------------------------------ |
| true  | 页面初次渲染好之后，就立即触发当前的 watch侦听器       |
| false | 默认值，页面初次渲染好之后，不会触发当前的 watch侦听器 |

```vue
watch:{
    username:{
        // immediate默认值为 false
        // 页面初次渲染好之后，就立即触发当前的 watch
        immediate: true
        handler(newVal,oldVal){
            console.log(newVal,oldVal)
        }
    }
}
```

## 深度监视

> 如果 watch 侦听的是一个对象，如果对象里面属性的变化，则不会被watch侦听器所监听到。如果想让监听到对象里面的属性，则需要使用 `deep`选项

配置`deep:true`可以监测对象内部值改变（多层）

deep 属性值

| 值    | 说明                                                         |
| ----- | ------------------------------------------------------------ |
| true  | 开启深度监听，只要对象中任何一个属性发生变化，都会触发对象的侦听器 |
| false | 默认值。关闭深度监听                                         |

1. Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不监视对象内部值的改变（一层）
2. 使用wathch时根据数据的具体结构，决定是否采用深度监视


   ```vue
   watch:{
       sum:{
           immediate:true,		// 初始化时让handlet调用一下
           deep:true,			// 深度监视
           handler(newValue,oldValue){
               console.log('num被修改了', newValue,oldValue)
           }    	
       }
   ```

# computed 和 watch 之间的区别

- 区别
  1. computed能完成的功能，watch都可以完成
  2. watch能完成的功能，computed不一定能完成，例如：watch可以简写异步操作
  3. watch监听数据必须是data中声明过或者父组件传递过来的props中的数据，当数据变化时，触发其他操作，函数有两个参数
  4. watch不支持缓存，数据变，直接会触发相应的操作
- 两个重要的小原则：
  1. 所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
  2. 所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数，Promise的回调函数等），最好写成箭头函数，这样this的指向才是vm或组件实例对象

# 修改样式

## 修改class样式

写法

```javascript
:class="xxx"
// xxx可以是字符串、对象、数组
```

1. 字符串写法适用于：类名不确定，要动态获取
2. 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
3. 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用

```vue
<style>
    div{
        margin-bottom: 10px;
    }
    .wh{
        width: 200px;
        height: 100px;
        border: 1px solid grey;
    }
    .Red{
        background-color: red;
    }
    .SkyBlue{
        background-color: skyblue;
    }
    .Pink{
        background-color: pink;
    }
</style>
<body>
	 <div id="app">
         <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
         <div class="wh" :class="mood" @click="changMood">{{name}}</div>

         <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定，名字也不确定 -->
         <div class="wh" :class="classAry">{{name}}</div>

         <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
         <div class="wh" :class="classObj">{{name}}</div>
    </div>
</body>
<script>
	 new Vue({
         el:'#app',
         data:{
             name:'我是盒子',
             // 字符串写法
             mood:"Red",
             // 数组写法
             classAry:['Red', 'SkyBlue', 'Pink'],
             // 对象写法
             classObj:{
                 Red: false,
                 SkyBlue: false
             }
         },
         methods: {
             // 绑定点击事件，点击随机样式
             changMood(){
                 const arr = ['Red', 'SkyBlue', 'Pink'];
                 let index = Math.floor(Math.random()*3);
                 this.mood = arr[index];
             }
         },
     })
</script>
```

## 修改style样式

写法

```javascript
:style="{fontSize: xxx}"
// 其中xxx是动态值

:style="[a, b]"
//其中a、b是样式对象
```

```vue
<style>
    div{
            margin-bottom: 10px;
        }
    .wh{
        width: 200px;
        height: 100px;
        border: 1px solid grey;
    }
</style>

<body>
	 <div id="app">
         <!-- 绑定style样式--对象写法-->
         <div class="wh" :style="styleObj">{{name}}</div>

         <!-- 绑定style样式--数组写法-->
         <div class="wh" :style="styleAry">{{name}}</div>
     </div>
</body>

<script>
	new Vue({
        el:'#app',
        data:{
            name:'我是盒子',
            styleObj:{
                fontSize:'40px',
                color:'red',
            },
            styleObj2:{
                backgroundColor:'blue'
            },
            styleAry:[
                {
                    fontSize:'40px',
                    color:'red',
                },
                {
                    backgroundColor:'blue'
                }
            ]
        }
    })
</script>
```

# 条件渲染

## v-if

> 适用于：切换频率较低的场景
>
> 特点：不展示的DOM元素直接被移出
>
> 注意：`v-if`可以和`v-else-if`、`v-else`一起使用，但要求结构不能被 “ 打断 ”

```vue
v-if="表达式"
v-else-if="表达式"
v-else="表达式"
```

```vue
<body>
    <div id="app">
        <div v-if="num === 1">一</div>          <!--当num = 1时显示-->
        <div v-else-if="num === 2">二</div>     <!--当num = 2时显示-->
        <div v-else-if="num === 3">三</div>     <!--当num = 3时显示-->
        <div v-else>零</div>                    <!--当num不是以上值时显示-->
        <button @click="num++">点击num++</button>
    </div>
</body>
<script>
	new Vue({
        el:'#app',
        data:{
            num:0
        }
	});
</script>
```

## v-show

> 适用于：切换频率较高的场景
>
> 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉
>
> 备注：使用 v-if 的时候，元素可能无法获取到，而使用v-show可以获取到

```vue
v-show="表达式"
```

```vue
<body>
    <div id="app">
        <h2 v-show="a">Vue</h2	>
        <!-- a如为true则显示，false为隐藏 -->
        <button @click="a = !a">显示/隐藏</button>
    </div>
</body>

<script>
	new Vue({
        el: '#app',
        data: {
            a: true
        }
	});
</script>
```

# 列表渲染

## v-for指令

> 用于展示列表数据
>
> 可遍历：数组、对象、字符串（用的少）、指定次数（用的少）

```vue
v-for="(item,index) in xxx" :key="id"
// xxx：所遍历的对象
// item：遍历的每一项
// index：遍历的每一项的索引
```

```vue
<div id="app">
    <ul>
        <!-- 遍历数组 -->
        <li v-for="(p,index) in persons" :key="index">   <!--括号可省略（不建议）-->
            {{p.id}}-{{p.name}}-{{p.age}}
        </li>
    </ul>
    
      <ul>
        <!-- 遍历对象 -->
        <li v-for="(value, k) of car" :key="k">     <!--for in 和 for of 一样-->
            {{k}}-{{value}}
        </li>
    </ul> 
    
        <ul>
        <!-- 遍历字符串 -->
        <li v-for="(char, index) in str" :key="index">     <!--for in 和 for of 一样-->
            {{index}}-{{char}}
        </li>
    </ul>
    
    <ul>
        <!-- 遍历指定次数 -->
        <li v-for="(number, index) in 5" :key="index">     <!--for in 和 for of 一样-->
            {{index}}-{{number}}
        </li>
    </ul>
</div>

<script>
	const vm = new Vue({
        el:'#app',
        data:{
            // 遍历数组
            persons:[
                {id:'01', name:'张三',age:18},
                {id:'02', name:'李四',age:20},
                {id:'03', name:'王五',age:21}
            ],
            // 遍历对象
            car:{
                name:'宝马',
                price:'100万',
                color:'黑色'
            },
            // 遍历字符串
            str:'HelloWorld',
        }
    });
</script>
```

## key的作用

react、vue中的key有什么作用?（key内部原理）
> 虚拟DOM中key的作用：
>
> - key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

对比规则：
> 旧虚拟DOM中找到了与新虚拟DOM相同的key：
>
>   - 若虚拟DOM中的内容没变，直接使用之前的真实DOM
>
>   - 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换页面中之前的真实DOM
>
> 旧虚拟DOM中未找到与新虚拟DOM相同的key
>
> - 创建新的真实DOM，随后渲染到页面

用index作为key可能会引发的问题：

> 1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作：
>      - 会产生没有必要的真实DOM更新：界面效果没问题，但效率低
>
> 2. 如果结构中还包含输入类的DOM：
>
>      - 会产生错误DOM更新：界面有问题
>
> 3. 开发中如何选择key？
>
>    - 最好使用每条数据的唯一标识作为key，比如id、手机、身份证号、学号等唯一值
>
>    - 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示

# Vue监视数据的原理（set方法）

vue会监视data中所有层次的数据

通过setter实现监视，且要在new Vue 时就传入要监测的数据

> <span style="color:red;">对象中后追加的属性，Vue默认不做响应式处理</span>

1. （数组或对象）如需给添加的属性做响应式，使用`Vue.set `或` Vue.$set `方法

   ```vue
   Vue.set(target, propertyName/index, value)	// 使用此方法，须先导入import Vue from "vue"	
   // 或
   Vue.$set(target, propertyName/index, value)
   ```

2. 若想移除对象中某个响应式属性，可以使用`Vue.$delete`

   ```vue
   Vue.$delete(obj,'target')
   // 或
   Vue.delete(obj,'target')
   ```

如何监测数组中的数据

> 通过包裹数组更新元素的方法实现，本质就是做了两件事：
>
> 1. 调用原生对应的方法对数组进行更新
> 2. 重新解析模板，进而更新页面

在Vue**修改数组**中的某个元素一定要用如下方法：

> 使用这些API：`push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`
>
> - 这些API为Vue自己封装的，**Vue会对使用这些API的对象绑定响应式**
>
> `Vue.set() ` 或 `vm.$set()`

注意：`Vue.set()` 和` vm.$set()` 不能给 vm 或 vm 的根数据对象添加属性

比如给 student 添加属性和值

```vue
<body>
    <div id="app">
        <!-- 添加对象属性 -->
        <div v-for="(k,value) in student" :key="value">
            <h3>{{k}}---{{value}}</h3>
        </div>
        <button @click="addGender">添加性别</button>
        <button @click="rmAge">删除年龄</button>

        <div v-for="(p,index) in student.hobby" :key="index">
            <h3>{{index}}---{{p}}</h3>
        </div>
        <button @click="setHobby">修改爱好</button>
        <button @click="addHobby">添加爱好</button>
    </div>
</body>
<script>
    new Vue({
        el: '#app',
        data: {
            student: {
                name: "张三",
                age: 18,
                hobby: ['游戏','学习','睡觉']
            }
        },
        methods: {
            addGender() {	// 添加性别
                Vue.set(this.student, 'gender', '男')
                // this.$set(this.student,'gender','男')
            },
            rmAge() {		// 删除年龄
                Vue.delete(this.student, 'age')
                // this.$delete(this.student,'age')
            },
            setHobby() {	// 修改数组元素
                Vue.set(this.student.hobby, 1, '吃饭')
                // this.$set(this.student.hobby,1,'吃饭')
            },
            addHobby() {	// 添加数组元素（调用vue封装的push方法）
                this.student.hobby.push('敲代码')
            }
        },
    });
</script>
```

# 收集表单数据

收集表单数据
> 若：`<input type="text"/>`，则v-model收集的是value值，用户输入的就是value值
>
> 若：`<input type="radio"/>`，则v-model收集的是value值，且要给标签配置value值
>
> 若：`<input type="checkbox"/>`
>
> - 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
> - 配置input的value属性：
>   1. v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
>   2. v-model的初始值是数组，那么收集的就是value组成的数组

```vue
<body>
    <div id="app">
        <form @submit.prevent="demo">
            账号：<input type="text" v-model.trim="userInfo.account"> <br /><br />
            密码：<input type="password" v-model="userInfo.password"> <br /><br />
            年龄：<input type="number" v-model.number="userInfo.age"> <br /><br />
            性别：
            男<input type="radio" name="gender" v-model="userInfo.gender" value="male">
            女<input type="radio" name="gender" v-model="userInfo.gender" value="female"> <br /><br />
            爱好：
            学习<input type="checkbox" v-model="userInfo.hobby" value="study">
            打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
            吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat"> <br /><br />
            所属校区
            <select v-model="userInfo.city">
                <option value="">请选择校区</option>
                <option value="beijing">北京</option>
                <option value="shanghai">上海</option>
                <option value="shenzhen">深圳</option>
                <option value="wuhan">武汉</option>
            </select> <br /><br />
            其他信息：
            <textarea v-model.lazy="userInfo.other"></textarea><br /><br />
            <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.baidu.com">《用户协议》</a>
            <button>提交</button>
        </form>
    </div>
</body>
<script>
    new Vue({
        el: '#app',
        data: {
            userInfo: {
                account: '',
                password: '',
                age: '',
                gender: 'male',
                hobby: [],
                city: 'beijing',
                other: '',
                agree: false
            }
        },
        methods: {
            demo() {
                console.log(JSON.stringify(this.userInfo))
            }
        }
    });
</script>
```

# 过滤器（Filters）

## 过滤器的基本使用

> 定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）
>
> 说明：
>
> 1. 过滤器可以用在两个地方：**插值表达式** 和 v**-bind 属性绑定**
>
> 2. 在过滤器函数中，一定要有 retern 值
>
> 3. 过滤器并没有改变原本的数据，是产生新的对应的数据
>
> 4. 如果全局过滤器和私有过滤器名字一致，此时按照“就近原则”，调用的是“私有过滤器”

过滤器使用（注册过滤器）

1. 全局过滤器

   ```vue
   // 全局过滤器（能在多个vue实例之间共享过滤器）
   Vue.filter('name', callback)
   
   // 	第 1 个参数，是全局过滤器的“名字”
   // 	第 2 个参数，是全局过滤器的“处理函数”，function(任起名) 括号内为管道符前面的值
   ```

     - 全局过滤器需要声明到创建实例前，否则会报错


2. 局部过滤器

   ```vue
   // 局部过滤器（只能在当前vm实例所控制的el区域内使用）
   new Vue{
       filters:{
           function(){}
       }
   }
   ```

3. 使用过滤器：{{ 表达式| 过滤器名 }} 或 v-bind:属性 = "表达式 | 过滤器名"（`|`为管道符）

   ```vue
   <body>
       <div id="app">
           <!-- 在插值语法中通过“管道符”调用 str 过滤器，对 n 的值进行格式化 -->
           <h2>{{n | str}}</h2>    <!--格式化后的插值为 “你好”-->
   
           <!-- 在 v-bind 中通过“管道符”调用 formatId 过滤器，对 rawId 的值进行格式化 -->
           <h2 v-bind:id="rawId | formatId"></h2>  <!--格式化后的id为demo-->  
       </div>
   </body>
   
   <script>
   	// 定义全局过滤器
       const str = Vue.filter('str',function(){
           return '你好'
       })
   
       // Vue实例对象
       new Vue({
           el:'#app',
           data:{
               n:'999',
               rawId:'test'
           },
           filters:{
               num(){
                   return '123'
               },
               formatId(){
                   return 'demo'
               },
               str		// 传入 str全局过滤器
           }
       });
   </script>
   ```

## 多个过滤器串联 和 传参

多个过滤器也可以串联

过滤器的本质是 JavaScript 函数，因此可以接收参数

> 过滤器处理函数的形参列表中
>
> - 第一参数：永远都是“管道符”前面待处理的值
> - 第二参数，才是调用过滤器时传递过来的 arg1 和 arg2 参数

```vue
<body>
    <div id="app">
         <!-- 多个过滤器串联调用 -->
         <!-- 把 num 的值，交给 filterA 进行处理-->
         <!-- 把 filterA 处理的结果，再交给 filterB 进行处理 -->
         <!-- 最终把 filter 处理的结果，作为最终的值渲染到页面上 -->
         <h2>{{num | filterA | filterB}}</h2>	<!-- 9990 -->
        
         <!-- 过滤器传参 -->
         <h2>{{num | f(2,3)}}</h2>	<!-- 15 -->
    </div>
</body>

<script>
	 new Vue({
     el: '#app',
     data: {
         num: 10
     },
     filters: {
         filterA() {
             return 999
         },
         // 过滤器处理函数的形参列表中
         filterB(value) {	 // 第一参数：永远都是“管道符”前面待处理的值
             return value * 10
         },
         // 过滤器传参
         f(value,n1,n2){	 // 从第二个参数开始，才是调用过滤器时传递过来的 n1 和 n2 参数
             return value = value + n1 + n2
         }
     }
 });
</script>
```

# 内置指令

## v-text

作用：向其所在的节点中渲染文本内容

`v-text`指令的缺点：会覆盖元素内部原有的内容

与插值语法的区别：`v-text`会替换掉节点中的内容，`{{ }}`则不会

```vue
<body>
    <div id="app">
        <!-- 把 name 对应的值，渲染到 p标签中 -->
        <h2 v-text="name"></h2>
        <!-- p标签中如已有默认的内容而且有加v-text，则会覆盖原内容-->
        <h2 v-text="age">99</h2>
    </div>
</body>

<script>
	new Vue({
        el:'#app',
        data:{
            name:'张三',
            age:18
        }
    });
</script>
```

## v-html

作用：向指定节点中渲染包含html结构的内容

> 与插值语法的区别：
>
> 1. `v-html` 会替换掉节点中所有的内容，`{{ }}`则不会
> 2. <span style="color:red;">v-html可以识别 html 结构</span>

> 严重注意：v-html有安全性问题
>
> 1. 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击
> 2. 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上

```vue
<body>
    <div id="app">
     <!-- v-html将包含html结构的内容渲染到p标签-->     
        <p v-html="info"></p>
    </div>
</body>

<script>
	 new Vue({
         el:'#app',
         data:{
             info:'<h1 style="color:red;">你好</h1>'
         }
     });
</script>
```

## v-clock

v-clock（没有值）本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性

使用css配合v-cloak可以解决网速慢时页面展示出 <span style="color:red;">未渲染的{{ xxx }}</span> 的问题

```vue
<style>
    [v-clock]{
        display:none;
    }
</style>

<body>
	<div id="app">
    	<h1 v-clock>{{msg}}</h1>
	</div>
</body>

<script>
	new Vue({
        el:'#app',
        data:{
            msg:'你好呀'       
        }
    });
</script>
```

## v-once

v-once所在节点在初次动态渲染后，就视为静态内容了

> 以后数据的改变不会引起 v-once 所在结构的更新，可以用于优化性能

```vue
<body>
    <div id="app">
         <!-- v-once在初次渲染后， -->
         <h2 v-once>n初始值为：{{n}}</h2>
         <h2>n的值{{n}}</h2>
         <button @click="n++">点击n++</button>
    </div>
</body>

<script>
	 new Vue({
         el:'#app',
         data:{
             n:0
         }
     });
</script>
```

## v-pre

跳过其所在节点的编译过程

可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

## 自定义指令

### 局部指令

配置项

```vue
directives:{})
// 或
directives(){}
```

> 配置对象中常用的3个回调：
>
> 1. bind：指令与元素成功绑定时调用
> 2. inserted：指令所在元素被插入页面时调用
> 3. update：指令所在模板结构被重新解析时调用

函数式写法

```vue
directives:{
    // element：被绑定的标签的DOM元素
    // binding：Object对象（包含绑定元素的值及其他方法）
    指令名([element],[binding]){
        配置对象
    }
})
```

对象式写法

```vue
directives:{
    指令名:{
        bind(){}
        inserted(){}
        updata(){}
    }
})
```


### 全局指令

配置项

```vue
Vue.directive(指令名，{
        bind(){}
        inserted(){}
        updata(){}
})
// 或
Vue.directive(指令名,回调函数([element],[binding]){
              
})
```

> 使用指令
>
> 1. 指令定义不加 v-，但使用时前面要加 v-
>
> 2. 指令名如果是多个单词，要使用 kebab-case命名方式（单词之间横杠分隔，整个要引号包裹），不要用 camelCase （驼峰命名法）命名

```javascript
  kebab-case命名方式：'user-name'	// 单词之间横杠分隔，整个要引号包裹
  camelCase命名方式：userName		// 使用此类自定义指令名会报错
```

自定义指令_函数式

```vue
  <body>
      <div id="app">
           <h2>{{name}}</h2>
           <h2>n放大10倍后的值：<span v-big="n"></span></h2>
      </div>
  </body>
  
  <script>
  	new Vue({
          el: '#app',
          data: {
              // 如果修改name的值，big函数所在的模板被重新解析时会重新调用big函数
              name:'张三',
              n:1
          },
          directives:{
              // 指令与元素成功绑定时，big函数就会被调用
              // 指令所在的模板被重新解析时，big函数也会被调用
              big(element,binding){
                  element.innerText = binding.value * 10
  
                  // console.dir(element)
                  // console.log(element,binding)
                  console.log('big函数被调用')
              }
          }
      });
  </script>
```

自定义指令_对象式

```vue
  <body>
      <div id="app">
          <!-- input和v-fbind在内存当中先绑定，此时input还没进入页面，如用函数式定义无法一进入页面获取焦点-->
          <input type="text" v-fbind:value="n">
      </div>
  </body>
  
  <script>
  	new Vue({
          el: '#app',
          data: {
              n: 1
          },
          directives: {
              // 一进入页面自动获取input焦点
              fbind: {
                  // 指令与元素成功绑定时调用（在内存中绑定）
                  bind(element,binding) {
                      element.value = binding.value
                  },
                  // 指令所在元素被插入页面时调用
                  inserted(element,binding) {
                      element.focus()
                  },
                  // 指令所在模板结构被重新解析时调用
                  updata(element,binding) {
                      element.value = binding.value
                      element.focus()
                  }
              }
          }
      });
  </script>
```

# 生命周期

> 概念:
>
> 1. 又名：生命周期回调函数、生命周期函数、生命周期钩子
> 2. 是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数
> 3. 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
> 4. 生命周期函数中的this指向是 vm 或 组件实例对象

> 常用的生命周期钩子：
>
> 1. mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
> 2. beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【首尾工作】

> 关于销毁Vue实例
>
> - 销毁后借助Vue开发者工具看不到任何信息
> - 销毁后自定义事件会失效，但原生DOM事件依然有效
> - 一般不会在beforeDestroy操作数据，因为即使操作数据，叶不会再触发更新流程了

![](http://127.0.0.1:8888/uploads/202308191509341.png)

vue生命周期分析

1. 初始化显示
   - beforeCreate()
   - created()
   - beforeMount()
   - mounted()
2. 更新状态：this.xxx = value
   - beforeUpdate()
   - updated()
3. 销毁vue实例：vm.$destory()
   - beforeDestory()
   - destoryed()

常用的生命周期方法

- mounted()：发送ajax请求，启动定时器等异步任务
- beforeDestory()：做收尾工作，如：清除定时器

# 组件

## 非单文件组件

定义：实现应用中局部功能代码和资源的集合

> Vue中使用组件的三大步骤
>
> 1. 定义组件（创建组件）
> 2. 注册组件
> 3. 使用组件（写组件标签）

定义组件
> 使用`Vue.extend(options)`创建，其中 options 和 new Vue(options) 时传入的那个options几乎一样，但区别如下：
>
> - el不要写。最终终所有的组件经过一个vm的管理，由vm中的el决定服务哪个容器
> - data必须写成函数。避免组件被复用时，数据存在引用关系

备注：使用`template`可以配置组件结构

### 注册组件

局部注册：new Vue 的时候传入 components 选项

```vue
new Vue({
	el:'',
	data:{},
	components:{ 组件名 }
})
```

全局注册

```vue
Vue.component('真正的组件名', 创建的组件)
```

- 全局组件的一个简写方式

  ```vue
  const 组件名 = Vue.extend(options) 
  可简写为：const 组件名 = options
  ```

编写组件标签

```vue
<组件名></组件名>
<组件名/>
```

备注：不用使用脚手架时，\<组件名/\>会导致后续组件不能渲染

> 关于组件名
>
> 1. 一个单词组成：
>
>      - 第一种写法（首字母小写）：say
>
>      - 第二种写法（首字母大写）：Say
>
> 2. 多个单词组成
>
>      - 第一种写法（kebab-case命名）：my-say
>
>      - 第二种写法（CameCase命名）：MySay（需要Vue脚手架支持）
>
> 3. 注意
>
>      - 组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行
>
>      - 可以使用name配置项指定组件在开发者工具中呈现的名字


   ```vue
   <body>
       <div id="app">
            <!-- 组件标签 -->
            <Name></Name>
       </div>
   <body>
       
   <script>
       const Say = Vue.extend({
           name:'mysay',	// name 配置项指定组件在开发者工具中呈现的名字
           template:`
                   <div>
                       <h2>{{say}}</h2>    
                   </div>
                   `,
           data(){
               return{
                   say:'你好'
               }
           }
       })
       const vm = new Vue({
           el:'#app',
           components:{
               // 完整写法：Say:Say（真正的组件名:创建的组件名）
               Say    // 如创建和注册的组件名一致可简写
           }
       });
   </script>
   ```

### 组件的嵌套

组件的嵌套

```vue
<body>
	<div id="app"></div>
</body>

<script>
    const student = Vue.extend({	// 	student组件
        template: `
            <div>
                <h2>{{ name }}</h2>    
            </div>
        `,
        data() {
            return {
                name: '张三'
            }
        }
    })
    const school = Vue.extend({		//  school组件（嵌套student子组件）
        template: `
            <div>
                <h2>{{ name }}</h2>
                <student></student>
            </div>
        `,
        data() {
            return {
                name: '蓝翔'
            }
        },
        components: {
            student		//  使用子组件之前，必须把定义好的子组件写在父组件上方
        }
    })
    const hello = Vue.extend({		    // hello组件
        template: `
            <div>
                <h2>{{ say }}</h2>    
            </div>
        `,
        data() {
            return{
                say:'你好呀'
            }
        }
    })
    const app = Vue.extend({		//  app专门管理所有组件
        template: `
            <div>
                <school></school>
                <hello></hello> 
            </div>
        `,
        components: {				//   注册组件
            school,
            hello
        }
    })

    const vm = new Vue({
        template: `
            <div>
                <app></app>  
            </div>
        `,
        el: '#app',
        components: {
            app			// 注册 app 组件
        }
    });
</script>
```

## 关于VueComponent

创建的组件本质是一个名为<span style="color:red"> VueComponent</span>的构造函数，且不是程序员定义的，是 **Vue.extend** 生成的

> 只需要写 \<组件名/\> 或 <组件名></组件名>，Vue解析时会帮我们创建组件的实例对象，即Vue帮我们执行的：`new VueComponent(options)`

特别注意：每次调用 Vue.extend，返回的都是一个全新的 VueComonent

> 关于 this 指向
>
> 1. 组件配置中：
>    - data函数、methods中的函数、watch中的函数、computed中的函数 它们的 this 均是【VueComponent实例对象】
> 2. new Vue(options) 配置中：
>    - data函数、methods中的函数、watch中的函数、computed中的函数 它们的 this 均是 【Vue实例对象】

VueComponent 的实例对象可称之为：**组件实例对象**

内置关系（一个重要的关系）

```javascript
VueComponent.prototype.__proto.__ === Vue.prototype		// true
```

- 为什么要有这个关系
  - 让组件实例对象（VueComponent）可以访问到 Vue原型上的属性、方法

## 单文件组件

### 脚手架

> Vue脚手架是Vue官方提供的标准化开发工具（开发平台）
>
> 文档：https://cli.vuejs.org/zh/

#### 安装步骤

> 第一步（仅第一次执行）：全局安装@vue/cli

```
npm install -g @vue/cli
```

> 第二步：**切换到你要创建项目的目录**，然后使用命令创建项目

```
vue create 要创建的目录名
```

> 第三步：启动项目

```
npm run serve
```

备注：如出现下载缓慢请配置 npm 淘宝镜像：

```
npm config set registry "https://registry.npm.taobao.org"
```

- Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpack 配置，请执行：`vue inspect > output.js`


#### 脚手架文件结构

Tree目录结构

```
├─node_modules
├─public
| ├── favicon.ico：页签图标
| └── index.html：主页面    
├──src
|  ├── assets：存放静态资源
|  |  	└── logo.png
│  ├── components：存放组件
|  |    └── HelloWorld.vue
|  ├── App.vue：汇总所有组件
|  ├── main.js：入口文件   
├── .gitignore：git版本管制忽略的配置
├── babel.config.js：babel的配置文件
├── sconfig.json
├── package-lock.json：包版本控制文件
├── package.json：应用包配置文件
├── README.md：应用描述文件
└── vue.config.js：Vue脚手架的默认配置
```

### 关于不同版本的Vue

1.  vue.js 与 脚手架里的 vue.runtime.xxx.js的区别

   - vue.js是完整版的Vue，包含：核心功能 + 模板解析器
   - vue.runtime.xxx.js是运行版的Vue，只包含：核心功能：没有模板解析器

2. 因为vue.runtime.xxx.js没有模板解析器，所有不能使用 template 配置项，需要使用 render 函数接收的 createElement 函数去指定具体内容


### render函数

render函数

```javascript
render(createElement){
    
}
render:h => ('h1','你好')
```

### vue.config.js配置文件

- Vue脚手架隐藏了所有 webpack相关的配置
  - 使用` vue inspect > output.js`可以查看到Vue脚手架的具 体webpack 默认配置（仅查看，修改无效果）

- 使用 vue.config.js 可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh

# ref 属性

1. 作用：用于给节点打标识

2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上获取的是组件实例对象（VueComponent）

3. 使用方式

   - 使用 ref 标识

     ```html
     <h1 ref="名称">...</h1>
     或
     <组件标签 ref="名称"></组件标签>
     ```

   - 使用 $refs 获取

     ```javascript
     this.$refs.ref名称
     ```

   - 脚手架App汇总组件

     ```html
     <template>
       <div>
         <!-- ref标识 -->
         <h1 v-text="msg" ref="title"></h1>
         <button ref="btn" @click="showDOM">点击获取上方DOM元素</button>
         <MyDemo ref="demo"/>
       </div>
     </template>
     
     <script>
     // 导入MyDemo组件      
     import MyDemo from './components/MyDemo'
         
     export default {
       name: 'App',
       data(){
         return{
           msg:'你好'
         }
       },
       methods:{
         showDOM(){
           // $refs获取
           console.log(this.$refs.title) // 真实DOM元素
           console.log(this.$refs.btn)   // 真实DOM元素
           console.log(this.$refs.demo)  // MyDome组件的实例对象（VueComponent）
         }
       },
       // 注册MyDemo组件
       components: {MyDemo}
     }
     </script>
     ```

# 配置项props

- 作用：用于父组件给子组件传递数据

- 备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据

  - 传递数据

    ```javascript
    <组件名 name="xxx" age="xx"/>		// 给组件传递数据
    ```

  - 接收数据

    1. 第一种方式（只接收）：

       ```javascript
       props:['name','age']
       ```

    2. 第二种方式（指定名称和限制类型）：

       ```javascript
       props:{
       	name:String,
       	age:Number
       }
       ```

    3. 第三种方式（指定名称、限制类型、限制必要性、指定默认值）：

       ```
       props:{
       	name:{
       		type:String,  		// 类型
       		required:true,		// 必要性
       		default:'张三'	   // 默认值
       	}
       }
       ```

# $attrs

- 

# mixin（混入）

> 混入即把组件共用的配置提取成一个混入对象，混合即复用配置

- 功能：可以把多个组件共用的配置提取成一个混入对象

- 说明

  - data 里面的数据不会被混合的数据覆盖，不相同的混入，相同的以组件的配置项为主
  - mounted 会把混合的数据和自身的数据混入一起（会优先输出混合的数据，后输出自身的数据）

- 使用方式

  1. 第一步定义混合，例如

     ```javascript
     {
     	data(){...},
     	methods:{...},
         mounted(){...},
     	...
     }
     ```

  2. 第二步使用混入，例如

     - 全局混入：Vue.mixin(xxx)
       - 备注： 全局混入vm和VueComponent也会得到混入的数据
     - 局部混入：mixins:['xxx']
       - 备注：配置项

# 插件

- 功能：用于增强Vue

- 本质：包含install方法的一个对象，install的第一个参数是Vue，第二个的参数是插件使用者传递的数据

- 使用插件

  ```vue
  Vue.use(插件名, 实参1, ...)
  ```

- 定义插件

  ```vue
  {
  install = function(Vue, 形参1, ...){
  	1、添加全局过滤器
  	2、添加全局指令
  	3、配置全局混入（合）
  	4、添加实例方法
  	}
  }
  ```

# 组件里的样式（预编译）

- scoped样式

  - 作用：让样式在局部生效，防止冲突

  - 写法

    ```
    <style scoped> </style>
    ```

- lang属性

  - 作用：指定style类型为 css、scss 或 less（默认不写lang就为css）

  - 写法 

    ```
    <style lang="less"> </style>
    ```

# 组件的自定义事件

1. 一种组件间通信的方式，适用于：**子组件 ===> 父组件**

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给绑定自定义事件（事件的回调在A中）

3. 绑定自定义事件

   - 第一种方式：在父组件中

     ```vue
     <组件名 @自定义事件名="处理事件"/>
     // 或 
     <组件名 v-on自定义事件名="处理事件"/>
     ```

   - 第二种方式

     ```vue
     <组件名 ref="demo"/>
     ......
     mounted(){
     	this.$refs.xxx.$on('自定义事件名',this.处理事件)
     }
     ```

     - 若想让自定义事件只能触发一次，可以使用`once`修饰符，或`$once`方法

4. 触发自定义事件：`this.$emit('自定义事件名',数据)`

5.  解绑自定义事件：`this.$off('自定义事件名')`

6. 组件上也可以绑定原生DOM事件，需要使用`native`修饰符

7. 注意：通过`this.$refs.xxx.$on('自定义事件名', 回调)`绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出现问题

# 全局事件总线（GlobalEventBus）

Vue 原型对象上包含事件处理的方法

> `$on(eventName, listener)`: 绑定自定义事件监听
>
> `$emit(eventName, data)`: 分发自定义事件
>
> `$off(eventName)`: 解绑自定义事件监听
>
> `$once(eventName, listener)`: 绑定事件监听, 但只能处理一次

所有组件实例对象的原型对象的原型对象就是 Vue 的原型对象

> 1. 所有组件对象都能看到 Vue 原型对象上的属性和方法
> 2. Vue.prototype.\$bus = new Vue(), 所有的组件对象都能看到$bus 这个属性对象

1. 一种组件间通信的方式，适用于**任意组件间通信**

2. 安装全局事件总线（main.js文件）

   ```javascript
   new Vue({
       beforeCreate(){	 // 尽量早的执行挂载全局事件总线对象的操作
       	Vue.prototype.$bus = this // 安装全局事件总线，$bus 就是当前应用的vm
   	},
   }).$mount("#app")
   ```

3. 指定事件总线对象

   - 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的**回调留在A组件自身**

     ```javascript
     methods(){
     	demo(data){...}
     }
     ......
     // 接收数据
     mounted(){
         // this.$bus.$on('xxxx', ()=>{}))
     	this.$bus.$on('xxxx', this.demo)
     }
     ```

   - 提供数据：

     - `this.$bus.$emit('xxxx',数据)`

4. 最好在beforeDestroy钩子中，用$off去解绑**当前组件所用到的**事件

绑定事件

```vue
this.$bus.$on('deleteTodo', this.deleteTodo)
```

分发事件

```vue
this.$bus.$emit('deleteTodo', this.index)
```

解绑事件

```vue
this.$bus.$off('deleteTodo')
```



# 消息订阅与发布（pubsub）

1. 一种组件间通信的方式，适用于**任意组件间通信**

2. 使用步骤

   1. 安装pubsub：`npm i pubsub-js`

   2. 引入：`import pubsub from 'pubsub-js'`

   3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的<span style="color:red"><strong>回调留在A组件本身</strong></span>

      ```javascript
      methods(){
          demo(data){......}
      }
      ......
      mounted(){
          this.pid = pubsub.subscribe('xxx', this.demo)	// 订阅消息
      }
      ```

   4. 提供数据：`pubsub.publish('xxx', 数据)`

   5. 最好在beforeDestory钩子中，用`PubSub.unsubscribe(pid)`去取消订阅

# nextTick

1. 语法：

   ```
   this.$nextTick(回调函数)
   ```
2. 作用：在下一次DOM更新结束后执行其指定的回调
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

# Vue封装的过渡与动画

1. 作用：在插入，更新或移除DOM元素时，在合适的时候给元素添加样式类名

2. 写法：

   - 元素进入的样式
     1. v-enter：进入的起点
     2. v-enter-active：进入过程中
     3. v-enter-to：进入的终点
   - 元素离开的样式
     1. v-leave：离开的起点
     2. v-leave-active：离开的过程中
     3. v-leave-to：离开的终点

3. 使用`<transition>`包裹要过渡的元素，并配置name属性：

   ```
   <transition name="hello">
   	<h1 v-show="isShow">你好</h1>
   </transition>
   ```

4. 备注：若有多个元素需要过渡，则需要使用：`<transition-group>`，且每个元素都要指定`key`值

# vue脚手架配置代理

- ws: true默认不写为 true
- changeOrigin: true 默认不写为 true
- 在React脚手架里会默认为false

## 配置的方法

### 方法一

- 在vue.config.js中添加如下配置

  ```javascript
  devServer:{
      proxy:"http://localhost:3000"
  }
  ```

  - 说明
    1. 优点：配置简单，请求资源时直接发给前端（8080）即可
    2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理
    3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器（优先匹配前端资源）

### 方法二

- 编写vue.config.js配置具体代理规则

  ```javascript
  module.exports = {
      pages:{
          index:{
              // 入口
              entry:'src/main.js',
          }
      }
      devServer: {
          proxy: {
              '/api1' :{	// 匹配所有以 '/api1' 开头的请求路径
               target: 'http://localhost:5000',	// 代理目标的基础路径
               changeOrigin: true,
               pathRewrite: {'^/apl1': ''}
              },
              '/api2' :{	// 匹配所有以 '/api2' 开头的请求路径
               target: 'http://localhost:5001',	// 代理目标的基础路径
               changeOrigin: true,
               pathRewrite: {'^/apl2': ''}
              },
          }
      }
  }
  /*
  	changeOrigin 设置为 true时，服务器收到的请求头中的host为：localhost:5000
  	changeOrigin 设置为 false时，服务器收到的请求头中的host为：localhost:8000
  	changeOrigin: true 默认不写为 true
  */
  ```
  
  - 说明
    1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理
    2. 缺点：配置略微繁琐，请求资源时必须加前缀

# vue-resource

> vue插件库，vue1.x使用广泛，官方已不维护

- vue-resource 是对 xhr的封装

- 导入后，所有的组件实例会有 $http属性

- 用法和axios相似

- 安装

  ```
  npm i vue-resource
  ```

- 使用

  ```
  import vueResource from 'vue-resource'
  Vue.use(vueResource)
  ```

# 插槽

> 插槽的数据会存储在$slots身上

1. 作用：让父组件可以向子组件指定位置插入 html 结构，也是一种组件间通信的方式，适用 **父组件 ==> 子组件**

2. 分类：默认插槽、具名插槽、作用域插槽

3. 使用方式

   1. 默认插槽

      ```html
      父组件中：
      	<组件名>
      		<div>html结构</div>
      	</组件名>
      子组件中：
      	<template>
          	<div>
                  <!-- 定义插槽 -->
                  <slot>插槽默认内容</slot>
              </div>    
      	</template>
      ```

   2. 具名插槽

      ```html
      父组件中：
      	<组件名>
      		<template slot="center">
                  <div>html结构</div>
              </template>
              
              <template v-slot:footer>
                  <div>html结构</div>
              </template>
      	</组件名>
      子组件中：
      	<template>
              <div>
              	<!-- 定义插槽 -->
                  <slot name="center">插槽默认内容1</slot>
                   <slot name="footer">插槽默认内容2</slot>
              </div>
      	</template>
      ```

   3. 作用域插槽

      1. 理解：**数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定**（数据在子组件中，但使用数据所遍历出来的结构由App组件决定）

      2. 具体编码

         ```html
         父组件中
         	<组件名>
                 <template scope="scopeData">
         			<!-- 生成的是ul列表-->
                     <ul>
                         <li v-for="g in scopeData.games" :key="g">{{g}}</li>
                     </ul>
                 </template>
              </组件名>
         
         	<组件名>
                 <!-- scope自Vue2.5起已弃用，推荐使用 slot-scope-->
                 <template scope="scopeData">
         			<!-- 生成的是h4标题-->
                     <ul>
                         <h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
                     </ul>
                 </template>
              </组件名>
         子组件中：
         	<template>
                 <div>
                     <slot :games="games"></slot>
                 </div>
             </template>
         
         	<script>
         		export default{
                     name:'组件名',
                     // 数据在子组件自身
                     data(){
                         return{
                             games:['红色警戒','cs','超级玛丽']
                         }
                     }
                 }
         	</script>
         ```

- 

# Vue UI组件库

## 移动端常用UI组件库

1. Vant	https://youzan.github.io/vant
2. Cube UI	https://didi.github.io/cube-ui
3. Mint UI	http://mint-ui.github.io

## PC端常用UI组件库

1. Element UI	https://element.eleme.cn
2. IView UI	https://www.iviewui.com

   
