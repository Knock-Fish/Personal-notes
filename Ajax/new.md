# XMLHttpRequest

## 基本使用

- XMLHttpRequset（简称xhr）是浏览器提供的JavaScript对象，通过它，可以**请求服务器上的数据资源**。jQuery中的Ajax函数，是基于xhr对象封装出来的

### 使用xhr发起GET请求

1. 创建xhr对象

   ```javascript
   let xhr new XMLHTTpRequest()
   ```

2. 调用xhr.open()函数

   ```
   xhr.open('GET','URL地址')
   ```

3. 调用xhr.send()函数

   ```
   xhr.send()
   ```

4. 监听xhr.onreadystatechange事件

   ```
   xhr.onreadystatechange = function(){}
   ```

- 示例

  ```javascript
  // 1、创建 xhr 对象
  let xhr = new XMLHttpRequest()
  // 2、调用 open 函数，指定 请求方式 与 URL地址
  xhr.open('GET','URL地址')
  // 3、调用send函数，发起Ajax请求
  xhr.send()
  // 4、监听 onreadystatechange事件
  xhr.onreadystatechange = function(){
  	// 监听xhr对象的请求状态 readyState；与服务器响应的状态 status
  	if (xhr.readyState === 4 && xhr.status === 200){
  
  	}
  }
  ```

### 使用xhr发起POST请求

1. 创建xhr对象

   ```
   let xhr = new XMLHttpRequest()
   ```

2. 调用xhr.open()函数（固定属性）

   ```
   xhr.open('POST','URL地址')
   ```

3. **设置Content-Type属性**（固定写法）

   ```
   xhr.setRequsetHeader('Content-Type','application/x-www-form-urlencoded')
   ```

4. 调用xhr.send()函数，**同时指定要发送的数据**

5. 监听xhr.onreadystatechange事件

- 示例

  ```javascript
  // 1、创建 xhr 对象
  let xhr = new XMLHttpRequest()
  // 2、调用 open 函数
  xhr.open('POST','URL地址')
  // 3、设置 Content-Type 属性（固定写法）
  xhr.setRequsetHeader('Content-Type','application/x-www-form-urlencoded')
  // 4、调用send函数，同时将数据以查询字符串的形式，提交给服务器
  xhr.send('name=张三&age=20')
  // 5、监听 onreadystatechange事件
  xhr.onreadystatechange = function(){
  	if (xhr.readyState === 4 && xhr.status === 200){
  
  	}
  }
  ```

## xhr对象的readyState属性

- XMLHttpRequset对象的readyState属性，用来表示**当前Ajax请求所处的状态**。每个Ajax请求必然处于以下状态中的一个

  | 值   | 状态             | 描述                                           |
  | ---- | ---------------- | ---------------------------------------------- |
  | 0    | unsent           | XMLHttpRequst对象已被创建，但尚未调用open方法  |
  | 1    | opened           | open()方法已经被调用                           |
  | 2    | headers_received | send()方法已经被调用，响应头也已经被接收       |
  | 3    | loading          | 数据接收中，此时response属性中已经包含部分数据 |

## 使用xhr发起带参数的GET请求

- 使用xhr对象发起带参数的GET请求时，只需在调用xhr.open期间，为URL地址指定参数即可

  ```
  xhr.open('GET','URL地址?id=1&name=xxx')
  ```

  - 这种在URL地址后面拼接的参数，叫做**查询字符串**

## 查询字符串

- 定义：查询字符串（URL参数）是指在URL的末尾加上用于向服务器发送信息的字符串（变量）
- 格式：将英文的 **?** 放在URL的末尾，然后再加上 参数=值，如想加上多个参数，使用 **&** 符号进行分隔。以这个形式，可以将想要发送给服务器的数据添加到URL中

- url地址示例

  ```
  // 不带参数的URL地址
  URL地址
  // 带一个参数的URL地址
  URL地址?id=1
  // 带两个参数的URL地址
  URL地址?id=1&name=xxx
  ```

## GET请求携带参数的本质

- 无论使用\$.ajax()，还是使用\$.get()，又或者直接使用 xhr 对象发起 GET 请求，当需要携带参数的时候，本质上，都是直接将参数以查询字符串的形式，追加到URL地址的后面，发送到服务器的

- 示例

  ```
  $.get('url',{name:'zs',age:20},function(){})
  // 等价于
  $.get('url?name=zs&age=20',function(){})
  
  $.ajax({method:'GET', url:'url', data:{name:'zs',age:20},success:function(){}})
  // 等价于
  $.ajax({method:'GET',url:'url?name=zs&age=20',success:function(){}})
  ```

## URL编码与解码

- URL地址中，只允许出现英文相关的字母、标点符号、数字，因此，在URL地址中不允许出现中文字符（或其他字符）

- 如果URL中需要包含中文这样的字符，则必须对中文字符进行**编码**

- **URL编码的原则**：使用安全的字符（没有特殊用途或者特殊意义的可打印字符）

  - 通俗理解：使用英文字符去表示非英文字符

    ```
    URL地址?id=1&bookname=西游记
    // 经过URL编码之后，URL地址变成了如下格式
    URL地址?id=1&bookname=%E8%A5%BF%E6%B8%B8%E8%AE%B0
    
    // %E8%A5%BF 对应 “西”
    // %E6%B8%B8 对应 “游”
    // %E8%AE%B0 对应 “记”
    ```

- 对URL进行编码与解码

  - 浏览器提供了URL编码与解码的API，分别是

    | 函数        | 作用 |
    | ----------- | ---- |
    | encodeURL() | 编码 |
    | decodeURL() | 解码 |

    - URL编码与解码示例

      ```
      console.log(encodeURI('张三'))  					// %E5%BC%A0%E4%B8%89
      console.log(decodeURI('%E5%BC%A0%E4%B8%89'))     // 张三
      ```

# 数据交换格式

- 数据交换格式，就是服务端与客户端之间进行数据传输与交换的格式
- 有两种数据交换格式分别是XML和JSON

##  XML

- XML的英文全称是EXtensible Markup Language，即可扩展标记语言。

- XML和HTML类似，也是一种标记语言

  - HTML

    ```html
    <!DOCTYPE html>
    <html>
    	<head>
            <tiile>Document</tiile>
        </head>
        <body></body>
    </html>
    ```

  - XML

    ```xml
    <note>
        <to>ls</to>
        <from>zs</from>
        <heading>通知</heading>
        <body>晚上开会</body>
    </note>
    ```

  - XML的缺点

    - XML格式臃肿，和数据无关的代码多，体积大，传输效率低
    - 在JavaScript中解析XML比较麻烦

- XML和HTML的区别

  - HTML被设计用来描述网页上的内容，是网页内容的载体
  - XML被设计用来传输和存储数据，是数据的载体

## JSON

1. JSON的英文全称是JavaScript Object Notation，即 “JavaScript对象表示法”。

2. **JSON是JavaScript对象和数组的字符串表示法**，它使用文本表示一个JS对象或数组的信息，因此，**JSON的本质是字符串**

3. 作用：JSON是一种**轻量级的文本数据交换格式**，在作用上类似于XML，专门用于存储和传输数据，但是JSON比XML**更小、更快、更易解析**

   - 在计算机与网络之间存储和传输数据

4. JSON的两种结构

   - JSON就是用字符串来表示JavaScript的对象和数组。所以，JSON中包含**对象**和**数组**两种结构，通过这两种结构的**相互嵌套**，可以表示各种复杂的数据结构
     - **对象结构**：对象结构在JSON中表示为 { } 括起开的内容。数据结构为`{key:value, key:value,...}`的键值对结构。其中，key必须是使用**英文的双引号包裹**的字符串，value的数据类型可以是**数字、字符串、布尔值，null、数组、对象**6种类型
     - **数组结构**：数组结构在JSON中表示为 [ ] 括起来的内容。数据结构为[ "java","javascript",30,true...]。数组中数据的类型可以是**数字、字符串、布尔值、null、数组、对象**6种类型

5. JSON语法注意事项

   1. 属性名必须使用双引号包裹
   2. 字符串类型的值必须使用双引号包裹
   3. JSON中不允许使用单引号表示字符串
   4. JSON中不能写注释
   5. JSON的最外层必须是对象或数组格式
   6. 不能使用undefined或函数作为JSON的值

6. JSON和JS对象的关系

   - JSON是JS对象的字符串表示法，它使用文本表示一个JS对象信息，本质是一个字符串

     ```json
     // 对象
     let obj = {a:'Hello', b:'World'}
     
     // 这是一个JSON字符串，本质是一个字符串
     let json = '{"a":"Hello","b":"World"}'
     ```

7. JSON 和 JS 对象的互转

   - 从JSON字符串转换为 JS 对象，使用<span style="color:red;"> JSON.parse()</span>方法

     ```
     let obj =JSON.parse('{"a":"Hello","b":"World"}')
     // 结果是 {a: 'Hello', b: 'World'}
     ```

   - 从JS对象转换为JSON字符串，使用<span style="color:red;">JSON.stringify()</span>方法

     ```
     let json = JSON.stringify({a: 'Hello', b: 'World'})
     // 结果为 '{"a": "Hello", "b": "World"}'
     ```

8. 序列化和反序列化

   - 把**数据对象转换为字符串**的过程，叫做**序列化**。例如：调用JSON.stringify()函数操作，叫做JSON序列化
   - 把**字符串转换为数据对象**的过程，叫做**反序列化**。例如：调用JSON.parse()函数的操作，叫做JSON反序列化

# XMLHttpRequest Level2的新特性

## 设置HTTP请求时限

- 有时，Ajax操作很耗时，而且无法预知要花多少时间。如果网速很慢，用户可能要等很久。新版本的XMLHttpRequest 对象，增加了 timeout 属性，可以设置 HTTP 请求的时限：

  - 语法

    ```javascript
    xhr.timeout = 毫秒数；
    ```

    - timeout设置时限，过了这个时限，就自动停止HTTP请求。

    - timeout事件，用来指定回调函数：

      ```javascript
      xhr.ontimeout = function(event) {
      	alert('请求超时');
      }
      ```


## FormData对象过来表单数据

- Ajax操作往往用来提交表单数据。为了方便表单处理，HTML5新增了一个 FormData对象，可以模拟表单操作：

  ```
  let fd = new FormData()
  ```

- 示例

  ```javascript
  // 1、新增 FormData对象
  let fd = new FormData()
  // 2、为 FormData 添加表单项
  fd.append('uname', 'zs')
  fd.append('upwd', '1234556')
  // 3、创建 xhr 对象
  let xhr = new XMLHttpRequest()
  // 4、指定请求类型与URL地址
  xhr.open('POST', 'URL地址')
  
  //设置 Content-Type 属性（如果设置请求头数据会被加密，会看不见）
  // xhr.setRequsetHeader('Content-Type','application/x-www-form-urlencoded')
  
  // 5、直接提交 FormData 对象，这与提交网页表单的效果，完全一样
  xhr.send(fd)
  // 6、xhr.onreadystatechange事件
  xhr.onreadstatechange = function(){
      if(xhr.readyState === 4 && xhr.status === 200){
          console.log(JSON.parse(xhr.responseText))
      }
  }
  ```

### 获取网页表单的值

- 示例

  ```javascript
  // 获取form表单
  let form = document.querySelector('form')
  // 监听表单元素的 submit 事件
  form.addEvenListener('submit', function(e) {
      e.preventDefault()	// 阻止默认行为
      // 根据 form 表单创建 FormData 对象，会自动将表单数据填充到 FormData 对象中
      let fd = new FormDate(form)
      let xhr = new XMLHttpRequest()
      xhr.open('POST', 'URL地址')
      xhr.send(fd)
      xhr.onreadystatechange = function(){
          if(xhr.readyState === 4 && xhr.status === 200){
              console.log(JSON.parse(xhr.responseText))
          }
      }
  })
  ```

## 上传文件

- 使用XMLHttpRequest对象上传文件

- 实现步骤

  1. 定义UI 结构

     ```html
     <!-- 文件选择框 -->
     <input type="file" id="file1"/>
     <!-- 上传按钮 -->
     <button id="btnUpload">上传文件</button>
     <!-- 显示上传到服务器上的图片 -->
     <img src="" alt="" id="img" width="800">
     ```

  2. 验证是否选择了文件

  3. 向FormData中追加文件

  4. 使用 xhr 发起上传文件的请求

  5. 监听 onreadystatechange事件

## 显示文件上传进度

- 可以通过监听xhr.upload.onprogress事件，来获取文件的上传进度

  - 语法格式如下（代码需要放在opan之前）

    ```javascript
    let xhr = new XMLHttpRequest()
    // 监听 xhr.upload 的 onprogress 事件
    xhr.upload.onprogress = function(e){
        // e.lengthComputable 是一个布尔值，表示当前上传的资源是否具有可计算的长度
        if(e.lengthComputable){
            // e.loaded 已传输的字节
            // e.total 需传输的总字节
            let percentComplete = Math.celi((e.loaded / e.total) * 100)
        }
    }
    ```

# 同源策略和跨域

## 同源策略

- 什么是同源
  - 如果两个页面的协议，域名和端口都相同，则两个页面具有相同的源

- 什么是同源策略
  - **同源策略**（英文全称Same origin policy）是浏览器提供的一个安全功能
  - MDN官方给定的概念：同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制
    - 浏览器规定，A网站的JavaScript，不允许和非同源的网站 C 之间，进行资源的交互，例如：
      1. 无法读取非同源网页的Cookie、LocalStorage 和 IndexedDB
      2. 无法接触非同源网页的 DOM
      3. 无法向非同源地址发送Ajax请求

## 跨域

- 什么是跨域
  - 同源指的是两个 URL的协议、域名、端口一致，反之，则是跨域
  - 出现跨域的根本原因：浏览器的同源策略不允许非同源的 URL 之间进行资源的交互
- 浏览器对跨域请求的拦截
  - 浏览器允许发起跨域请求，但是，跨域请求回来的数据，会被浏览器拦截，无法被页面获取到![](http://127.0.0.1:8888/uploads/202305071930976.png)

- 如何实现跨域数据
  - 实现跨域数据请求，最主要的两种解决方案，分别是 JSONP 和 CORS
  - JSONP：缺点是只支持GET请求，不支持POST请求（兼容低版本IE）
  - CORS：属于跨域Ajax请求的根本解决方案。支持GET 和 POST 请求。缺点是不兼容某些低版本的浏览器

# JSONP

> JSONP（JSON with Padding）是JSON的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题

1. JSONP的实现原理

   - 由于<span style="color:red">**浏览器同源策略**</span>的限制，网页中<span style="color:red">**无法通过 Ajax 请求非同源的接口数据**</span>。但是\<script>标签不受浏览器同源策略的影响，可以通过 src 属性，请求非同源的 js脚本

   - 因此，JSONP的实现原理，就是通过\<script>标签的 src 属性，请求跨域的数据接口，并通过<span style="color:red">**函数调用**</span>的形式，接收跨域接口响应回来的数据

2. 实现简单的JSONP

   - 示例

     - HTML部分

       ```html
       <script>
           function f(data) {
               console.log(data)
           }
       </script>
       // 可以通过查询字符串的形式指定回调函数
       <script src="./appjs/f.js?callback=f"></script>
       ```

     - appjs目录下的 f.js

       ```
       f({ name: "张三" ,age: "18"})
       ```

3. JSONP的缺点

   - 由于JSONP是通过\<script>标签的src属性，来实现跨域数据获取的，所以，JSONP只支持GET数据请求，不支持 POST请求

4. 注意

   - <span style="color:red">**JSONP 和 Ajax 之间没有任何关系**</span>，不能把JSONP请求数据的方式叫做Ajax，因为JSONP没有用到XMLHttpRequest这个对象

# HTTP协议简介

## 通信协议

- <span style="color:red">**通信协议**</span>（Communication Protocol）是指通信的双方完成通信所<span style="color:red">必须遵守</span>的<span style="color:blue">规则和约定</span>
  - 通俗的理解：通信双方采用约定好的格式来发送和接受信息，这种<span style="color:red">事先约定好的通信格式，就叫做通信协议</span>
  - 客户端与服务器之间要实现<span style="color:red">网页内容</span>的传输，则通信的双方必须遵守<span style="color:red">网页内容的传输协议</span>
    - <span style="color:blue">网页内容</span>又叫做<span style="color:red">**超文本**</span>，因此<span style="color:blue">网页内容的传输协议</span>又叫做<span style="color:red">**超文本传输协议**</span>（HyperTest Transfer Protocol），简称<span style="color:red">**HTTP协议**</span>

## HTTP协议

- <span style="color:red">**HTTP协议**</span>即超文本传送协议（**H**yper**T**est **T**ransfer **P**rotocol），它规定了客户端与服务器之间进行网页内容传输时，所必须遵守的传输格式
  - 例如
    - <span style="color:blue">客户端</span>要以HTTP协议要求的格式把数据<span style="color:red">提交</span>到<span style="color:blue">服务器</span>
    - <span style="color:blue">服务器</span>要以HTTP协议要求的格式把内容<span style="color:red">响应</span>给<span style="color:blue">客户端</span>

## HTTP请求消息

- 由于HTTP协议属于客户端浏览器和服务器之间的通信协议。因此，客户端发起的请求叫做**HTTP请求**，客户端发送到服务器的消息，叫做**HTTP请求消息**
  - 注意：HTTP**请求消息**又叫做HTTP**请求报文**

### HTTP请求消息的组成部分

#### 请求行

- <span style="color:red">**请求行**</span>由<span style="color:blue">请求方式、URL和HTTP协议版本</span>3个部分组成，他们之间使用空格隔开

#### 请求头

- 请求头部用来描述客户端的基本信息，从而把客户端相关的信息告知服务器

  - 比如
    - User-Agent 用来说明当前是什么类型的浏览器
    - Content-type 用来描述发送到服务器的数据格式
    - Accept 用来描述客户端能够接收什么类型的返回内容
    - Accept-Language用来描述客户端期望接收哪种人类语言的文本内容

  - 请求头部由多行 **键 / 值对** 组成，每行的键和值之间用英文的冒号分隔

- 请求头部 - 常见的请求头字段

  | 头部字段        | 说明                                           |
  | --------------- | ---------------------------------------------- |
  | Host            | 要请求的服务器域名                             |
  | Connection      | 客户端与服务器的连接方式（close 或 keepalive） |
  | Content-Length  | 用来描述请求体的大小                           |
  | Accept          | 客户端可识别的响应内容类型列表                 |
  | User-Agent      | 产生请求的浏览器类型                           |
  | Content-Type    | 客户端告诉服务器事迹发送的数据类型             |
  | Accept-Encoding | 客户端可接收的内容压缩编码形式                 |
  | Accept-Language | 用户期望获得的自然语言的优先顺序               |

- 空行
  - 最后一个请求字段的后面是一个**空行**，通知服务器**请求头部至此结束**
  - 请求消息中的空行，用来分隔**请求头部**与**请求体**

- 请求体
  - 请求体中存放的，是要通过POST方式提交到服务器的数据
  - 只有POST请求才有请求体，GET请求没有请求体
  - get方式的请求参数携带在URL中，而URL在请求行中

## HTTP响应信息

- 响应信息就是服务器响应给客户端的消息内容，也叫响应报文

### HTTP响应消息的组成部分

- HTTP响应信息由状态行、响应头部、空行和响应体 4 个部分组成

#### 状态行

- 状态行由 HTTP协议版本、状态码和状态码的描述文本 3 个部分组成，之间使用空格隔开

#### 响应头部

- **响应头部**用来描述**服务器的基本信息**。响应头部由多行 **键/ 值对** 组成，每行的键和值之间用英文的冒号分隔

#### 空行

- 在最后一个响应头部字段结束之后，会紧跟一个**空行**，用来通知客户端** **
- 响应消息中的空行，用来分隔**响应头部**与**响应体**

#### 响应体

- 响应体中存放的，是服务器响应给客户端的资源内容

## HTTP请求方法

- HTTP请求方法，属于 HTTP协议中的一部分，请求方法的作用：用来表明**要对服务器上的资源执行的操作**
  - 最常用的请求方法是 GET 和 POST

- 方法

  | 方法    | 描述                                                         |
  | ------- | ------------------------------------------------------------ |
  | GET     | （查询）发送请求来获取服务器上的资源，请求体中不会包含请求数据，请求数据放在协议中 |
  | POST    | （新增）向服务器提交资源（例如提交表单或上传文件）。数据被包含在请求体中提交给服务器 |
  | PUT     | （修改）向服务器提交资源，并使用提交的新资源，替换掉服务器对应的旧资源 |
  | DELETE  | （删除）请求服务器删除指定的资源                             |
  | HEAD    | HEAD方法请求一个与 GET 请求的响应相同的响应，但没有响应体    |
  | OPTIONS | 获取http服务器支持的http请求方法，允许客户端查看服务器的性能，比如ajax跨域时的预检等 |
  | CONNECT | 建立一个到由目标资源的标识的服务器的隧道                     |
  | TRACE   | 沿着到目标资源的路径执行一个消息环回测试，主要用于测试或诊断 |
  | PATCH   | 是对PUT方法的补充，用来对已知资源进行局部更新                |

## HTTP响应状态码

- **HTTP响应状态码**（HTTP Status Code），也属于HTTP协议的一部分，**用来标识响应的状态**
- 响应状态码会随着响应信息一起被发送至客户端浏览器，浏览器根据服务器返回的响应状态码，就能知道这次HTTP请求的结果是成功还是失败了

### HTTP响应状态码的组成及分类

- HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字用来对状态码进行细分。

  - HTTP状态码共分为 5 种类型

    | 分类 | 分类描述                                                     |
    | ---- | ------------------------------------------------------------ |
    | 1**  | **信息**，服务器收到请求，需要请求者继续执行操作 <br>（实际开发中很少遇到 1** 类型的状态码） |
    | 2**  | **成功**，操作被成功接收并处理                               |
    | 3**  | **重定向**，需要进一步的操作以完成请求                       |
    | 4**  | **客户端错误**，请求包含语法错误或无法完成请求               |
    | 5**  | **服务器错误**，服务器在处理请求的过程中发生了错误           |

#### <span style="color:red">2** 成功</span>相关的响应状态码

- 2** 范围的状态码，表示服务器已成功接收到请求并进行处理

- 常见的 2** 类型的状态码如下

  | 状态码 | 状态码英文名称 | 中文描述                                                     |
  | ------ | -------------- | ------------------------------------------------------------ |
  | 200    | OK             | **请求成功**。一般用于 GET 与 POST 请求                      |
  | 201    | Created        | **已创建**。成功请求并创建了新的资源，通常用于 POST 或 PUT 请求 |

#### <span style="color:red">3** 重定向</span>相关的响应状态码

- 3** 范围的状态码，表示表示服务器要求客户端重定向，需要客户端进一步的操作已完成资源的请求

- 常见的 3** 类型的状态码如下

  | 状态码 | 状态码英文名称    | 中文描述                                                     |
  | ------ | ----------------- | ------------------------------------------------------------ |
  | 301    | Moved Permanently | **永久移动**。请求的资源已被永久的移动到新URL，返回信息会包括新的URL，浏览器会自动定向到新URL。今后任何新的请求都应使用新的URL代替 |
  | 302    | Found             | **临时移动**。与301类似。但资源只是临时被移动。客户端应继续使用原有URL |
  | 304    | Not Modified      | **未修改**。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源（响应报文中不包含响应体）。客户端通常会缓存访问过的资源 |

#### <span style="color:red">4** 客户端错误</span>相关的响应状态码

- 4** 范围的状态码，表示客户端的请求有非法内容，从而导致这次请求失败

- 常见的 4** 类型的状态码如下：

  | 状态码 | 状态码英文名称  | 中文描述                                                     |
  | ------ | --------------- | ------------------------------------------------------------ |
  | 400    | Bad Request     | 1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求<br>2、请求参数有误 |
  | 401    | Unauthorized    | 当前请求响应用户验证                                         |
  | 403    | Forbiden        | 服务器已经理解请求，但是拒绝执行它                           |
  | 404    | Not Found       | 服务器无法根据客户端的请求找到资源（网页）                   |
  | 408    | Request Timeout | 请求超时。服务器等待客户端发送的请求时间过长，超时           |

#### <span style="color:red">5** 服务端错误</span>相关的响应状态码

- 5** 范围的状态码，表示服务器未能正常处理客户端的请求而出现意外错误

- 常见的 5** 类型的状态码如下

  | 状态码 | 状态码英文名称        | 中文描述                                                     |
  | ------ | --------------------- | ------------------------------------------------------------ |
  | 500    | Internal Server Error | 服务器内部错误，无法完成请求                                 |
  | 501    | Not Implemented       | 服务器不支持该请求方法，无法完成请求。只有 GET 和 HEAD 请求方法是要求每个服务器必须支持的，其它请求方法在不支持的服务器上会返回501 |
  | 503    | Service Unavailable   | 由于超载或系统维护，服务器暂时的无法处理客户端的请求         |
