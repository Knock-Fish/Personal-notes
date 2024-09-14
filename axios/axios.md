# token

# fetch

# axios简介

## 简介

- axios是基于 Promise 的网络请求库，可以在浏览器 或 node.js环境当中去运行
  - 在浏览器（客户端）中，可以借助 axios 向服务端发送 Ajax 请求获取数据
  - 在 Node.js 中，可以借助 axios 向远端服务发送 HTTP 请求
- axios特点
  - 可以在浏览器当中创建 XMLHttpRequests
  - 可以在 Node.js 当中创建HTTP请求
  - 支持 Promise API
  - 拦截请求和响应（在请求之前做预备工作，在响应回来的结果作预处理）
  - 转换请求和响应数据
  - 取消请求
  - 超时处理
  - 自动将请求体序列化为：
    - JSON (`application/json`)
    - Multipart / FormData (`multipart/form-data`)
    - URL encoded form (`application/x-www-form-urlencoded`)
  - 将 HTML Form 转换成 JSON 进行请求
  - 自动转换JSON数据
  - 获取浏览器和 node.js 的请求进度，并提供额外的信息（速度、剩余时间）
  - 查询参数序列化支持嵌套项处理
  - 客户端支持针对 XSRF 进行保护
  - 为 node.js 设置带宽限制
  - 兼容符合规范的 FormData 和 Blob（包括 node.js）

## 安装

- 方式

  - npm（在项目当中使用的安装方式）

    ```
    npm install axios
    ```

  - yarn（在项目当中使用的安装方式）

    ```
    yarn add axios
    ```

  - 使用 bower

    ```
    bower install axios
    ```

  - 使用 jsDelivr CDN

    ```
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    ```

  - 使用 unpkg CDN

    ```
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    ```

- 也可以CDN方式引入

- 可以在如下的国内的服务器上访问（更快）

  - [[axios (v1.3.6) - Axios 是一个基于 promise 的 HTTP 库,可以用在浏览器和 Node.js 中。 | BootCDN - Bootstrap 中文网开源项目免费 CDN 加速服务](https://www.bootcdn.cn/axios/)](https://www.bootcdn.cn/)

# 请求配置对象（config）

- 创建请求时可以用的配置选项。只有 `url` 是必需的。如果没有指定 `method`，请求将默认使用 `GET` 方法

  - 配置对象

    ```
    {
      // url 是用于请求的服务器 URL
      url: '/user',
    
      // `method` 是创建请求时使用的方法
      method: 'get', // 默认值
    
      // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
      // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
      baseURL: 'https://some-domain.com/api/',
    
      // `transformRequest` 允许在向服务器发送前，修改请求数据
      // 它只能用于 'PUT', 'POST' 和 'PATCH' 这几个请求方法
      // 数组中最后一个函数必须返回一个字符串， 一个Buffer实例，ArrayBuffer，FormData，或 Stream
      // 你可以修改请求头。
      transformRequest: [function (data, headers) {
        // 对发送的 data 进行任意转换处理
    
        return data;
      }],
    
      // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
      transformResponse: [function (data) {
        // 对接收的 data 进行任意转换处理
    
        return data;
      }],
    
      // 自定义请求头
      headers: {'X-Requested-With': 'XMLHttpRequest'},
    
      // `params` 是与请求一起发送的 URL 参数（查询字符串）
      // 必须是一个简单对象或 URLSearchParams 对象
      params: {
        ID: 12345
      },
    
      // `paramsSerializer`是可选方法，主要用于序列化`params`
      // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
      paramsSerializer: function (params) {
        return Qs.stringify(params, {arrayFormat: 'brackets'})
      },
    
      // `data` 是作为请求体被发送的数据
      // 仅适用 'PUT', 'POST', 'DELETE 和 'PATCH' 请求方法
      // 在没有设置 `transformRequest` 时，则必须是以下类型之一:
      // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
      // - 浏览器专属: FormData, File, Blob
      // - Node 专属: Stream, Buffer
      data: {
        firstName: 'Fred'
      },
      
      // 发送请求体数据的可选语法
      // 请求方式 post
      // 只有 value 会被发送，key 则不会
      data: 'Country=Brasil&City=Belo Horizonte',
    
      // `timeout` 指定请求超时的毫秒数。
      // 如果请求时间超过 `timeout` 的值，则请求会被中断
      timeout: 1000, // 默认值是 `0` (永不超时)
    
      // `withCredentials` 表示跨域请求时是否需要使用凭证
      withCredentials: false, // default
    
      // `adapter` 允许自定义处理请求，这使测试更加容易。
      // 返回一个 promise 并提供一个有效的响应 （参见 lib/adapters/README.md）。
      adapter: function (config) {
        /* ... */
      },
    
      // `auth` HTTP Basic Auth
      auth: {
        username: 'janedoe',
        password: 's00pers3cret'
      },
    
      // `responseType` 表示浏览器将要响应的数据类型
      // 选项包括: 'arraybuffer', 'document', 'json', 'text', 'stream'
      // 浏览器专属：'blob'
      responseType: 'json', // 默认值
    
      // `responseEncoding` 表示用于解码响应的编码 (Node.js 专属)
      // 注意：忽略 `responseType` 的值为 'stream'，或者是客户端请求
      // Note: Ignored for `responseType` of 'stream' or client-side requests
      responseEncoding: 'utf8', // 默认值
    
      // `xsrfCookieName` 是 xsrf token 的值，被用作 cookie 的名称
      xsrfCookieName: 'XSRF-TOKEN', // 默认值
    
      // `xsrfHeaderName` 是带有 xsrf token 值的http 请求头名称
      xsrfHeaderName: 'X-XSRF-TOKEN', // 默认值
    
      // `onUploadProgress` 允许为上传处理进度事件
      // 浏览器专属
      onUploadProgress: function (progressEvent) {
        // 处理原生进度事件
      },
    
      // `onDownloadProgress` 允许为下载处理进度事件
      // 浏览器专属
      onDownloadProgress: function (progressEvent) {
        // 处理原生进度事件
      },
    
      // `maxContentLength` 定义了node.js中允许的HTTP响应内容的最大字节数
      maxContentLength: 2000,
    
      // `maxBodyLength`（仅Node）定义允许的http请求内容的最大字节数
      maxBodyLength: 2000,
    
      // `validateStatus` 定义了对于给定的 HTTP状态码是 resolve 还是 reject promise。
      // 如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，
      // 则promise 将会 resolved，否则是 rejected。
      validateStatus: function (status) {
        return status >= 200 && status < 300; // 默认值
      },
    
      // `maxRedirects` 定义了在node.js中要遵循的最大重定向数。
      // 如果设置为0，则不会进行重定向
      maxRedirects: 5, // 默认值
    
      // `socketPath` 定义了在node.js中使用的UNIX套接字。
      // e.g. '/var/run/docker.sock' 发送请求到 docker 守护进程。
      // 只能指定 `socketPath` 或 `proxy` 。
      // 若都指定，这使用 `socketPath` 。
      socketPath: null, // default
    
      // `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
      // and https requests, respectively, in node.js. This allows options to be added like
      // `keepAlive` that are not enabled by default.
      httpAgent: new http.Agent({ keepAlive: true }),
      httpsAgent: new https.Agent({ keepAlive: true }),
    
      // `proxy` 定义了代理服务器的主机名，端口和协议。
      // 您可以使用常规的`http_proxy` 和 `https_proxy` 环境变量。
      // 使用 `false` 可以禁用代理功能，同时环境变量也会被忽略。
      // `auth`表示应使用HTTP Basic auth连接到代理，并且提供凭据。
      // 这将设置一个 `Proxy-Authorization` 请求头，它会覆盖 `headers` 中已存在的自定义 `Proxy-Authorization` 请求头。
      // 如果代理服务器使用 HTTPS，则必须设置 protocol 为`https`
      proxy: {
        protocol: 'https',
        host: '127.0.0.1',
        port: 9000,
        auth: {
          username: 'mikeymike',
          password: 'rapunz3l'
        }
      },
    
      // see https://axios-http.com/zh/docs/cancellation
      cancelToken: new CancelToken(function (cancel) {
      }),
    
      // `decompress` indicates whether or not the response body should be decompressed 
      // automatically. If set to `true` will also remove the 'content-encoding' header 
      // from the responses objects of all decompressed responses
      // - Node only (XHR cannot turn off decompression)
      decompress: true // 默认值
    }
    ```

# axios API

## axios()

> 可以向 axios 传递相关配置来创建请求

- 语法

  ```
  axios(config)
  axios(url[,config])
  ```

- 示例

  - 浏览器中

    ```javascript
    // 发送post请求
    axios({
        method: 'post',
        url: 'http://127.0.0.1/user',
        data:{
            name: '张三',
        	age: '18'
        }
    });
    ```

  - node.js中

    ```javascript
    // 在 node.js 用GET请求获取远程图片
    axios({
      method: 'get',
      url: 'http://127.0.0.1/img',
      responseType: 'stream'
    })
      .then(function (response) {
        response.data.pipe(fs.createWriteStream('123.jpg'))
      });
    ```

  - 默认请求方式

    ```
    // 发起一个 GET 请求 (默认请求方式)
    axios('/user/12345');
    ```

## 请求方式别名

> 为了方便起见，已经为所有支持的请求方法提供了别名
>
> 在使用别名方法时`url`、`method`、`data` 这些属性都不必在配置中指定

### axios.request()

> axios.request 和 axios() 使用一样

- 语法

  ```javascript
  axios.request(config)
  ```

- 示例

  ```javascript
   axios.request({
       // 请求类型
       method:'GET',
       // URL
       url:'http://localhost:3000/goods/1'
   }).then(response => {       // 设置成功的回调
       console.log(response);  
   })
  ```

### axios.post()

> `url`、`method`、`data` 这些属性都不必在配置中指定

- 语法

  ```
  axios.post(url[, data[, config]])
  ```

- 示例

  ```javascript
   axios.post(
       'http://localhost:3000/goods',
       {
           "name": "商品",
           "price": "$8.99"
       }).then(response => {
       console.log(response)
   })
  ```

### axios.get()

- 语法

  ```
  axios.get(url[, config])
  ```

- 示例

  ```javascript
  axios.get(
      'http://localhost:3000/books/1'
  ).then(response => {
      console.log(response)
  })
  ```

### axios.delete()

- 语法

  ```
  axios.delete(url[,config])
  ```

- 示例

  ```javascript
  axios.delete(
      'http://localhost:3000/goods/4'
  ).then(response => {
      console.log(response)
  })
  ```

### axios.head()

- 语法

  ```
  axios.head(url[,config])
  ```

### axios.options()

- 语法

  ```
  axios.options(url[, config])
  ```

### axios.put()

- 语法

  ```
  axios.put(url[, data[, config]])
  ```

### axios.patch()

- 语法

  ```
  axios.patch(url[, data[, config]])
  ```

### axios.postForm()

- 语法

  ```
  axios.postForm(url[, data[, config]])
  ```

### axios.putForm()

- 语法

  ```
  axios.putForm(url[, data[, config]])
  ```

### axios.patchForm()

- 语法

  ```
  axios.patchForm(url[, data[, config]])
  ```

# 响应结构

- 响应结果的data为什么是对象
  - axios自动将服务器返回结果进行JSON解析，把结果转成一个对象

- 一个请求的响应包含以下信息

  ```
  {
    // `data` 由服务器提供的响应
    data: {},
  
    // `status` 来自服务器响应的 HTTP 状态码
    status: 200,
  
    // `statusText` 来自服务器响应的 HTTP 状态信息
    statusText: 'OK',
  
    // `headers` 是服务器响应头
    // 所有的 header 名称都是小写，而且可以使用方括号语法访问
    // 例如: `response.headers['content-type']`
    headers: {},
  
    // `config` 是 `axios` 请求的配置信息
    config: {},
  
    // `request` 是生成此响应的请求
    // 在node.js中它是最后一个ClientRequest实例 (in redirects)，
    // 在浏览器中则是 XMLHttpRequest 实例
    request: {}
  }
  ```

  - 当使用 `then` 时，将接收如下响应:

    ```
    axios.get('/user/12345')
      .then(function (response) {
        console.log(response.data);
        console.log(response.status);
        console.log(response.statusText);
        console.log(response.headers);
        console.log(response.config);
      });
    ```

  - 当使用 `catch`，或者传递一个[rejection callback](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)作为 `then` 的第二个参数时，响应可以通过 `error` 对象被使用，正如在[错误处理](https://axios-http.com/zh/docs/handling_errors)部分解释的那样

# 默认配置

- 示例

  - HTML

    ```html
    <button>发送GET类型请求</button>
    ```

  - axios

    ```javascript
     let btn = document.querySelector('button');
    // 默认配置
    axios.defaults.method = 'GET';   //设置默认的请求类型为 GET
    axios.defaults.baseURL = 'http://localhost:3000';   // 设置基础URL
    axios.defaults.timeout = 3000;
    btn.onclick = function(){
        axios({
            url:'/books'
        }).then(response => {
            console.log(response)
        })
    }
    ```

# 创建axios实例对象

> 相当于axios二次封装

- 使用自定义配置新建一个实例

  - 语法
  
    ```
    axios.create([config])
    ```
  
- 示例

  ```javascript
  // 创建实例对象
  let aObj = axios.create({
      baseURL: 'http://localhost:3000',
      timeout: 2000
  });
  // 可以创建多个实例对象
  let bObj = axios.create({
      baseURL: 'http://localhost:8080',
      timeout: 2000
  });
  
  // 这里 aObj 与 axios对象的功能几近是一样的
  /* aObj({
      url: 'books',
  }).then(response => {
      console.log(response);
  }) */
  
  aObj.get('/books').then(response => {
      console.log(response.data)
  })
  ```

## 实例方法

- 以下是可用的实例方法。指定的配置将与实例的配置合并
  - axios#request(config)
  - axios#get(url[, config])
  - axios#delete(url[, config])
  - axios#head(url[, config])
  - axios#options(url[, config])
  - axios#post(url[, data[, config]])
  - axios#put(url[, data[, config]])
  - axios#patch(url[, data[, config]])
  - axios#getUri([config])

# 拦截器

> 在请求或响应被 then 或 catch 处理前拦截

## 添加拦截器

- 请求拦截器：在请求发送之前，可以借助回调，对请求的参数、内容作处理和检测
- 响应拦截器：当服务器响应结果的时候，可以响应处理结果之前，先对响应结果作预处理
- use原理是由Promise 的then方法对成功和失败作指定

  - 示例

    ```javascript
     // 设置拦截器
    axios.interceptors.request.use(function (config) {
        // 发送之前处理
        console.log('请求拦截器 成功');
        return config;
    }, function (error) {
        // 对请求错误处理
        console.log('请求拦截器 失败');
        return Promise.reject(error);
    });
    
    // 设置响应拦截器
    axios.interceptors.response.use(function(response) {
        // 2** 范围内的状态码都会触发该函数
        // 对响应数据处理
        console.log('响应拦截器 成功')
        return response;
    },function(error){
        // 超过 2** 范围的状态码都会触发该函数
        // 对响应错误处理
        console.log('响应拦截器 失败')
        return Promise.reject(error);
    });
    
    // 发送请求
    axios({
        method: 'GET',
        url:"http://localhost:3000/books"
    }).then(response => {
        console.log('自定义回调处理成功的结果')
        console.log(response);
    }).catch(reason => {
        console.log('自定义失败回调');
    })
    ```

- 可以给自定义的 axios 实例添加拦截器

  ```javascript
  const instance = axios.create();
  instance.interceptors.request.use(function () {/*...*/});
  ```

## 移除拦截器

- 语法

  ```javascript
  axios.interceptors.request.eject(拦截器);
  ```

# 取消请求

