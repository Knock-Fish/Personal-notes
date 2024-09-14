# BOM概述

1. BOM（Browser Object Model）即**浏览器对象模型**，它提供了独立于内容而于**浏览器窗口进行交互的对象**，其核心对象是window
2. BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性
3. BOM缺乏标准，JavaScript语法的标准化组织是ECMA，DOM的标准化组织是W3C，BOM最初是Netscape浏览器标准的一部分

## BOM

1. 浏览器对象模型
2. 把浏览器当成一个对象来看待
3. BOM的顶级对象是window
4. BOM学习的是浏览器窗口交互的一些对象
5. BOM是浏览器厂商在各自浏览器上定义的，兼容性较差

## BOM构成

1. window对象是浏览器的顶级对象，它具有双重角色
2. 它是JS访问浏览器窗口的一个接口
3. 它是一个全局对象。定义在全局作用域中的变量、函数会变成window对象的属性和方法。在调用的时候可以省略window
4. **window下的一个特殊属性** `window.name`（命名注意）![](http://127.0.0.1:8888/uploads/202303021407138.png)

------

# window对象的常见事件

## 窗口加载事件

`window.onload`是窗口（页面）加载事件，当内容完全加载完成会触发该事件（包括图像、脚本文件、CSS文件等）

- 注意

  1. 有了`window.onload`就可以把JS代码写到页面元素的上方，`onload`是等页面内容全部加载完毕，再去执行处理函数

  2. `window.onload`传统注册方式只能写一次，如果有多个，会以最后一个`window.onload`为准

  3. 如果使用`addEventListener`则没有限制

     ```javascript
     window.onload = function()
     或者
     window.addEventListener("load",function(){});
     ```

     ![](http://127.0.0.1:8888/uploads/202303021417587.gif)


## DOM加载事件

- `DOMContentLoadad`事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等（ie9以上支持）

  ```javascript
  document.addEventListener('DOMContentLoaded',function(){})
  ```

  - 用处
    - 如果页面图片很多的话，从用户访问到onload触发可能需要较长的时间，交互效果就不能实现，必然影响用户的体验，此时用`DOMContentLoad`事件比较合适

## 调整窗口大小事件

`window.onresize`是调整窗口大小加载事件，当触发时就调用的处理函数

```javascript
// 传统注册事件
window.onresize = function(){}

// addEventListener注册事件
window.addEventListener("resize",function(){});
```

1. 主要窗口大小发生像素变化，就会触发这个事件

2. 可以利用这个事件完成响应式布局。`window.innerWidth`（当前窗口宽度）和`window.Height`（当前窗口高度）![](http://127.0.0.1:8888/uploads/202303021429480.gif)

------

# 定时器

>  window对象提供了2种定时器方法

## 设置定时器

### setTimeout()定时器

`setTimeout()`方法用于设置一个定时器，该定时器在定时器到期后执行调用函数

```javascript
window.setTimeout(调用函数，[延迟的毫秒数]);
```

- 延时时间单位是毫秒，如果省略默认是0
- 页面如有多个定时器，可以给定时器加标识符（名字）![](http://127.0.0.1:8888/uploads/202303021454023.gif)

### setInterval()定时器

`setInterval()`方法**重复调用**一个函数，每隔这个时间，就调用一次回调函数

```javascript
window.setTimeout(调用函数,[延迟的豪秒数]);
```

![](http://127.0.0.1:8888/uploads/202303021458067.gif)

### 两种定时器方法注意

1. window可以省略
2. 调用函数**可以直接写函数**，**或者写函数名**或者采取字符串**'函数名()'**三种形式第三种不推荐
3. 延迟的毫秒数省略默认是0，如果写，必须是毫秒（1s = 1000ms）
4. 如果定时器有很多，可以给定时器赋值一个标识符

## 停止定时器

> 注意
>
> 1. window可以省略
> 2. 停止定时器里面的参数就是定时器的标识符

### 停止setTimeout()定时器

`clearTimeout()`方法取消了先前通过调用`setTimeout()`建立的定时器

```javascript
window.clearTimeout(timeoutID)
```

![](http://127.0.0.1:8888/uploads/202303021515529.gif)

### 停止setInterval()定时器

`clearInterval()`方法取消先前通过调用`setInterval()`建立的定时器

```javascript
window.clearInterval(intervalID);
```

![](http://127.0.0.1:8888/uploads/202303021519450.gif)

# JS执行机制

## JS是单线程

1. JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。JavaScript是为处理页面中用户的交互，以及操作DOM而诞生的。比如对某个DOM元素进行添加和删除操作，不能同时进行。应该先进行添加，之后再删除
2. 单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是∶如果JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

## 同步和异步

1. 为了解决这个问题，利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程。于是，JS中出现了**同步**和**异步**

  1. 同步：前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的
  2. 异步：同一时间处理多个任务
2. 同步任务：同步任务都在主线上执行，形成一个**执行栈**
3. 异步任务
  - JS的异步是通过回调函数实现的
    - 一般而言，异步任务有以下三种类型
      1. 普通事件，如click、resize等
      2. 资源加载，如load、error等
      3. 定时器，包括setInterval、setTimeout等
4. 执行
  1. 先执行**执行栈中的同步任务**
  2. 异步任务（回调函数）放入任务队列中
  3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取**任务队列**中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。
5. 机制
  - 由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为**事件循环（ event loop )**![](http://127.0.0.1:8888/uploads/202303021841393.png)

------

# location对象

window对象给我们提供了一个location属性用于获取或设置窗体的URL，并且可以用于解析URL。因为这个属性返回的是一个对象，所以我们将这个属性也称为location对象

## URL说明

> **统一资源定位符(Uniform Resource Locator, URL)**是互联网上标准资源的地址。互联网上的每个文件都有—个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它

URL的一般语法格式为

```
protocol://host[:port]/path/[?query]#fragment
对应
http://www.baidu.com/index.html?name=andy&age=18#link
```

- 说明

  | 组成     | 说明                                                         |
  | -------- | ------------------------------------------------------------ |
  | protocol | 通信协议 常用的http，ftp，maito等                            |
  | host     | 主机（域名） www.baidu.com                                   |
  | port     | 端口号 可选，省略时使用方案的默认端口 如http的默认端口为80   |
  | path     | 路径 由 零或多个 ' / ' 符号隔开的字符串，一般用来表示主机上的一个目录或文件地址 |
  | query    | 参数  以键值对的形式，通过 & 符号分隔开来                    |
  | fragment | 片段 #后面内容 常见于链接 锚点                               |

## location对象的属性

| location对象属性  | 返回值                             |
| ----------------- | ---------------------------------- |
| location.href     | 获取或者设置 整个URL               |
| location.host     | 返回主机（域名）www.baidu.com      |
| location.port     | 返回端口号 如果未写返回 空字符串   |
| location.pathname | 返回路径                           |
| location.search   | 返回参数                           |
| location.hash     | 返回片段 #后面内容 常见于链接 锚点 |

| location对象方法   | 返回值                                                       |
| ------------------ | ------------------------------------------------------------ |
| location.assign()  | 跟href一样，可以跳转页面（也称为重定向页面）                 |
| location.replace() | 替换当前页面，因为不记录历史，所以不能后退页面               |
| location.reload()  | 重新加载页面，相当于 刷新按钮 或者 f5 如果参数为true强制刷新 ctrl + f5 |

- 获取URL

  ![](http://127.0.0.1:8888/uploads/202303022157177.gif)

# history对象

> window对象给我们提供了一个history对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中)访问过的URL

| history对象方法 | 作用                                                     |
| --------------- | -------------------------------------------------------- |
| back()          | 可以后退功能                                             |
| forward()       | 前进功能                                                 |
| go(参数)        | 前进后退功能 参数如果是1前进1个页面，如果是-1后退1个页面 |

# 元素偏移量offset系列

## offset概述

offset（偏移量），使用offset系列相关属性可以**动态的**得到该元素的位置（偏移）、大小等
1. 获取元素距离带有定位父元素的位置
2. 获得元素自身的大小（宽度和高度）
3. **返回的数值都不带单位**

## offset系列常用属性

| offset系列属性       | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.offsetParent | 返回作为该元素带有定位的父级元素，如果父级都没有定位则返回body |
| element.offsetTop    | 返回元素相对带有定位父元素上方的偏移                         |
| element.offsetLeft   | 返回元素相对带有定位父元素左边框的偏移                       |
| element.offsetWidth  | 返回子身包括padding、边框、内容区的宽度、返回数值不带单位    |
| element.offsetHeight | 返回自身包括padding、边框、内容区的高度，返回数值不带单位    |

# 元素可视区client系列

> client（客户端）：使用client系列的相关属性来获取元素可视区的相关信息。通过client系列的相关属性可以动态的得到该元素的边框大小，元素大小等

## client系列属性

| client系列属性       | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.clientTop    | 返回元素上边框的大小                                         |
| element.clientLeft   | 返回元素左边框的大小                                         |
| element.clientWidth  | 返回自身包括padding、内容区的宽度，不含边框，返回数值不带单位 |
| element.clientHeight | 返回自身包括padding、内容区的高度，不含边框，返回数值不带单位 |

# 元素滚动scroll系列

> scroll（滚动的）：使用scroll系列的相关属性可以动态的得到该元素的大小、滚动距离等

## scroll系列属性

| scroll系列属性       | 作用                                           |
| -------------------- | ---------------------------------------------- |
| element.scrollTop    | 返回被卷去的上侧距离，返回数值不带单位         |
| element.scrollLeft   | 返回被卷去的左侧距离，返回数值不带单位         |
| element.scrollWidth  | 返回自身实际的宽度，不含边框，返回数值不带单位 |
| element.scrollHeight | 返回自身实际的高度，不含边框，返回数值不带单位 |

## 页面卷去的头部

- 元素卷去的头部是`element.scrollTop`，左侧则是`element.scrollLeft`

- 页面卷去的头部：可以通过`window.pageYOffset`获取，左侧则是`window.pageXOffset`

- 页面被卷去的头部，有兼容性问题，因此被卷去的头部通常有如下几种写法：
  1. 声明了DTD，使用document.documentElement.scrollTop
  2. 未声明DTD，使用document.body.scrollTop
  3. 新方法 window.pageYOffset 和 window.pageXOffset，IE9开始支持

# 动画

- 动画原理
  1. 获得盒子当前位置
  2. 让盒子在当前位置加上1个移动距离
  3. 利用定时器不断重复这个操作
  4. 加一个结束定时器的条件
  5. 注意此元素需要添加定位，才能使用element.style.left

## 动画函数封装-给不同元素记录不同定时器

封装函数

```javascript
// 均速动画：盒子当前位置 + 固定的值
// 给不同的元素指定不同的定时器
// obj目标对象 target目标位置
function animate(obj,target) {
    // 清除已开启的定时器
    clearInterval(obj.timer);
    obj.timer = setInterval(function () {
        if (obj.offsetLeft == target) {
            // 到目标位置停止定时器
            clearInterval(obj.timer);
        }
        obj.style.left = obj.offsetLeft + 10 + 'px';	// 10 为步长
    },30);
}
```

## 缓动动画

- 效果原理

  - 缓动动画就是让元素运动速度有所变化，最常见的是让速度慢慢停下来

  - 思路：

    1. 让盒子每次移动的距离慢慢变小，速度就会慢慢落下来

    2. 核心算法：**（目标值 - 现在的位置 ）/ 10** 做为每次移动的距离步长

    3. 停止的条件是：让当前盒子位置等于目标位置就停止定时器

       ```javascript
       // 缓动动画：(盒子当前的位置 + 变化的值) / 10
       // 给不同的元素指定不同的定时器
       // obj目标对象 target目标位置
       function animate(obj,target) {
           // 清除已开启的定时器
           clearInterval(obj.timer);
           obj.timer = setInterval(function () {
               // 设置步长	可实现多个目标值之间移动
               let step = (target - obj.offsetLeft) / 10;
               // step 为正值向上取整，step 为负值向下取整
               step = step > 0 ? Math.ceil(step) : Math.floor(step);
               if (obj.offsetLeft == target) {
                   // 到目标位置停止定时器
                   clearInterval(obj.timer);
               }
               obj.style.left = obj.offsetLeft + step + 'px';
               console.log(obj.style.left)
           },30);	// 30毫秒
       }
       ```

## 动画添加回调函数

> **回调函数原理**：函数可以作为一个参数，将这个函数作为参数传到另一个函数里面，当那个函数执行完之后，再执行传传进去的这个函数，这个过程就叫做**回调**

```javascript
// 给不同的元素指定不同的定时器
// obj目标对象 target目标位置
function animate(obj,target,callback) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function () {
        // 设置步长
        let step = (target - obj.offsetLeft) / 10;
        step = step > 0 ? Math.ceil(step) : Math.floor(step);
        if (obj.offsetLeft == target) {
            clearInterval(obj.timer);
        //  if(callback){
        //      callback()
        //  }
            callback && callback()	// 高级写法：等价于上面 if语句
        }
        obj.style.left = obj.offsetLeft + step + 'px';
        console.log(obj.style.left)
    },30);
}
let div = document.querySelector('div');
let btn200 = document.querySelector('.btn200');
btn200.addEventListener('click',function(){
    animate(div,200,function(){
        div.style.backgroundColor = 'red';
    });
})
```

![](http://127.0.0.1:8888/uploads/202303031547885.gif)

## 节流阀

防止轮播图按钮连续点击造成播放过快

节流阀目的：当上一个函数动画内容执行完毕，再取执行下一个函数动画，让事件无法连续触发

核心实现思路：利用回调函数，添加一个变量来控制，锁住和解锁函数
1. 开始设置一个变量 let flag = true;
2. if(flag){flag;do somehing}
   - 关闭水龙头
3. 利用回调函数动画执行完毕，flag = true 打开水龙头

# 本地存储

本地存储特性
1. 数据存储在用户浏览器中
2. 设置、读取方便、页面刷新不丢失数据
3. 容量较大，sessionStorage约5M、localStorage约20M
4. 只能存储字符串，可以将对象JSON.stringify()编码后存储

## window.sessionStorage

1. 只要不关闭浏览器窗口，数据一直存在

2. 在同一个窗口（页面）下数据可以共享

3. 以键值对的形式存储使用

   | 属性               | 作用                                       |
   | ------------------ | ------------------------------------------ |
   | setItem(key,value) | 如果键名存在，则更新其对应的值（存储数据） |
   | getItem(key)       | 返回键名对应的值（获取数据）               |
   | removeIten(key)    | 删除键名对应的值（删除数据）               |
   | clear()            | 清空所以存内容（储删除所有数据）           |
   | key(n)             | 返回sessionStorage对象中第n个key的名称     |
   | length             | 返回sessionStorage对象中包含的item的数量   |

   ![](http://127.0.0.1:8888/uploads/202303052130840.gif)

## window.localStorage

1. 数据除非手动删除，否则手动关闭页面数据也会存在

2. 可以多窗口（页面）共享（同一浏览器可以共享）

3. 以键值对的形式存储使用

   | 属性               | 作用                                       |
   | ------------------ | ------------------------------------------ |
   | setItem(key,value) | 如果键名存在，则更新其对应的值（存储数据） |
   | getItem(key)       | 返回键名对应的值（获取数据）               |
   | removeIten(key)    | 删除键名对应的值（删除数据）               |
   | clear()            | 清空所以存内容（储删除所有数据）           |
   | key(n)             | 返回localStorage对象中第n个key的名称       |
   | length             | 返回localStorage对象中包含的item的数量     |

![](http://127.0.0.1:8888/uploads/202303052138771.gif)

## Web Strorage事件监听

1. 当使用Web Strorage存储的数据发生变化时，会触发Window对象的storage事件

2. 监听该事件并指定事件处理函数，当其他页面中的localStorage或sessionStorage 中保存的数据发生改变时，就会执行事件处理函数

   ```javascript
   window.addEventListener(事件名,事件处理函数);
   ```
   
   - 监听storage事件

     ```javascript
     window.addEventListener('storage',function(event){
        console.log(event.key); 
     });
     ```
   
3. event对象的属性

   | 属性              | 描述                                                         |
   | ----------------- | ------------------------------------------------------------ |
   | event.key         | 获取在 sessionStorage 或 localStorage 中被修改的数据键名     |
   | event.oldValue    | 获取在 sessionStorage 或 localStorage 中被修改前的值         |
   | event.newValue    | 获取在 sessionStorage 或 localStorage 中被修改后的值         |
   | event.url         | 获取在 sessionStorage 或 localStorage 中发生修改的页面URL地址 |
   | event.storageArea | 获取变动的 sessionStorage对象 或 localStorage对象            |
