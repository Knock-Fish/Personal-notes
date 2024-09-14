# MockJS

在开发前端的时候，后端可能还没有出接口，我们可以根据后端的响应 mock 假数据

除此之外，mock还能做到

1. 前端后端并行开发
2. 前端独立运行

## 安装

```vue
npm i mockjs
npm i -D @types/mockjs
```

## 快速尝鲜

```typescript
import { mock } from "mockjs";		// 导入mockjs
let data = mock({
	"data": "@cname()",
	"age": "@integer(1,100)",
	"addr": "@city(true)".
	"email": "@email(qq.com)"
})
console.log(date)
```

# 数据模板定义规范

数据模板中的每个属性由 3 部分构成：属性名、生成规则、属性值，如下：

```typescript
"属性名 | 生成规则" : 属性值
```

生成规则规则有七种

```typescript
'name|min-max': value				// 整数的范围 
'name|count': value					// 具体的整数
'name|min-max.dmin-dmax': value		// 整数的范围 和 小数部分的范围
'name|min-max.dcount': value		// 整数的范围 和 小数部分
'name|count.dmin-dmax': value		// 整数 和 小数部分的范围
'name|count.dcount': value			// 整数 和 小数部分
'name|+step': value					// 值每次相加step
```



## 值是字符串

```typescript
let data = mock({
	"name1|2-3":"a",	// 重复后面的字符串 2 或者 3 次
	"name2|3":"b"		// 重复后面这个字符串 3 次
})
console.log(data)
```



## 值是数字

```typescript
let data = mock({
	"list|5":[		// 生成 5 个已经定义属性名的列表
		{
			"num1|+1": 1,
			"num2|+5": 1,			// 每次 +5，从 1 开始
			"num3|11-20": 1,		// 范围随机取出 1 个数
			"num4|2.2": 1,			// 整数部分等于 2，小数部分保留 2 位
			"num5|4-5.2-3": 1,		// 整数部分 4 或者 5，小数部分保留 2 或者 3 位
		}
	]
})
console.log(data)
```



## 值是布尔

```typescript
let data = mock({
	"bool1|1": ture,		// 值为 true 的概率是 1/2，值为 false 的概率同样是 1/2
	"bool2|min-max":value,	// 值为 value 的概率是 min / (min + max)，值为 !value 的概率是 max / (min + max)
})
```



## 值是对象

```typescript
let data = mock({
	"list|5":[
		{
			"person|1":{		// 从这个对象里面随机取一个属性
				name: "张三",
				age: 21
			},
		}
	]
})
console.log(JSON.stringify(data))
```



## 值是数组

```typescript
let data = mock({
	"list|5":[
		{
			"a1|1":[1,2,3,8],	// 从数组里面随机选取一个
			“a2|2”:[1,2],		// 重复数组里面的数据 2 次	"a2":[1,2,1,2]
		}
	]
})
```

**注意点**：为 1 是随机选 1 个，除了 1 其他都是重复多少次



## 值是函数

```typescript
let data = mock({
	"list|5":[
		{
			"f1":function(item:any):string{
				console.log(item)
				return "张三"
			}
		}
	]
})
console.log(JSON.stringify(data))
```



## 值是正则表达式

根据正则表达式反向生成可以匹配它的字符串。用于生成自定义格式的字符串

```typescript
let data = mock({
	"list|5":[
		{
			'regexp1': /[a-z][A-Z][0-9]/,
    		'regexp2': /\w\W\s\S\d\D/,
    		'regexp3': /\d{5,10}/
		}
	]
})
```

# 数据占位符

占位符 只是在属性值字符串中占个位置，并不出现在最终的属性值中

占位符的格式为：

```typescript
@占位符
@占位符(参数 [, 参数])
```



占位符引用的是 `Mock.Random` 中的方法

以下是支持的方法

| Type          | Method                                                       |
| ------------- | ------------------------------------------------------------ |
| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last ,name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | area, region                                                 |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |



下面这两种写法是等价的

```typescript
import {mock, Random} from "mockjs"

let data = mock({
    "list|5":[
        '随机布尔1':"@boolean",
        '随机布尔2':Random.boolean()
    ]
})
```



## 图片

https://dummyimage.com/

```typescript
Random.image("200X100","Adobe","fff","png","Hello")
// 大小 背景色 前景色 后缀 文本
```



## 常用的

```typescript
import {mock, Random} from "mockjs"
let data = mock({
    "list|2":[
        {
            '数字':"@integer(2, 5)",
            '小数':"@float(2, 5, 1, 1)",		// 从2-5，1位小数
            '数组':"@range(1, 10, 3)",		// 从1开始，到10，步长是3
    	},
        {
            "日期":"@date",
            "现在的时间":"@now",
            "日期时间":"@datetime",
        },
        {
            "图片":Random.image("200X100","Adobe","fff","png","Hello"),
        },
        {
            "颜色":"@color"
        },
        {
            "中文文本":"@cparagraph(10,20)",
            "中文句子":"@csentence(10,20)",
            "中文标题":"@ctitle(3,5)",
        },
        {
            "中文名字":"@cname"
        },
        {
            "url":"@url('http')",
            "域名":"@domain('com')",
            "ip":"@ip()",
            "邮箱":"@email",
        },
        {
            "地址":"@city(true)",		// 完整的省市
            “城市”:"@city",			// 市
        },
        {
            "uuid":"@guid",
            "id":"@id"
        }
    ]
})
console.log(JSON.stringify(data))
```



# 拦截axios

可以给请求写一个 mock 的规则，不需要后端接口也能跑前端

**注意：需要在发请求之前把 mock 的拦截写上**

```typescript
import axios from "axios"
import {mock} from "mockjs"
mock("http://localhost:3000/users",{
    code: 0,
    data: {
        "id|+1": 1,
        name: "@cname",
        "age|16-30": 1,
    },
    msg: "成功"
})

async function getData(){
    let res = await axios.get("http://localhost:3000/users")
    console.log(res.data)
}
getData()
```

运行之后，实际的响应数据是 mock 里面的数据

mock 拦截请求的原理简单点说就是把原生的 XHR 给重写了，变成自己的

所有说，在开启 mock 之后，被拦截的请求是没有发送的，在网络请求中是看不到的

## 拦截的几种写法

### 完整匹配

```typescript
import axios from "axios"
import {mock} from "mockjs"
mock("http://localhost:3000/users",{
    code: 0,
    data: {
        "id|+1": 1,
        name: "@cname",
        "age|16-30": 1,
    },
    msg: "成功"
})

async function getData(){
    let res = await axios.get("http://localhost:3000/users")
    console.log(res.data)
}
getData()
```



### method

```typescript
import {mock} from "mockjs"
mock("http://localhost:3000/users", "get", {
    code: 0,
    data: {
        "id|+1": 1,
        name: "@cname",
        "age|16-23": 1,
    },
    mag: "成功"
})
```



### 正则

```typescript
import {mock} from "mockjs"
mock(/.*?\/users/, {
    code: 0,
    data: {
        "id|+1": 1,
        name: "@cname",
        "age|16-23": 1,
    },
    mag: "成功"
})
```



### 函数模式

```typescript
import {mock} from "mockjs"
import type {MockjsRequestOptions} from "mockjs"
mock("http://localhost:3000/users", function(options: MockjsRequestOptions){
    console.log(options)
    return{
        code: 0,
        data: {
            "id|+1": 1,
            name: "@cname",
            "age|16-23": 1,
        },
        msg: "成功"
    }
})
```



### 代理增删改查

```typescript
import axios from "axios"
import {mock} from "mockjs"
import type {MockjsRequestOptions} from "mockjs"

// 增
mock("http://localhost:3000/users", "post", function(options: MockjsRequestOptions){
    console.log(options)
    return{
        code: 0,
        data:{},
        msg: "创建成功"
    }
})

// 删
mock("http://localhost:3000/users", "delete", function(options: MockjsRequestOptions) {
    console.log(options)
    return{
        code: 0,
        data: {},
        msg: "删除成功"
    }
})

// 改
mock("http://localhost:3000/users", "put", function(options: MockRequestOptions){
    console.log(options)
    return{
        code: 0,
        data:{},
        msg: "修改成功"
    }
})

// 查
mock("http://localhost:3000/users", "get", {
    code: 0,
    data: {
        "id|+1": 1,
        name: "@cname",
        "age|16-23" :1,
    },
    msg: "成功 "
})

async function getData(){
    axios.defaults.baseURL = "http://localhost:3000"
    
    let res = await axios.get("/users")
    console.log(res.data)
    
    res = await axios.post("/users/1",{name: "王五"})
    console.log(res.data)
    
    res = await axios.put("/users/1",{name: "张三"})
    console.log(res.data)
    
    res = await axios.delete("/users/1")
    console.log(res.data)
}
getData()
```

