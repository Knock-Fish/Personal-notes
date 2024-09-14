# AJAX

- 定义
  - AJAX 是异步的 JavaScript 和 XML （Asynchronous JavaScript And XML）
  - 使用 XMLHttpRequest 对象与服务器通信
  - 可以使用 JSON，XML，HTML 和 text 文本等格式发送和接收数据
  - “异步”特性：AJAX可以在不重新刷新页面的情况下与服务器通信，交换数据，或更新页面（浏览器和服务器之间通信，动态数据交互）

# URL说明

- **统一资源定位符(Uniform Resource Locator, URL)**是互联网上标准资源的地址。互联网上的每个文件都有—个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它

- URL的一般语法格式为

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

