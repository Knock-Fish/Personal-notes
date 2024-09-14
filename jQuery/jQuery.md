# jQuery概述

- ### JavaScript库

  - 即library，是一个**封装**好的特定的**集合**（方法和函数）。从封装函数的角度理解库，就是在这个库中，封装了很多预定定义好的函数在里面，比如动画animate、hide、show，比如获取元素等
    - 简单理解：就是一个JS文件，里面对原生js代码进行了封装，存放到里面。这样可以快速高效的使用这些封装好的功能了
    - 比如jQuery，就是为了快速方便的操作DOM，里面基本都是函数（方法）
  - 常见的JavaScript库
    1. jQuery
    2. Prototype
    3. YUI
    4. Dojo
    5. Ext JS
    6. 移动端的zepto

  - 这些库都是对原生JavaScript的封装，**内部都是用JavaScript实现的**

- ### jQurey的下载

  - 网址：https://jQurey.com

------

# jQurey的基本使用

## 入口函数

1. 等着DOM结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完成，jQuery已经完成了封装

2. 相当于原生js种的DOMContentLoaded

3. 不同原生js种的load事件是等页面文档、外部的js文件、css文件、图片加载完毕才执行内部代码

4. 语法

   ```javascript
   // 第一种
   $(function(){
   	... // 页面DOM加载完成的入口
   })
   
   // 第二种
   $(document).ready(function(){
   	... // 页面DOM加载完成的入口
   })
   ```

## 顶级对象 $ 

1. \$ 是 jQuery的别称，在代码中可以使用jQuery代替\$，但一般为了方便，通常都直接使用\$

2. \$是jQuery的顶级对象，相当于原生JavaScript中的window。把元素利用\$包装成jQuery对象，就可以调用jQuery的方法

3. 代码

   ```javascript
   $('div');	//	直接获取对象
   let Div = $('div');		// 可以定义变量赋值
   console.dir($('div'))
   console.dir(Div);
   ```

## jQuery对象和DOM对象

1. 用原生JS获取来的对象就是DOM对象

   ```javascript
   let Div = document.querySelector('div');	// Div 是DOM对象
   ```

2. jQuery方法获取的元素就是jQuery对象

   ```javascript
   $('div');	// $('div')是一个jQuery对象
   ```

3. jQuery对象本质是：利用\$对DOM对象包装后产生的对象（伪数组形式存储）

4. jQuery对象只能使用jQuery方法，DOM对象则使用原生的JavaScript属性和方法

### jQuery对象和DOM对象的相互转换

- 原生js比jQuery更大，原生的一些属性和方法jQuery没有封装，要想使用这些属性和方法需要把jQuery对象转换为DOM对象才能使用

  1. DOM对象转换为jQuery对象：**$(DOM对象)**

     ```javascript
     let Div = document.querySelector('div');	// 获取DOM对象
     $(Div);		// 放在$()括号里即转换为jQuery对象
     ```

  2. jQuery对象转换为DOM对象（两种方式）

     - ```javascript
       $('jQuery对象')[index]	// index 是索引号
       ```

     - ```javascript
       $('jQuery对象').get[index]	// index 是索引号
       ```

------

# 基础选择器

- jQuery给选择器做了封装，使获取元素统一标准

  - 语法

    ```javascript
    $("选择器")	// 里面选择器直接写CSS选择器即可，但是要加引号
    ```

  - 基本选择器

    | 名称       | 用法                       | 描述                     |
    | ---------- | -------------------------- | ------------------------ |
    | ID 选择器  | $(" #id ")                 | 获取指定ID的元素         |
    | 全选选择器 | $(" * ")                   | 匹配所有元素             |
    | 类选择器   | $(" .class ")              | 获取同一类class的元素    |
    | 标签选择器 | $(" 标签名 ")              | 获取同一类标签的所有元素 |
    | 并集选择器 | $(" 元素1 , 元素2 , ... ") | 选取多个元素             |
    | 交集选择器 | $(" 元素1.元素2 ")         | 交集元素                 |

  - 层级选择器

    | 名称       | 用法               | 描述                                             |
    | ---------- | ------------------ | ------------------------------------------------ |
    | 子代选择器 | $(" 元素1>元素2 ") | 使用 > 号，获取最近子级的层级元素                |
    | 后代选择器 | $(" 元素1 元素2 ") | 使用空格，代表后代选择器，获取该元素下所有子元素 |

------

# 隐式迭代

1. 遍历内部DOM元素（伪数组形式存储）的过程就叫做**隐式迭代**
2. 隐式迭代把匹配到的所有元素进行循环遍历，执行相应的方法
3. 代码![](http://127.0.0.1:8888/uploads/202303061306577.png)

> :checked选择器，选择所有被选中的复选框

------

# 筛选选择器

- 选择器

  | 语法       | 用法                | 描述                                                     |
  | ---------- | ------------------- | -------------------------------------------------------- |
  | :first     | $("元素:first")     | 获取第一个元素                                           |
  | :last      | $("元素:last")      | 获取最后一个元素                                         |
  | :eq(index) | $("元素:eq(index)") | 获取到的元素中，选择指定索引号的元素，索引号index从0开始 |
  | :odd       | $("元素:odd")       | 获取到的元素中，选择索引号为奇数的元素                   |
  | :even      | $("元素:even")      | 获取到的元素中，选择索引号为偶数的元素                   |

------

# 筛选方法

- 方法

  | 语法                  | 用法                           | 说明                                                   |
  | --------------------- | ------------------------------ | ------------------------------------------------------ |
  | parent( )             | $("元素").parent();            | 查找最近的父级*                                        |
  | childtern( selector ) | $("父元素").children("子元素") | 相当于子级选择器，获取最近子级*                        |
  | find( selector )      | $("父元素").find("子元素")     | 相当于后代选择器，获取该元素下所有指定的子元素*        |
  | siblings( selector )  | $("元素").siblings(" 元素 ")   | 查找所有兄弟节点，不包括自己本身*                      |
  | nextAll([ expr ])     | $("元素").nextAll()            | 查找当前元素之后所有的同辈元素                         |
  | prevAll([ expr ])     | $("元素").prevAll()            | 查找当前元素之前所有的同辈元素                         |
  | hasClass( class )     | $('元素').hasClass("类名")     | 检查当前的元素是否含有某个特定的类，如果有，则返回true |
  | eq( index )           | $("li").eq(index)              | 相当于$("li:eq(index)")，index从0开始*                 |

------

# 链式编程

- 链式编程是为了节省代码量，看起来更简洁

  ```javascript
  $(this).css('color','red').sibling().css('color','')
  ```

  - 使用链式编程一定注意是哪个对象执行样式![](http://127.0.0.1:8888/uploads/202303062233568.gif)

------

# 操作样式

- jQuery可以使用css方法来修改简单元素样式；也可以操作类，修改多个样式

  1. 参数这写属性名，则是返回属性值

     ```javascript
     $(this).css("color");
     ```

  2. 参数是**属性名 , 属性值，逗号分隔** , 是设置一组样式，属性必须加引号，值如果是数子可以不用跟单位和引号

     ```javascript
     $(this).css("color","red");
     ```

  3. 参数可以是对象形式，方便设置多组样式。属性名和属性值用冒号隔开，属性可以不用加引号

     ```javascript
     $(this).css({"color":"red","font-size":"20px"})
     ```

------

# 类操作

- 作用等同于以前的classList，可以操作类样式，注意操作类里面的参数不要加 “ . ”

  1. 添加类

     ```javascript
     $("元素").addClass("类名");
     ```

  2. 移除类

     ```javascript
     $("元素").removeClass("类名");
     ```

  3. 切换类

     ```javascript
     $("元素").toggleClass("类名");
     ```

## 类操作与className区别

1. 原生JS中className会覆盖元素原先类名的类名

2. jQuery里面类操作只是对指定类进行操作，不影响原先的类名

   ```html
   <style>
       .one{
           width: 100px;
           height: 100px;
           background-color: red;
       }
       .two{
           width: 200px;
           height: 200px;
       }
   </style>
   <body>
       <div class="one"></div>
   
       <script>
           // jQuery
           $(".one").click(function(){
               $(this).addClass("two")		// addClass 不会覆盖原先的类名
           })
   
           // 原生JavaScript
           let div = document.querySelector('.one');
           div.addEventListener('click',function(){
               this.className = "two";		// className 会覆盖原先的类名
           })
       </script>
   </body>
   ```


------

# 效果

效果的主要参数

1. 参数都可以省略，无动画直接显示或隐藏
2. speed：三种预定速度之一的字符串（“ slow ”，“normal（默认值）”，“ fast ”）或表示动画时长的毫秒数值
3. easing：（Optional）用来指定切换效果，默认是“ swing ”，可用参数 “ linear ”
4. fn：回调函数，在动画完成时执行的函数，每个元素执行一次

## 显示隐藏效果

1. 显示语法规范

   ```javascript
   show([speed],[easing],[fn])
   ```

2. 隐藏语法规范

   ```javascript
   hide([speed],[easing],[fn])
   ```

3. 切换语法规范

   ```javascript
   toggle([speed],[easing],[fn])
   ```

## 滑动效果

1. 下滑动语法规范

   ```
   slideDown([speed],[easing],[fn])
   ```

2. 上滑动语法规范

   ```
   slideup([speed],[easing],[fn])
   ```

3. 滑动切换

   ```
   slideToggle([speed],[easing],[fn])
   ```

## 淡入谈出

1. 谈入效果语法规范

   ```
   fadeIn([speed,[easing],[fn]])
   ```

2. 谈出效果语法规范

   ```
   fadeOut([speed,[easing],[fn]])
   ```

3. 淡入谈出切换效果语法规范

   ```
   fadeToggle([speed,[easing],[fn]])
   ```

4. 渐进方式调整到指定的不透明度

   ```
   fadeTo([[speed],opcity,[easing],[fn]])
   ```

   - 主要参数：opacity透明度必须写，取值0~1之间，speed必须写

## 事件切换

- 语法

  ```
  hover([over,]out)
  ```

  - over：鼠标移到元素上要触发的函数（相对于mouseenter）
  - out：鼠标移出元素触发的函数（相当于mouseleave）
  
  - ![](http://127.0.0.1:8888/uploads/202303081015641.gif)

## 动画队列及其停止排队方法

1. 动画或效果队列

   - 动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行

2. 停止排队

   - 语法

     ```
     stop()
     ```

     - stop()方法用于停止动画或效果
     - 注意：stop()写到动画或者效果的**前面，相对于停止结束上一次的动画**

## 自定义动画animate

1. 语法

   ```javascript
   animate(params,[speed],[easing],[fn])
   ```

   - 主要参数
     - **params：想要更改的样式属性，一对象形式传递，必须写。属性名可以不用带引号，如果是复合属性则需要采取驼峰命名法borderLef**t。其余参数可以省略

   - 代码![](http://127.0.0.1:8888/uploads/202303081115017.gif)

# 属性操作

- #### 设置或获取元素固有属性值prop()

  - 所谓元素固有属性就是元素本身自带的属性，比如\<a>元素里面的href，比如\<input>元素里面的type

  - 获取属性语法

    ```
    element.prop("属性名")		// 获取属性值
    ```

  - 设置属性语法

    ```
    element.prop("属性名","属性值")
    ```

- #### 设置或获取元素自定义属性值 attr()

  - 用户自己给元素添加的属性，称为自定义属性。比如给div添加 index = "1"

  - 获取属性语法

    ```javascript
    attr("属性")		//类似原生 getAttribute()
    ```

  - 设置属性语法

    ```javascript
    attr("属性","属性值")	// 类似原生 setAttribute()
    ```

- #### 数据缓存data()

  1. data()方法可以在指定的元素上存取数据，并不会修改DOM元素结构。一旦页面刷新，之前存放的数据都将被移除

  2. 还可以读取HTML5自定义属性 data-index，得到的是数字型

     - data()方法获取data-index 	H5自定义属性，不用写前面的data-

       ```
       $("元素").data("index")
       ```

  3. 语法

     - 附加数据语法

       ```
       data("name","value")	// 向被选元素附加数据
       ```

     - 获取数据语法

       ```
       data("name")	// 向被选元素获取数据
       ```

------


# 内容文本值

- 主要针对元素的**内容**还有**表单的值**操作

  1. 普通元素内容`html()`（相当于原生innerHTML）

     ```
     html()			// 获取元素的内容（包括标签）
     html("内容")	   // 设置元素的内容
     ```

  2. 普通元素文本内容`text()`（相当于原生innerText）

     ```
     text()			 //	 获取元素的内容（不包括标签）
     text('内容')		//  设置元素的内容
     ```

  3. 获取设置表单值`val()`

     ```
     val()			 // 获取表单值
     val("内容")		// 设置表单的内容
     ```


------

# 元素操作

遍历、创建、添加、删除元素操作

## 遍历元素

- jQuery隐式迭代时对同一个类元素做了同样的操作。如果想要给同一类元素做不同操作，就需要用到遍历

  - 语法一

    ```
    $("div").each(function (index,domEle){})
    ```

    1. each()方法遍历匹配的每一个元素。主要用DOM处理。each每一个

    2. 里面的回调函数有2个参数：index是每个元素的索引号；demEle是每个**DOM元素对象，不是jQuery对象**

    3. 要使用jQuery方法，需要给这个dom元素转换为jQuery对象 $(domEle)

    4. 代码演示

       ```javascript
       <body>
           <div>1</div>
           <div>2</div>
           <div>3</div>
           <script>
               $(function(){
                   // 如果针对于同一类元素做不同操作，需要用到遍历元素（类似for，但是比for强大）
                   // each() 方法遍历元素
                   $('div').each(function(i,domEle){
                       // 回调函数第一个参数一定是索引号，可以自己指定索引号号名称
                       console.log(i)
                       // 回调函数第二个参数一定是dom元素对象
                       console.log(dimEle)
                   })
               })
           </script>
       </body>
       ```

  - 语法二

    ```
    $.each(object,function(index,element{}))
    ```

    1. $.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，对象等
    2. 里面函数用2个参数：index是每个元素的索引号；element 遍历内容
    3. 代码演示

       ```html
       <body>
           <div>1</div>
           <div>2</div>
           <div>3</div>
           <script>
               let arr = ['red','green','blue']
               $(function(){
                   // 遍历所有盒子
                   $.each($('div'),function(i,ele){
                       console.log('盒子索引号：',i);     // 索引号
                       console.log(ele);   // 元素对象
                   })
                   // 遍历数组
                   $.each(arr,function(i,ele){
                       console.log('数组索引号：',i);     // 索引号
                       console.log(ele);   // 元素对象
                   })
                   // 遍历对象
                   $.each({
                       uname:"张三",
                       age:18
                   },function(i,ele){
                       console.log(i);     // 对象的键
                       console.log(ele);   // 对象的值
                   })
               })
           </script>
       </body>
       ```

## 创建、添加、删除元素

1. 创建元素

   - 语法

     ```
     $("<li></li>");
     let li = $("<li></li>")		// 赋值操作
     ```

2. 添加元素

   - 内部添加（生成父子关系）

     - `append`：把创建的元素内部添加并且添加到元素的末尾。类似原生appendChild

       ```
       element.append('创建元素')
       ```

     - `prepend`：把创建的元素内部添加并且放到元素的最前面

       ```
       element.prepend('创建元素')
       ```

   - 外部添加（生成兄弟关系）

     - `after`：把创建的元素添加到目标元素后面

       ```
       element.after("创建元素")
       ```

     - `before`：把创建的元素添加到目标元素前面

       ```
       element.before("创建元素")
       ```

3. 删除元素

   - remove：删除匹配的元素（删除element本身）

     ```
     element.remove()	// 删除本身
     ```

   - empty：删除匹配的元素集合中所有的子节点

     ```
     element.empty()		// 可以删除指定子节点
     ```

   - html：清空匹配的元素内容

     ```
     element.html("")	// 可以删除指定子节点
     ```

------

# 尺寸操作

- 语法

  | 语法                                           | 用法                                                   |
  | ---------------------------------------------- | ------------------------------------------------------ |
  | width() / height()                             | 获取或设置匹配元素宽度和高度值只算width / height       |
  | innerWidth() / innerHieght()                   | 获取或设置匹配元素宽度和高度值 包含 padding            |
  | outerWidth() / outerHeight()                   | 获取或设置匹配元素宽度和高度值 包含padding、border     |
  | outerWidth(true) / outerHeight(true)获取或设置 | 获取或设置元素宽度和高度值 包含 padding、borde、margin |

  1. 参数为空，则是获取相应值，返回的是数字型
  2. 如果参数为数字，则是修改相应值
  3. 参数可以不必写单位

# 位置操作

位置主要有三个：offset()、position()、scrollTop() / scrcollLeft()

## offset()设置或获取元素偏移

1. `offset()`方法设置或返回被选元素相对于**文档**的偏移坐标，跟父级没有关系

2. 该方法有2个属性left、top

   ```javascript
   offset().top	// 用于获取距离文档顶部的距离
   offset().left	// 用于获取距离文档左侧的距离
   
   // 设置元素的偏移
   offset({
   	left:200,
   	top:200
   });
   ```

## position()获取元素偏移

1. `position()`方法用于返回被选元素相对于**带有定位的父级**偏移坐标，如果父级都没有定位，则以文档为准

2. 该方法只能获取不能设置偏移

   ```html
       <style>
           *{
               margin: 0;
               padding: 0;
           }
           .father{
               position: absolute;
               width: 300px;
               height: 300px;
               background: skyblue;
           }
           .son{
               position: relative;
               left: 10px;
               top: 10px;
               width: 100px;
               height: 100px;
               background-color: red;
           }
       </style>
   <body>
       <div class="father">
           <div class="son"></div>
       </div>
       <script>
           $(function(){
            // position()获取带有定位的元素的偏移
            	console.log($('.son').position())	// 输出结果：{top:10, left:10}
           })
       </script>
   </body>
   ```

## scrollTop() / scrollLeft()设置获取元素被卷去的头部和左侧

1. `scrllTop()`方法设置或返回被选元素被卷去的头部
2. `scrollLeft()`方法设置或返回被选元素被卷去的左侧

------

# 事件

## 事件注册

- #### 单个事件注册

  - 语法

    ```
    element.事件(function(){})
    ```

    - 其他事件和原生基本一致
      - 比如mouseover、mouseout、blur、focus、change、keydowm、resize、scroll等

## 事件处理on()

- #### on()绑定事件：`on()`方法在匹配元素上绑定一个或多个事件的事件处理函数

  ```
  element.on(events,[selector],fn)
  
  
  // on()可以绑定多个事件，多个处理事件处理程序
  
  $("div").on({
  	mouseover:function(){},
  	mouseout:function(){},
  	click:function(){}
  });
  
  
  $("div").on("mouseover mouseout",function(){
  
  });
  ```

  1. events：一个或多个用空格分隔的事件类型，如“click”或“keydown”
  2. selector：元素的子元素选择器
  3. fn：回调函数，即绑定在元素身上的侦听函数

- #### 事件委派（委托）

  - 把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素

  - 语法

    ```javascript
    $('ul').on('click','li',function(){
    
    });
    ```


- #### 动态创建元素

  - click()没有办法绑定事件，on()可以给动态生成的元素绑定事件

    ```
    //$("ol li").click(function(){
    //	  alter(11);
    //});
    
    // on可以给未来动态创建的元素绑定事件
    $("ol").on("click","li",function(){
    	alert(11);
    })
    let li = $("<li></li>");
    $("ol").append(li);
    
    
    ```


## 解绑事件

- `off()`方法可以移除通过`on()`方法添加的事件处理程序

  ```
  // 解绑div元素所有事件处理程序
  $("div").off()
  
  // 解绑div元素上面的点击事件
  $("div").off("click")
  
  // 解绑事件委托
  $("ul").off("click","li");
  ```

## 触发一次事件

- `one()`方法只触发一次事件，之后不再触发

  ```
  $("div").one("click",function(){})
  ```

## 自动触发事件

- 有些事件希望自动触发，比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发按钮点击事件，不必鼠标点击触发

  1. 简写形式

     ```
     element.click()
     ```

  2. 自动触发模式

     ```
     element.trigger("type")
     ```

  3. 第二种自动触发模式

     ```
     element.triggerHandler(type)
     ```

     - `triggerHandler()`不会触发元素的默认行为（比如文本框的光标为默认行为）

------

# 事件对象

- 事件被触发，就有事件对象的产生

  ```
  element.on(events,[selector],function(event){})
  ```

  1. 阻止默认行为：event.preventDefault() 或者 return false
  2. 阻止冒泡：event.stopPropagation(0)

# 对象拷贝

- 如果想要把某个对象拷贝（合并）给另一个对象使用，此时可以使用`$.extend()`方法

- 语法

  ```
  $.extend([deep],target,object1,[objectN])
  ```

  1. deep：如果设为true为深拷贝，默认为false浅拷贝
  2. target：要拷贝的目标对象
  3. object1：待拷贝到第一个对象的对象
  4. objectN：待拷贝到第N个对象的对象
  5. 浅拷贝是把被拷贝的对象**复杂数据类型中的地址**拷贝给目标对象，修改目标对象**会影响**被拷贝对象
     - 如果有冲突的属性，会覆盖原有的属性
  6. 深拷贝，前面加true，深拷贝把里面的数据完全复制一份给目标对象（拷贝的对象，而不是地址），修改目标对象**不会影响**被拷贝对象
     - 如果里面有不冲突的属性，会合并到一起

------

# 多库共存

1. 问题概述

   - jQuery使用\$作为标识符，其他js库也会用\$作为标识符，这样一起使用会引起冲突

2. 客观需求

   - 让jQuery和其他的js库不存在冲突，可以同时存在，这就叫做多库共存

3. 解决方案

   - 把里面的 $ 符号统一改为jQuery。比如jQuery("div")

   - jQuery变量规定新的名称：$.noConflict()

     ```javascript
     let 变量名 = $.noConflict();
     
     // 代码演示
     // let code = jQuery.noConflict();
     let code = $.noConflict();
     code("div");	// 选择元素
     ```

   ------


# jQuery插件

- jQuery功能比较有限，想要更复杂的特殊效果，可以借助于jQuery插件完成
- 注意：这些插件也是依赖于jQuery来完成的，所以必须要先引入jQuery文件，因此也称为jQuery插件
  - jQuery插件库：http://www.jq22.com/
  - jQuery之家：http://www.htmleaf

- jQuery插件使用步骤
  1. 引入相关文件。（jQuery文件和插件文件）
  2. 复制相关html、css、js（调用插件）
