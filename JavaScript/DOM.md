# DOM简介

1. 文档对象模型(Document Object Model，简称 **DOM**)，是W3C组织推荐的处理可扩展标记语言（HTML或者XML）的标准**编程接口**
2. W3C已经定义了一系列的DOM接口，通过这些DOM接口可以改变网页的内容、结构和样式
3. DOM树![](http://127.0.0.1:8888/uploads/202302281702265.png)
   - 文档：一个页面就是一个文档，DOM中使用document 表示
   - 元素：页面中的所有标签都是元素，DOM中使用element表示
   - 节点：网页中的所有内容都是节点（标签、属性、文本、注释等），DOM中使用node表示
   - **DOM把以上内容都看做是对象**

# 获取页面元素

> DOM在实际开发中主要用来操作元素

## getElementByld

1. 使用` getElementByld()`方法可以获取带有ID的元素对象

1. get 获得，element 元素，by 通过。驼峰命名法 get Element By Id

1. 参数id是大小写敏感的字符串，代表所要查找的元素的唯一ID

1. element是一个 Element 对象。如果当前文档中拥有特定ID的元素不存在则返回 null

1. 返回值：返回一个匹配到 ID 的 DOM Element 对象。若在当前 Document 下没有找到，则返回 null

   ```javascript
   let timer = document.getElementById('time');    // 返回的是一个元素对象
   console.log(timer);
   console.log(typeof timer); // 输出obj
   
   // console.dir 打印返回的元素对象，更好的查看里面的属性和方法
   console.dir(timer);
   ```

##  getElementdByTagName

使用`getElementdByTagName()`方法可以返回带有指定标签名的对象的集合

```html
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
<body> 
<script>
    let lis = document.getElementsByTagName('li');  // 返回的是 获取过来元素对象的集合，以伪数组的形式存储的
    // 如果页面中只有一个li，返回的还是伪数组的形式：[li]
    // 如果页面中没有这个元素，返回的空的伪数组的形式：[]
    console.log(lis);   	// 获取lis元素对象集合(伪数组) [li, li, li, li, li]
    console.log(lis[0]); 	//获取下标为 0 的元素

    // 依次打印里面的元素对象可以采取遍历的方式
    for (let i = 0; i < lis.length; i++ ){
        console.log(lis[i]);
    }
</script>

```

## getElementsByTagName

`getElementsByTagName`获取某个元素(父元素)内部所有指定标签名的子元素

```javascript
element.getElementsByTagName('标签名')；
```

```html
<body>
    <ol>
        <li>6</li>
        <li>7</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ol>
</body>
<script>
    //获取某个元素(父元素)内部所有指定标签名的子元素
    // element.getElementsByTagName('标签名');  父元素必须是指定的单个元素
    let ols = document.getElementsByTagName('ol');  	// 获取[ol]的内容

    // 获取 ols 里面的 li 元素 [li, li, li, li, li]
    console.log(ols[0].getElementsByTagName('li')); 	// 获取的父元素必须添加索引号
</script>
```

## getElementById

`getElementById`根据ID来指定获取元素

```html
<body>
    <ol id="oL">
        <li>6</li>
        <li>7</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ol>
</body> 
<script>
    let oLs = document.getElementById('ol');		// 参数id为 ol
    console.log(oLs.getElementsByTagName('li'))		// 输出结果：[li,li,li,li,li]
</script>
```

## getElementByClassName

`getElementByClassName`根据类名返回元素对象集合

```
document.getElementByClassName('类名')
```

```html
<body>
    <div class="box">盒子1</div>
    <div class="box">盒子2</div>
    <div id="nav">
        <ul>
            <li>首页</li>
            <li>内容</li>
        </ul>
    </div>
</body> 
<script>
    // getElementsByClassName 根据类名获取某些元素集合
    let boxs = document.getElementsByClassName('box');
    console.log(boxs);
</script>
```

## querySelector

`querySelector`根据指定选择器返回第一个元素对象

```
document.querySelector('选择器')
```

```javascript
<body>
<div class="box">盒子1</div>
<div class="box">盒子2</div>
<div id="nav">
    <ul>
        <li>首页</li>
        <li>内容</li>
    </ul>
</div>
<script>
    // querySelector 返回指定选择器的 第一个元素对象
    let firstBox = document.querySelector('.box');  //加( . ) 表示是 类选择器
    console.log(firstBox);

    let nav = document.querySelector('#nav');   // 加( # ) 表示是 ID选择器
    console.log(nav);

    let li = document.querySelector('li');  // 标签选择器不需要加任何符号
    console.log(li);
</script>
</body>
```

## querySeletorAll

`querySeletorAll`根据指定选择器返回所有元素对象集合

```javascript
document.querySeletorAll('选择器')；
```

```html
<body>
<div class="box">盒子1</div>
<div class="box">盒子2</div>
<div id="nav">
    <ul>
        <li>首页</li>
        <li>内容</li>
    </ul>
</div>
<script>
    // querySelectorAll 返回指定选择器的 所有的元素对象
    let allBox = document.querySelectorAll('.box');
    console.log(allBox);
    let lis = document.querySelectorAll('li');
    console.log(lis);

</script>
</body>
```

## getComputedStyle

> `Window.getComputedStyle()`方法返回一个对象，该对象在应用活动样式表并解析这些值可能包含的任何基本计算后报告元素的所有 CSS 属性的值。私有的 CSS 属性值可以通过对象提供的 API 或通过简单地使用 CSS 属性名称进行索引来访问

返回的`style`是一个实时的 `CSSStyleDeclaration` 对象，当元素的样式更改时，它会自动更新本身

```javascript
window.getComputedStyle(element, [pseudoElt]);
```

- element：用于获取计算样式的`Element`
- pseudoElt：指定一个要匹配的伪元素的字符串。必须对普通元素省略（或`null`）

```javascript
let elem1 = document.getElementById("elemId");
let style = window.getComputedStyle(elem1, null);

// 它等价于
// let style = document.defaultView.getComputedStyle(elem1, null);
```

## getPropertyValue

**getPropertyValue()** 接口返回一个 `DOMString`，其中包含请求的 CSS 属性的值

```javascript
style.getPropertyValue(property);
```

- property 是一个 `DOMString`，是需要查询的 CSS 属性名称
- 返回值：`value` 是 `DOMString` ，包含查找属性的值。若对应属性没有设置，则返回空字符串

```javascript
let declaration = document.styleSheets[0].cssRules[0].style;
let value = declaration.getPropertyValue('margin'); // "1px 2px"
```

## 获取body元素

```javascript
// 获取 body 元素
let bodyEle = document.body;
console.log(bodyEle)
```

## 获取html元素

```javascript
// 获取 html 元素
let htmEle = document.documentElement;
console.log(htmlEle);
```

------

# 事件

1. JavaScript使我们有能力创建动态页面，而事件是可以被JavaScript侦测到的行为（触发---响应机制）
2. 网页中的每个元素都可以产生某些可以触发JavaScript的事件，例如，我们可以在用户点击某些按钮是产生一个事件，然后去执行某些操作
3. 在JavaScript里添加事件类型时，必须在事件类型前添加 “ on ”。例如onclick、onmouseover、onfocus
4. 使用addEventListener注册事件时，添加事件类型时不需要加 “on” 

## 鼠标事件

| 鼠标事件   | 触发条件                 |
| ---------- | ------------------------ |
| click      | 鼠标点击左键触发         |
| mouseover  | 鼠标经过触发             |
| mouseout   | 鼠标离开触发             |
| focus      | 获得鼠标焦点触发         |
| blur       | 失去鼠标焦点触发         |
| mousemove  | 鼠标移动触发             |
| mouseuo    | 鼠标弹起触发             |
| mousedown  | 鼠标按下触发             |
| scrool     | 滚动页面条               |
| mouseenter | 鼠标经过触发（不会冒泡） |

- **mouseenter 鼠标事件**
  - 当鼠标移动到元素上时就会触发mouseenter事件

  - mouseenter 和 mouseover 的区别![](http://127.0.0.1:8888/uploads/202302282225956.gif)
    1. mouseover鼠标经过自身盒子会触发，经过子盒子还会触发。
    2. mouseenter只会经过自身盒子触发
    3. 之所以这样，就是因为mouseenter不会冒泡（冒泡参考[](DOM事件流)）
    4. 跟mouseenter搭配 鼠标离开mouseleave同样不会冒泡

## 表单事件

| 表单事件 | 触发条件 |
| -------- | -------- |
| submit   |          |

## 键盘事件

| 键盘事件   | 触发条件                                                     |
| ---------- | ------------------------------------------------------------ |
| onkeyup    | 某个键盘按键被松开时触发                                     |
| onkeydown  | 某个键盘按键被按下时触发                                     |
| onkrypress | 某个键盘按键被按下时触发（但它不识别功能键 比如 ctrl shift 箭头等） |

- 三个事件执行顺序：keydown -> keypress -> keyup

# 操作元素

> JavaScript的DOM操作可以改变网页内容、结构和样式，我们可以利用DOM操作元素来改变元素里面的内容、属性等

## 改变元素内容

### 表单元素的属性操作

利用DOM可以操作表单元素的属性：type，value，checked，selected，disbled，src，href，id，alt

innerText和innerHTML

1. innerText语法

   ```
   element.innerText
   ```

   - 从起始位置到终止位置的内容，但innerText会去除 html 标签，同时空格和换行也会去掉

2. innerHTML语法

   ```
   element.innerHTML
   ```

   - 起始位置到终止位置的全部内容，包括html标签，同时保留空格和换行

3. **innerText和innerHTML的区别**

   1. innerText**不识别**html标签，去除空格和换行

   2. innerHTML**识别**html标签，保留空格和换行

   3. innerText和innerHTML是可读写的，可以获取元素里面的内容

   ![](http://127.0.0.1:8888/uploads/202302282248222.png)

### insertAdjacentHTML

> `insertAdjacentHTML()`方法将指定的文本解析为 `Element `元素，并将结果节点插入到 DOM 树中的指定位置。它不会重新解析它正在使用的元素，因此它不会破坏元素内的现有元素

```
element.insertAdjacentHTML(position, text);
```

- position表示插入内容相对于元素的位置，并且必须是以下字符串之一：
  1. `'beforebegin'`：元素自身的前面。
  2. `'afterbegin'`：插入元素内部的第一个子节点之前。
  3. `'beforeend'`：插入元素内部的最后一个子节点之后。
  4. `'afterend'`：元素自身的后面

- text：是要被解析为 HTML 或 XML 元素，并插入到 DOM 树中的 `DOMString`

```javascript
// 原为 <div id="one">one</div>
let d1 = document.getElementById('one');
d1.insertAdjacentHTML('afterend', '<div id="two">two</div>');

// 此时，新结构变成：
// <div id="one">one</div><div id="two">two</div>
```

## 样式属性操作

> 通过JS修改元素的大小、颜色、位置等

### 通过 style 修改样式

```javascript
元素.style.样式 = '值'			// 行内样式操作
```

1. JS里面的样式采取驼峰命名法，比如 fontSize、backgroundColor

2. JS修改style样式操作，产生的是行内样式（CSS权重比较高）

   ![](http://127.0.0.1:8888/uploads/202302282303547.png)


### 通过className 修改样式

```javascript
元素.className('类名')		// 类名式操作（类名不需要加点 ，并且是字符串）
```

1. 如果样式修改较多，可以采取操作类名方式更改元素样式

2. **className会直接更改元素的类名，会覆盖原先的类名**

   ![](http://127.0.0.1:8888/uploads/202302282317445.gif)

### 通过 classList  操作类控制 CSS

> 为了解决 className 容易覆盖以前的类名，可以通过 classList 方式追加和删除类名
>
> classList 不会影响其他类名

```javascript
元素.classList.方法('类名');		// 类名不需要加点，并且是字符串
```

| 方法名 | 说明                               |
| ------ | ---------------------------------- |
| add    | 追加一个类                         |
| remove | 删除一个类                         |
| toggle | 切换一个类（有就删掉，没有就加上） |

## 自定义属性的操作

### 获取属性值

1. 获取内置属性值（元素本身自带的属性）

   ```javascript
   element.属性
   ```

2. 主要获取自定义的属性（标准），程序员自定义的属性

   ```javascript
   element.getAttribute('属性');
   ```


### 设置属性值

设置内置属性值

```javascript
element.属性 = '值'`
```

```javascript
element.setAttribute('属性','值');
```

### 移除属性

```javascript
element.removeAttribute('属性');
```

```javascript
<body>
<!--自己添加的属性称为自定义属性 index-->
    <div id="demo" index="1" class="nav"></div>
    <script>
        let oDiv = document.querySelector('div');

        //获取元素的属性值
        // element.属性
        // 获取对象id
        console.log(oDiv.id);   // demo
        // 获取对象类名
        console.log(oDiv.class);    // undefined
        
        // 自己添加的属性称为自定义属性 index
        // element.getAttribute('属性')
        console.log(oDiv.getAttribute('id'));   //  demo
        console.log(oDiv.getAttribute('index'));    // 1
        console.log(oDiv.getAttribute('class'));    // nav

        // 设置元素的属性
        // element.属性 = '值';
        oDiv.id = 'test';
        oDiv.className = 'navs';
        // element.setAttribute('属性','值');  主要针对于自定义属性
        oDiv.setAttribute('index', 2);
        oDiv.setAttribute('class', 'box'); // class特殊，这里写class，不是className

        // 移除属性
        // element.removeAttribute('属性');
        oDiv.removeAttribute('index');
    </script>
</body>
```

# 自定义属性

标准属性：标签天生自带的属性，比如 class id title等，可以直接使用点语法操作比如：disabled、checked、selected

自定义属性：

1. 在html5中推出了专门的 data- 自定义属性
2. 在标签上一律以 data- 开头
3. 在DOM对象上一律以 dataset 对象方式获取

```javascript
<div data-id="1" data-spm="不知道">1</div>
<div data-id="2">2</div>
<div data-id="3">3</div>
<div data-id="4">4</div>
<div data-id="5">5</div>

const one = document.querySelector('div');
console.log(one.dataset);       // DOMStringMap {id: "1", spm: "不知道"}
console.log(one.dataset.id);    // 1
console.log(one.dataset.spm);   // 不知道
```

# 节点操作

## 节点层级

- 利用DOM树可以把节点划分为不同的层级关系，常见的是**父子层级关系**<img src="http://127.0.0.1:8888/uploads/202302281702265.png" style="zoom: 80%;" />

## 父级节点

`parentNode`属性可返回某节点**最近的一个父节点**

```javascript
.parentNode
```

如果指定的节点没有父节点则返回null![](http://127.0.0.1:8888/uploads/202303010950615.png)

## 子节点

`parentNode.childNodes`**返回包含指定节点的子节点的集合**，该集合为即时更新的集合

```javascript
.childNodes
```

**返回值包含了所有的子节点，包括元素节点，文本节点（包括换行）等![](http://127.0.0.1:8888/uploads/202303010959616.png)**

## 获取所有的元素节点

`parentNode.children`是一个只读属性，**返回所有的子元素节点**。

```javascript
.children
```

它只返回子元素节点，其余节点不返回![](http://127.0.0.1:8888/uploads/202303010952514.png)

## 获取第一个子节点和最后一个子节点

`parentNode.firstChild`返回第一个子节点，找不到则返回null。同样，也是包含所有的节点（元素节点，文本节点、换行等）

```javascript
.firstChild
```

`parentNode.lastChild`返回最后一个节点，找不到则返回null。同样，也是包含所有的节点（元素节点，文本节点、换行等）

```javascript
.lastChild
```

![](http://127.0.0.1:8888/uploads/202303011016948.png)

## 获取第一个子元素节点和最后一个子元素节点

`parentNode.firstElemenChild`返回第一个子元素节点，找不到则返回null

```javascript
.firstElemenChild
```

`parentNode.lastElementChild`返回最后一个子元素节点，找不到则返回null

```javascript
.lastElementChild
```

![](http://127.0.0.1:8888/uploads/202303011026991.png)

## 兄弟节点

`node.nextSibling`返回当前元素的下一个兄弟节点，找不到则返回null。同样，包含所有的节点

```javascript
.nextSibling
```

`node.previouSibling`返回当前元素的上一个兄弟节点，找不到则返回null。同样，包含所有的节点

```javascript
.previouSibling
```

![](http://127.0.0.1:8888/uploads/202303011030133.png)

## 获取兄弟节点的元素节点

`node.nextElementSibling`返回当前元素的下一个兄弟元素节点，找不到则返回null

```javascript
.nextElementSibling
```

`node.previousElementSibling`返回当前元素的上一个兄弟元素节点，找不到则返回null

```javascript
.previousElementSibling
```

![](http://127.0.0.1:8888/uploads/202303011052290.png)

## 创建和添加节点

- 创建节点
  - `document.createElement('tagName')`方法创建由tagName指定的HTML元素。因为这些元素原先不存在，是根据需求动态生成的，所以也称为**动态创建元素节点**

- 添加节点
  1. `node.appendChild(child)`方法将一个节点添加到指定父节点的子节点列表**末尾**。类似于css里面都after伪元素
  2. `node.insertBefore(child,指定元素)`方法将一个节点添加到父节点的指定字节点前面。类似于css里面的before

## 删除节点

`node.removeChild(child)`方法从DOM中删除一个子节点，返回删除的节点

![](http://127.0.0.1:8888/uploads/202303011332034.png)

## 复制节点（克隆节点）

`node.cloneNode()`方法返回调用该方法的节点的一个副本。也称为克隆节点/拷贝节点

1. 如果括号参数**为空或者为false**，则是**浅拷贝**，即只克隆复制节点本身，不克隆里面的子节点、内容
2. 如果括号参数为true，则是**深度拷贝**，会复制节点本身以及里面所有的子节点

![](http://127.0.0.1:8888/uploads/202303011341789.png)

## document.write动态创建元素

```JavaScript
document.write()
```

`document.write`是直接将内容写入页面的内容流，**但是文档流执行完毕，则它会导致页面全部重绘**

1. 页面从上往下依次执行，但它执行到最后一个页面全部显示完毕，那整个文档流就执行完毕了 
2. 页面重绘：重新绘制新的HTML页面![](http://127.0.0.1:8888/uploads/202303011408026.gif)

------

# addEvenListener注册事件（绑定事件）

## addEvenListener注册事件

1. `addEvenListener()`是一个方法
2. **特点**：同一个元素同一个事件可以注册多个监听器
3. 按注册顺序依次执行

## addEvenListener事件监听方式

```javascript
eventTarget.addEventListener(type,listener[, useCapture])
```

> `eventTarget.addEventListener()`方法将指定的监听器注册到eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数

- 该方法接收三个参数：
  1. **type**：事件类型字符串，比如click、mouseover，注意这里不要带on
  2. **listener**：事件处理函数，事件发生时，会调用该监听函数
  3. **useCapture**：可选参数，是一个布尔值，默认是false

  ![](http://127.0.0.1:8888/uploads/202303011415376.gif)

# 删除事件（解绑事件）

## 删除事件的方式

传统注册方式（删除事件）

```javascript
eventTarget.onclick = null;
```

方法监听注册方式（删除事件）

```javascript
eventTarget.removeEventListener(事件类型,事件处理函数,[获取捕获或者冒泡阶段])
```

![](http://127.0.0.1:8888/uploads/202303011448041.gif)

# DOM事件流

## 概述

1. **事件流**描述的是从页面中接收事件的顺序

2. 事件发生时会再元素节点之间按照特定的顺序传播，这个**传播过程**即**DOM事件流**

   - DOM事件流分为3个阶段
     1. 捕获阶段
     2. 当前目标阶段
     3. 冒泡阶段

3. *如给一个div注册了点击事件：*

   ![](http://127.0.0.1:8888/uploads/202303011454888.png) 

   <img src="http://127.0.0.1:8888/uploads/202303011455940.png" style="zoom:80%;" />  

4. **注意**

   1. JS代码中只能执行 捕获 或者 冒泡其中的一个阶段
   2. onclick 和 attachEvent（ie）只能得到冒泡阶段
   3. `addEventListener(type,listener[,useCapture]`)第三个参数如果是**true**，表示再事件捕获阶段调用事件处理程序；如果是**false**（省略默认是false），表示再事件冒泡阶段调用事件处理程序
   4. **有些事件是没有冒泡的，比如onblue、onfocus、onmouseenter、onmouseleave**

## 事件捕获

**由DOM最顶层节点开始，然后逐级向下传播到具体的元素接收的过程**

1. 捕获阶段 如果`addEventListener`第三个参数是 true 那么则处于捕获阶段
2. 捕获阶段 document -> html -> body -> father -> son
   
   ![](http://127.0.0.1:8888/uploads/202303011504616.gif)

## 事件冒泡

**事件开始时由最具体的元素接收，然后逐级向上传播到DOM最顶层节点的过程**

1. 冒泡阶段 如果`addEventListener`第三个参数是 false 或者 省略 那么则处于冒泡阶段
2. 冒泡阶段 son -> father -> body -> html -> document![](http://127.0.0.1:8888/uploads/202303011511450.gif)

------

# 事件对象

## 事件对象基本使用

> 事件对象是事件的一系列相关数据的集合，跟事件相关的

1. 事件发生后，比如鼠标点击里面就包含了鼠标的相关信息，鼠标坐标等，如果是键盘事件里面就包含的键盘事件的信息，比如判断用户按下了哪个键
2. 事件对象只有有了事件才会存在，它是系统自动创建的，不需要传递参数
3. 事件对象命名一般为event、e、evt![](http://127.0.0.1:8888/uploads/202303011604468.gif)


## 事件对象的常见属性和方法

### 属性

| 事件对象属性方法    | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| e.target            | 返回**触发**事件的对象                                       |
| e.srcElement        | 返回**触发**事件的对象                                       |
| e.type              | 返回事件的类型 比如 click mouseover 不带on                   |
| e.cancelBubble      | 该属性阻止冒泡  非标准  ie6-8使用                            |
| e.returnValue       | 该属性 阻止默认事件（默认行为）非标准 ie6-8使用 比如不让链接跳转 |
| e.preventDefault()  | 该方法阻止默认事件（默认行为） 标准 比如不让链接跳转         |
| e.stopPropagation() | 阻止冒泡 标准                                                |

### e.target和this的区别

1. `e.target`：返回的是触发事件的对象（元素）
   
   -  e.target 指向点击的对象，如点击的是 li，e.target 指向的就是 li
   
2. `this`：返回的是绑定事件的对象（对象）
   - 如给ul绑定了事件，this 就指向 ul
   
   ![](http://127.0.0.1:8888/uploads/202303012001846.gif)

### 返回事件类型（e.type）

![](http://127.0.0.1:8888/uploads/202303012030246.gif) 

### 阻止默认行为preventDefault()

![](http://127.0.0.1:8888/uploads/202303012034976.gif)

### 阻止事件冒泡

标准写法：利用事件对象里面的`stopPropagation()`方法

![](http://127.0.0.1:8888/uploads/202303012038383.gif)

------

# 事件委托（代理、委托）

事件委托也称为事件代理，在jQuery里面称为事件委派

事件委托的原理：**不是每个节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用冒泡原理影响设置每个子节点**![](http://127.0.0.1:8888/uploads/202303012121396.gif)

# 鼠标事件对象

## 禁止鼠标（右键菜单、选中文字）

1. 禁止鼠标右键菜单：`contextmenu`主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下菜单
2. 禁止鼠标选中：`selectstart`
   
   ![](http://127.0.0.1:8888/uploads/202303012132759.gif)

## 鼠标事件对象

1. **event**对象代表事件的状态，跟事件相关的一系列信息的集合

2. 现阶段主要是用**鼠标事件对象（MouseEvent）**和**键盘事件对象（KeyboardEvent）**

   | 鼠标事件对象 | 说明                                         |
   | ------------ | -------------------------------------------- |
   | e.clientX    | 返回鼠标相对于浏览器窗口可视区的 X 坐标      |
   | e.clientY    | 返回鼠标相对于浏览器窗口可视区的 Y 坐标      |
   | e.pageX      | 返回鼠标相对于文档页面的 X 坐标 （IE9+支持） |
   | e.pageY      | 返回鼠标相对于文档页面的 Y 坐标 （IE9+支持） |
   | e.screenX    | 返回鼠标相对于电脑屏幕的 X 坐标              |
   | e.screenY    | 返回鼠标相对于电脑屏幕的 Y 坐标              |


## client鼠标在可视区的x和y坐标

![](http://127.0.0.1:8888/uploads/202303012342724.gif)

## page鼠标在页面文档的x和y坐标

![](http://127.0.0.1:8888/uploads/202303012352282.gif)

## screen鼠标在电脑屏幕的x和y坐标

![](http://127.0.0.1:8888/uploads/202303012355518.gif)

# 拖拽操作

## 概念

在拖拽操作中，源对象表示被拖动的元素。为元素添加draggable属性可以设置此元素为源对象

```html
<!-- 设置<p>标签的draggable属性的值为true-->
<p draggable="true"></p>
```

1. 图片和链接默认是可以拖动的，不用添加draggable属性就可以进行拖拽

2. 当源对象进入的元素称为目标对象，目标对象可以是页面的任一元素，而且目标对象不需要设置draggable属性

## 拖拽事件

拖拽事件包括拖拽开始、拖拽进行中、拖拽进行中等事件

1. 源对象事件

   | 事件        | 描述                       |
   | ----------- | -------------------------- |
   | ondragstart | 当拖拽开始时触发           |
   | ondrag      | 在拖拽元素被拖拽过程中触发 |
   | ondragend   | 当拖拽结束时触发           |

2. 目标对象事件

   | 事件        | 描述                                 |
   | ----------- | ------------------------------------ |
   | ondragenter | 当源对象进入目标对象时触发           |
   | ondragover  | 当源对象悬停在目标对象上方时触发     |
   | ondragleave | 当源对象离开目标对象时触发           |
   | ondrop      | 当源对象在目标对象上方释放鼠标是触发 |


# 移动端触屏事件

## touch事件

touch对象代表一个触摸点。触摸事件可响应用户手指（或触控笔）对屏幕或者触控板操作

| 事件        | 事件描述                                        |
| ----------- | ----------------------------------------------- |
| touchstart  | 当手指触摸屏幕时触发（触摸一次就触发一次）      |
| touchmove   | 当手指在屏幕上滑动时触发（持续触发）            |
| touchend    | 当手指离开屏幕时触发（离开一次就触发一次）      |
| touchcancel | 当系统取消touch事件时触发（如来电、弹出信息等） |

```javascript
window.onload=function(){
	// 获取DOM元素
	var div = document.querySelector('div');
	// 添加开始触摸事件：当手指触摸屏幕时触发
	div.addEventListener('touchstart',function(){
	    console.log('touchstart');

	// 添加手指滑动事件：当手指在屏幕上滑动时触发
	div.addEventListener('touchmove',function(){
	    console.log('touchmove');

	// 添加触摸结束事件：当手指离开屏幕时触发
	div.addEventListener('touchend',function(){
	    console.log('touchend');

	// 添加触摸意外中断事件
	div.addEventListener('touchcancel',function(){
	    console.log('touchcancel');
	});
};
```

## TouchEvent对象

> 当手指触摸移动设备时，有时会出现多个手指同时触摸屏幕的情况，此时称为多点触摸。每一个 touch 事件的触发都会产生一个TouchEvent对象，该对象中包含了3个用于跟踪触摸的属性，这些属性用于返回不同的触摸点集合

- TouchEvent对象的3个属性

  | 属性名         | 属性描述                                                     |
  | -------------- | ------------------------------------------------------------ |
  | touches        | 表示当前跟踪的触摸操作的touch对象的触摸点集合                |
  | targetTouches  | 表示当前对象上所有触摸点的集合                               |
  | changedTouches | 返回在上一次触摸和此次触摸之间状态发生变化的所有触摸对象的集合 |
