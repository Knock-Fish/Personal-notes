# 视口

> **视口（viewport）**就是浏览器显示页面内容的屏幕区域。视口可以分为布局视口、视觉视口和理想视口

## 布局视口layout viewport

<img src="http://127.0.0.1:8888/uploads/202302252107898.png" style="zoom:50%;" />  

1. 一般移动设备的浏览器都默认设置了一个布局视口，用于解决早期的PC端页面在手机上显示的问题
2. iOS,Android基本都将这个视口分辨率设置为980px，所以PC上的网页大多都能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页

## 视觉视口visual viewport

<img src="http://127.0.0.1:8888/uploads/202302252109730.png" style="zoom:50%;" /> 

1. 视觉视口，是用户正在看到的网站的区域
2. 可以通过缩放去操作视觉视口，当不会影响布局视口，布局视口仍保持原来的宽度

## 理想视口ideal viewport（主要）

1. 理想视口，对设备来讲，是最理想的视口尺寸
2. 需要手动添写meta视口标签通知浏览器操作
3. meta视口标签的主要目的：布局视口的宽度应该与理想的视口宽度一致（设备有多宽，布局的视口就多宽）

## mete视口标签

- 格式

  ```html
  <meta name="view<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  ```

- 属性

  | 属性          | 说明                                                 |
  | ------------- | ---------------------------------------------------- |
  | width         | 宽度设置的是viewport宽度，可以设置device-width特殊值 |
  | initial-scale | 初始缩放比，大于0的数字                              |
  | maximum-scale | 最大缩放比，大于0的数字                              |
  | minimum-scale | 最小缩放比，大于0的数字                              |
  | user-scalable | 用户是否可以缩放，yes或no（1或0）                    |

## 标准的viewport设置

1. 视口宽度和设备保持一致
2. 视口的默认缩放比例1.0
3. 不允许用户自行缩放
4. 最大允许的缩放比例1.0
5. 最小允许的缩放比例1.0

------

# 二倍图

## 分辨率

- 分辨率分类
  1. **物理分辨率**是生产屏幕时就固定的，它是不可被改变的
  2. **逻辑分辨率**是由软件（驱动）决定的
- 了解移动端主流设备分辨率
  - 图示（红框为主要参考标准）![](http://127.0.0.1:8888/uploads/202303260112683.png)

## 多倍图

1. 对于图片，在手机Retina屏中打开，按照图片的物理分辨率比会放大倍数，这样会造成图片模糊
2. 在标准的viewport设置中，使用倍图来提高图片质量，解决在高清设备中的模糊问题
   - 图示（图左相对模糊）![](http://127.0.0.1:8888/uploads/202302252142860.png)

## 背景缩放background-size

- `background-size`属性规定背景图像的尺寸（如只写一个参数，会等比例缩放）

  ```css
  background-size: 背景图片宽度 背景图片高度
  ```

- **参数**

  | 参数    | 说明                             |
  | ------- | -------------------------------- |
  | cover   | 图片等比例拉伸且完全覆盖         |
  | contain | 图片等比例拉伸，直到图片铺满盒子 |
  | px      | 像素缩放                         |
  | %       | 百分比缩放                       |

- **当参数为 长度px 或  百分比**![](http://127.0.0.1:8888/uploads/202302252153953.png)

- **当参数为 cover  或 contain**![](http://127.0.0.1:8888/uploads/202302252152556.png)

- **cover  和 contain缩放原理**

  - cover把背景图像扩展至足够大，以是背景图像完全覆盖背景区域（等比例拉伸且完全覆盖）<img src="http://127.0.0.1:8888/uploads/202302252223581.gif" style="zoom:50%;" />

  - contain把图像扩展至最大尺寸，宽度或高度等比例拉伸，当宽度或者高度铺满盒子就不再进行拉伸

    - contain宽度铺满

      <img src="http://127.0.0.1:8888/uploads/202302252226006.gif" style="zoom:50%;" /> 

    - contain高度铺满

      <img src="http://127.0.0.1:8888/uploads/202302252229439.gif" style="zoom:50%;" /> 

------

# 移动端开发选择和技术解决方案

- 单独移动端页面（主流）

  - 通常情况下，网址域名前面加**m(mobile)**可以打开移动端。通过判断设备，如果是移动设备打开，则跳到**移动端页面**

- 响应式兼容PC移动端

  - 通过判断屏幕宽度来改变样式，以适应不同终端
  - 缺点：**制作麻烦**，需要花很大精力去调**兼容性**问题

- 特殊样式

  ```css
  /*CSS3盒子模型*/
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  /*点击高亮清除 设置为transprent 透明*/
  -webkit-tap-highlight-color: transparent;
  /*在移动端浏览器默认的外观在ios上加上这个属性才能给按钮和输入框自定义样式*/
  -webkit-appearance: none;
  /*禁用长按页面时的弹出菜单*/
  img, a{ -webkit-touch-callout: none; }
  ```
  

# 流式布局（百分比布局）

1. 流式布局，就是百分比布局，也称非固定像素布局
2. 通过盒子的宽度设置成百分比来根据屏幕的宽度来进行伸缩，不受固定像素的限制，内容向两侧填充

   - 使用样式限制伸缩范围

     - max-width 最大宽度 （max-height 最大高度）

     - min-width 最小宽度 （min-height 最小高度）
3. 流式布局方式是移动web开发使用的比较常见的布局方式
   - 示例![](http://127.0.0.1:8888/uploads/202302260025941.gif)

# flex布局

## flex布局设置方法

- 设置方式
  - 父元素添加`display: flex;`，子元素可以自动的挤压或拉伸


### 布局原理

- flex是flexible Box 的缩写，意为“弹性布局”，用来为盒状模型提供最大的灵活性，任何一个容器（不管块级元素或行内元素）都可以指定为flex布局![](http://127.0.0.1:8888/uploads/202302260030645.gif)

- **注意**

  - 当为父盒子设定为flex布局后，子元素的float、clear和vertical-align属性将失效

  - 伸缩布局 = 弹性布局 = 伸缩和布局 = 弹性和布局 = flex布局
- 采用Flex布局的元素，称为Flex容器（flex container），简称”容器“。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称”项目“

  <img src="http://127.0.0.1:8888/uploads/202302260030438.png" style="zoom:50%;" /> 

  - 子容器可以横向排列也可以纵向排列
  - **总结原理：通过父盒子添加flex属性，来控制子盒子的位置和排列方式**

## flex布局父项常见属性

### flex-direction设置主轴的方向

- 主轴与侧轴

  - 在flex布局中，是分为主轴和侧轴两个方向，同样的叫法有：行和列、x轴和y轴<img src="http://127.0.0.1:8888/uploads/202302260035262.png" style="zoom: 67%;" />
    1. 默认主轴方向就是x轴方向，水平向右
    2. 默认侧轴方向就是y轴方向，水平向下

- 属性值

  | 属性值         | 说明           |
  | -------------- | -------------- |
  | row            | 默认值从左到右 |
  | row-reverse    | 从右到左       |
  | column         | 从上到下       |
  | column-reverse | 从下到上       |

  - flex-dirction属性决定主轴的方向（即项目的排列方向）
  - 主轴和侧轴是会变化的，就看flex-direction设置谁为主轴，剩下的就是侧轴。而子元素是跟着主轴来排列的
  
  - 视图![](http://127.0.0.1:8888/uploads/202302261216364.jpg)

### justify-content设置主轴上的子元素排列方式

> **注意：使用该属性之前一定要确定好主轴 修改主轴justify-content使用同理 **

- 属性值

  - justify-content属性定义了项目在主轴上的对齐方式

    | 属性值        | 说明                                               |
    | ------------- | -------------------------------------------------- |
    | flex-start    | 默认值从头部开始，如果主轴是x轴，则从左到右        |
    | flex-end      | 从尾部开始排列                                     |
    | center        | 在主轴居中对齐（如果主轴是 x 轴则水平居中）        |
    | space-around  | 平分剩余空间（会给每个盒子添加相同的的左右外边距） |
    | space-between | 先两边贴边，再平分剩余空间（重要）                 |

  - 视图![](http://127.0.0.1:8888/uploads/202302261235656.jpg)

### flex-wrap设置子元素是否换行

1. 默认情况下，项目都排在一条线（又称“轴线”）上。flex-wrap属性定义，flex布局中默认不换行的

2. flex布局中，默认的子元素是不换行的，如果装不开，会缩小子元素的宽度，放到父元素里面

   - 示例![](http://127.0.0.1:8888/uploads/202302261442400.gif)

3. 属性值

   | 属性值 |      说明      |
   | :----: | :------------: |
   | nowrap | 默认值，不换行 |
   |  wrap  |      换行      |

   - 示例![](http://127.0.0.1:8888/uploads/202302261446718.png)

### align-content设置侧轴上的子元素的排列方式（多行）

1. 设置子项在侧轴上的排列方式，并且只能用于子项出现 换行 的情况（多行），在单行下是没有效果的

2. 有了换行就可以使用align-content在侧轴上控制子元素的对齐方式

3. 属性值

   | 属性值        | 说明                                   |
   | ------------- | -------------------------------------- |
   | flex-start    | 默认值在侧轴的头部开始排列             |
   | flex-end      | 在侧轴的尾部开始排列                   |
   | center        | 在侧轴中间显示                         |
   | space-around  | 子项在侧轴平分剩余空间                 |
   | space-between | 子项在侧轴先分布在两头，再平分剩余空间 |
   | stretch       | 设置子项元素高度平分父元素高度         |

   - 视图![](http://127.0.0.1:8888/uploads/202302271041583.jpg)

### align-items设置侧轴上的字元素排列方式（单行）

1. align-items是控制子项在侧轴（默认是y轴）上的排列方式，在子项为单项的时候使用

2. 属性

   | 属性值     | 说明                     |
   | ---------- | ------------------------ |
   | flex-start | 从上到下                 |
   | flex-end   | 从下到上                 |
   | center     | 挤在一起居中（垂直居中） |
   | stretch    | 拉伸（默认值）           |

   - 使用 `align-items: stretch; `会在**没有的宽度或高度限制下拉伸**（拉伸的方向为侧轴的方向）
   - 视图![](http://127.0.0.1:8888/uploads/202303050045195.jpg)

### flex-flow复合属性

- `flex-flow`属性是`flex-direction`和`flex-wrap`属性的复合属性，相当于同时设置了`flex-direction`和`flex-wrap`
  - 示例![](http://127.0.0.1:8888/uploads/202303050106978.png)

### flex布局子项常见属性

#### flex属性

- `flex`属性定义子项目分配剩余空间，用flex来表示占多少**份数**

  - `flex`属性值默认为0

  - 结构

    ```html
     <style>
            section{
                 display: flex;
                 width: 60%;
                 height: 150px;
                 background-color: blue;
                 margin: 0 auto;
             }
            section div:nth-child(1){
                width: 100px;
                height: 150px;
                background-color: red;
            }
            section div:nth-child(2){
                flex: 1;
                background-color: skyblue;
            }
            section div:nth-child(3){
                width: 100px;
                height: 150px;
                background-color: green;
            }
    
            p{
                display: flex;
                width: 60%;
                height: 150px;
                background-color: blue;
                margin: 100px auto;
            }
            p span{
                flex: 1;
            }
            p span:nth-child(2){
                flex: 2;
                background-color: red;
            }
        </style>
    <body>
        <section>
            <div></div>
            <div></div>
            <div></div>
        </section>
        <p><span>1</span><span>2</span><span>3</span></p>
    </body>
    ```

    - 示例![](http://127.0.0.1:8888/uploads/202303050923565.gif)
  

#### align-self控制子项自己在侧轴的排列方式

1. align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性
2. 默认值为auto，表示**继承父元素的align-items属性**，如果没有父元素，则等同于stretch
   - 示例![](http://127.0.0.1:8888/uploads/202303050927180.png)

#### order属性定义子项的排列顺序（前后顺序）

1. 数值越小，排列越靠前，默认为0
2. 注意：和z-index不一样
   - 示例![](http://127.0.0.1:8888/uploads/202303050930418.png)

# rem适配布局

## rem单位

1. rem（root em）是一个相对单位

   - 类似于em，em是父元素字体大小（如父级字体为12px，那么子级的1em = 12px），em是相对于父元素的字体大小而言

     - 示例

       <img src="http://127.0.0.1:8888/uploads/202303050941329.png" style="zoom:67%;" /> 

2. **rem的基准是相对于html元素的字体大小（rem相对于html元素字体大小而言）**

   - rem的优点就是可以通过修改html里面的文字大小来改变页面中元素的大小可以整体控制

   - 比如，根元素（html）设置font-size = 12px; 非根元素设置width:2rem;则换成px表示就是24px

     - 示例

       <img src="http://127.0.0.1:8888/uploads/202303050952102.gif" style="zoom:67%;" /> 

### 媒体查询

- **媒体查询（Media Query）**

  1. 使用 @media 查询，可以针对不同的媒体类型定义不同的样式
  2. **@media 可以针对不同的屏幕尺寸设置不同的样式**
  3. 当重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面
  4. 目前针对很多苹果手机、Android手机，平板等设备都用得到多媒体查询

- **语法规范**

  ```css
  @media mediatype and | not | only (media feature){
      CSS-Code;
  }
  ```

  1.  用 @media 开头，注意@符号
  2. mediatype 媒体类型
  3. 关键字 and not only
  4. media feature 媒体特性 必须有小括号包含

- **mediatype查询类型**

  - 将不同的终端设备划分成不同的类型，称为媒体类型

    | 值     | 说明                               |
    | ------ | ---------------------------------- |
    | all    | 用于所有设备                       |
    | print  | 用于打印机和打印预览               |
    | screen | 用于电脑屏幕，平板电脑，智能手机等 |

- **关键字**

  - 关键字将媒体类型或多个媒体特性连接到一起作为媒体查询的条件

    | 关键字 | 说明                                               |
    | ------ | -------------------------------------------------- |
    | and    | 可以将多个媒体特性连接到一起，相当于 “ 且 ” 的意思 |
    | not    | 排除某个媒体类型，相当于 “ 非 ” 的意思，可以省略   |
    | only   | 指定某个特定的媒体类型，可以省略                   |

- **媒体特性**

  - 每种媒体类型都具体各自不同的特性，根据不同媒体类型的媒体特性设置不同的展示风格

    | 值        | 说明                                 |
    | --------- | ------------------------------------ |
    | width     | 定义输出设备中页面可见区域的宽度     |
    | min-width | 定义输出设备中页面最小可见区域的宽度 |
    | max-width | 定义输出设备中页面最大可见区域的宽度 |

    - 示例![](http://127.0.0.1:8888/uploads/202303051123134.gif)
  


### 媒体查询+rem实现元素变化

- 示例![](http://127.0.0.1:8888/uploads/202303051535340.gif)

# 视窗单位

## vw / vh 

> 实现在不同宽度的设备中，网页元素尺寸等比例缩放效果

1. vw / vh 是一种视窗单位，也是相对单位
   - 相对的不是父节点或者页面的根节点。而是由视窗（Viewport）大小来决定的，单位 1，代表类似于 1%。
   - 视窗(Viewport)是你的浏览器实际显示内容的区域—（不包括工具栏和按钮的网页浏览器）
2. 相对视口的尺寸计算结果

   - vw：viewport width
     - 1vw = 1/100视口宽度 
   - vh：viewport height
     - vh = 1/100视口高度

   - 示例![](http://127.0.0.1:8888/uploads/202303281301635.png)
3. vw / vh 单位尺寸适配
   - vw单位的尺寸 = px 单位数值 / (1/100视口宽度)
   - vh 是1/100视口高度，**全面屏视口高度尺寸大**，如果混用可能会导致**盒子变形**
4. vw、vh 与 % 百分比的区别
   - % 是相对于父元素的大小设定的比率，vw、vh 是视窗大小决定的
   - vw、vh 优势在于能够直接获取高度，而用 % 在没有设置 body 高度的情况下，是无法正确获得可视区域的高度的

## vmin / vmax

> 照顾手机端横屏和竖屏的显示效果

- vmin 和 vmax 是与当下屏幕的宽度和高度的最大值或最小值有关，取决于哪个更大和更小
  - `vmin`使用最小那边的比例。也就是说，如果浏览器窗口的高度小于他的宽时，`1vmin`将等价于`1vh`；如果浏览器的宽度小于他的高度时，`1vmin`等价于`1vw`
  - `vmax`则相反：它使用最大的那一边。因此当viewport的宽大于高时，`1vmax`等价于`1vw`；如果浏览器的高大于宽时，`1max`将等价于`1vh`
- 用处
  - 做移动页面开发时，如果使用 vw、wh 设置字体大小（比如 5vw），在竖屏和横屏状态下显示的字体大小是不一样的
  - 由于 vmin 和 vmax 是当前较小的 vw 和 vh 和当前较大的 vw 和 vh。这里就可以用到 vmin 和 vmax。使得文字大小在横竖屏下保持一致

- 代码结构（示例）

  ```html
  <style>
  .box-vmin{
      width: 10vmin;
      height: 10vmin;
      background-color: blue;
  }
  .box-vw{
      width: 10vw;
      height: 10vw;
      background-color: red;
  }
  </style>
  <body>
      <div class="box-vmin">vmin</div>
      <hr>
      <div class="box-vw">vw</div>
  </body>
  ```

  - 竖屏状态下

    <img src="http://127.0.0.1:8888/uploads/202306062110910.png" style="zoom:60%;" /> 

  - 横屏状态下

    <img src="http://127.0.0.1:8888/uploads/202306062111193.png" style="zoom:60%;" /> 