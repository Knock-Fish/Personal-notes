# vue-router

> vue的一个插件库，专门用来实现SPA应用

对SPA应用的理解

> 1. 单页 Web 应用（single page web application, SPA）
> 2. 整个应用只有一个完整的页面
> 3. 点击页面中的导航链接不会刷新页面，只会做页面的局部更新
> 4. 数据需要通过ajax请求获取

路由的理解

> 什么是路由
>
> 1. 一个路由就是一组映射关系（key - value）
> 2. key为路径，value可能是function或component
> 3. 多个路由，需要经过路由器的管理
>
> 路由分类
>
> - 后端路由
>   1. 理解：value是function，用于处理客户端提交的请求
>   2. 工作过程：服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据
>
> - 前端路由
>   1. 理解：value是component，用于展示页面内容
>   2. 工作过程：当浏览器的路径改变时，对应的组件就会显示

# 基本使用

安装vue-router，命令：`npm i vue-router`（默认安装4x版本）

> - Vue2 安装 vue-router3x
> - Vue3 安装 vue-router4x

应用插件：`Vue.ues(VueRouter)`

编写router配置项

```javascript
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入 Luyou组件
import About from '../components/About'
import Home from '../components/Home'

// 创建router实例对象，去管理一组一组的路由规则
const router =  new VueRouter({
    routes:[
        {
            path:'/about',
            component:MyAbout
        },
        {
            path:"/home",
            component:MyHome
        }
    ]
})

// 暴露router
export default router
```

实现切换（active-class可配置高亮样式）

```html
<router-link active-class="active" to="/about"></router-link>
```

指定展示位置

```html
<router-view></router-view>
```

注意

> 1. 路由组件通常存放在pages文件夹，一般组件通常存放在components文件夹
>
> 2. 通过切换，“隐藏”了的路由组件，默认时被销毁的，需要的时候再去挂载
>
> 3. 每个组件都有自己的`$route`属性，里面存储着自己的路由信息
>
> 4. 整个应用只有一个router，可以通过组件的`$router`属性获取到
>
> 5. 注册完路由，不管路由或路由组件、还是非路由组件身上都有\$route 、 $router属性
>
>    - $route：一般获取路由信息【路径、query、params等等】
>
>    - $router：一般进行编程式导航进行路由跳转【push | replace】
>
> 6. 声明式导航：router-link（要有to属性），可以实现路由的跳转
>
> 7. 编程式导航：利用的是组件实例的$router.push | replace方法，可以实现路由的跳转

相关 API

> this.$router.push(path): 相当于点击路由链接(可以返回到当前路由界面)
>
> this.$router.replace(path): 用新路由替换当前路由(不可以返回到当前路由界面)
>
> this.$router.back(): 请求(返回)上一个记录路由
>
> this.$router.go(-1): 请求(返回)上一个记录路由
>
> this.$router.go(1): 请求下一个记录路由

# 多级路由（嵌套）

配置路由规则，使用children配置项

```javascript
routes:[
    {
        path:'/about',
        component:About
    },
    {
        path:"/home",
        component:Home,
        children:[					// 通过children配置子级路由
            {
                path:'news',		// 此处一定不要加 " / "
                component:News
            },
            {
                path:'message',		//  此处一定不要加 " / "
                component:Message
            }
        ]
    }
]
```

跳转（要写完整路径）

```html
<router-link to="/home/news">News</router-lin>
```

# 路由的query参数

query参数：属于路径当中的一部分，类似于ajax中的`queryString`，不需要占位

传递参数

```javascript
<!-- 跳转路由并携带query参数，to的字符串写法 -->
<!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link> -->
<!-- 跳转路由并携带query参数，to的对象写法 -->
<router-link :to="{
path:'/home/message/detail',
    query:{
        id:m.id,
        title:m.title
    }
}">
{{m.title}}
</router-link>
```

接收参数

```
$route.query.id
$route.query.title
```

# 命名路由

> 作用：可以简化路由的跳转

给路由命名

```javascript
routes: [
    {
        path: "/home",
        component: Home,
        children: [
            {
                name:'news',	// 给路由命名
                path: 'news',
                component: News
            }
        ]
    }
]
```

简化跳转

```html
<!-- 简化前，需要写完整的路径-->
<router-link :to="/home/message/detail"></router-link>
<!-- 简化后，直接通过名字跳转-->
<router-link :to="{name:'detail'}"></router-link>

<!-- 简化写法配合传递参数-->
<router-link :to="{
    name:'detail',
    query:{
     	id:001,
        title:'张三'
     }
}">
</router-link>
```

# 路由的params参数

params参数：不属于属于路径当中的一部分，在配置路由的时候，需要占位

配置路由，声明接收params参数

```javascript
 routes: [
     {
         path: "/home",
         component: Home,
         children: [
             {
                 path: 'message',
                 component: Message,
                 children: [
                     {
                         name:'detail',
                         path: 'detail/:id/:title',	// 使用占位符声明接收params参数
                         component: Detail
                     }
                 ]
             }
         ]
     }
})
```

传递参数

```html
 <!-- 跳转路由并携带params参数，to的字符串写法 -->
<router-link :to="`/home/message/detail/666/你好`"></router-link>

<!-- 跳转路由并携带params参数，to的对象写法 -->
<router-link :to="{
	name:'detail',	// 这里必须使用name
    params:{
       id:666,
       title:'你好'
    }
}">
</router-link>
```

注意
> 路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置

接收参数

```js
$route.params.id
$route.params.title
```

# 路由的props配置

作用：让路由组件更方便的收到参数

```javascript
 {
     name:'detail',
         path: 'detail/:id/:title',
             component: Detail, 
                 // props的第一种写法，值为对象，该对象中的所有key-value都会以props的形式传给Datail组件
                 props:{a:1,b:'hello'}    // 此方法数据只能写死
                 
      // props的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件
                 props:true

                 // props的第三种写法，值为函数
                 props($route){
                      return {
                       id:$route.query.id,
                       title:$route.query.title
                       }
                  }
     
                 // 解构赋值写法
                 props({query:{id,title}}){
                 return {id,title}
             }
 }
```


# \<router-link>的replace属性

作用：控制路由跳转时操作浏览器历史记录

浏览器的历史记录有两种写入方式：分别为`push`和`replace`，`push`是追加历史记录，`replace`是替换当前记录。路由跳转时候默认为`push`

> replace：不管当前栈顶是谁，直接干掉

如何开启`replace`模式

```html
<!-- 简写形式 -->
<router-link replace ......></router-link>
<!-- 完整写法 -->
<router-link :replace="true" ......></router-link>
```

# 编程式路由导航

> 路由跳转的两种形式：编程式导航、声明式导航

作用：不借助`<router-link>`实现路由跳转，让路由更加灵活

```javascript
 methods:{
     pushShow(m){
         this.$router.push({
             name:'detail',
             query:{
                 id:xxx,
                 title:xxx
             }
         })
     },
     replaceShow(m){
         this.$router.replace({
             name:'detail',
             query:{
                 id:xxx,
                 title:xxx
             }
         })
     }
 }
```

# $router上的前进和后退

```javascript
$router.back()		// 前进
$router.forward()	// 后退
$router.go(2)		// // go的参数：正数为前进，负数为后退，0为刷新页面
```

# 缓存路由组件

> 作用：让不展示的路由组件保持挂载，不被销毁

`include`缓存的是组件，参数只能是组件名

`include`写多个组件的方式

> 1. 直接用逗号分隔
>
> 2. 用v-bind绑定include，然后写成数组形式

```html
<keep-alive :include="['News','message']">
    <router-view></router-view>
</keep-alive>
```

使用

```html
<!-- <keep-alive>里面不写include默认为全部保持挂载-->
<keep-alive include="News">
    <router-view></router-view>
</keep-alive>
```

# 路由组件的生命周期钩子

> 作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态

具体名字
- `activated`路由组件被激活时触发（组件展示时）
- `deactivated`路由组件失活时触发（组件不展示时）

# 路由守卫

> 作用：对路由进行权限控制
>
> 分类：全局守卫、独享守卫、组件内守卫

## 全局路由守卫

```javascript
// 全局前置路由守卫——初始化的时候被调用、每次路由切换之前被调用
router.beforeEach((to,from,next)=>{
    console.log('前置路由守卫',to,from)
    if(to.meta.isAuth){
        if(localStorage.getItem('id') === '345'){
            next()
        }else{
            alert('id不对，无权限查看！')
        }
    }else{
        next()
    }
})


// 全局后置路由守卫——初始化的时候被调用、每次路由切换之后
router.afterEach((to,from)=>{
    console.log('后置路由守卫',to,from)
    // 修改网页的title
    document.title = to.meta.title || 'Vue路由组件'
})

```

## 独享路由守卫

> 为某一个路由所独享的

```javascript
 beforeEnter:(to,from,next) => {
     console.log('前置路由守卫',to,from)
     if(to.meta.isAuth){	// 判断当前路由是否需要进行权限控制
         if(localStorage.getItem('id') === '345'){
             next()
         }else{
             alert('id不对，无权限查看！')
         }
     }else{
         next()
     }
 }
```

## 组件内守卫

```javascript
// 进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter(to,from,next){
    
},
// 离开守卫，通过路由规则，离开该组件时被调用
beforeRouteLeave(to,from,next){
    
},
```


# 路由器的两种工作模式

1. 对于一个url来说，什么是hash值？——#及其后面的内容就是hash值
2. hash值不会包含在HTTP请求中，即：hash值不会带给服务器
3. 两种模式
   - hash模式
     1. 地址中永远带着#号，不美观
     2. 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法
     3. 兼容性较好
   - history模式
     1. 地址干净，美观
     2. 兼容性和hash模式相比略差
     3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题
        - history模式路由的选择路径会被一并带给服务器

# 路由元信息

- 定义路由的时候可以配置 meta 字段