# ?可选链操作符

# TypeScript

## 简介

- TypeScript是什么
  - 以JavaScript为基础构建的语言
  - 一个JavaScript的超集
  - 可以在任何支持JavaScript的平台中执行
    - TS不能被JS解析器直接执行，需要将TS编译成JS
  - TypeScript扩展了JavaScript并添加了类型

## TypeScript开发环境搭建

1. 使用npm全局安装typescript
   - 进入命令输入：`npm i -g typescript`
2. 创建一个ts文件
3. 使用tsc对ts文件进行编译
   - 进入命令行
   - 进入ts文件所在目录
   - 执行命令：`tsc xxx.ts`

# 基本类型

## 说明和使用

- 类型声明

  1. 类型声明是TS非常重要的一个特点

  2. 通过类型声明可以指定TS中变量（参数、形参）的类型

  3. 指定类型后，当为变量赋值时，TS编译会自动检查是否符合类型声明，符合则赋值，否则报错

  4. 简而言之，类型声明给变量设置了类型，使得变量只能存储某中类型的值

- 语法

   - 变量赋值类型声明

      ```typescript
      // 给变量设置了类型，使变量只能存储指定类型的值
      let 变量:类型;
      let 变量:类型 = 值;
      ```

   - 函数类型声明

     ```typescript
     // 给函数的参数和返回值设置类型，其传参的值只能传递指定类型的值
     function fn(参数:类型, 参数:类型):类型{
         ...
     }
     ```

- 自动类型判断

  - TS拥有自动的类型判断机制

  - 当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型

    - 依照第一次赋值的类型

  - 所以如果你的变量的声明和赋值是同时进行的，可以省略类型声明

    - 示例

      ```typescript
      let a = false;
      a = true;
      // a = 123;		// 此代码会报错，因为123为number类型
      ```

## 类型

- 基本类型

  | 类型    | 描述                           | 例子              |
  | ------- | ------------------------------ | ----------------- |
  | number  | 任意数字                       | 1，-33，2.5       |
  | string  | 任意字符串                     | "hi"，'hi'，`hi`  |
  | boolean | 布尔值true或false              | true、false       |
  | 字面量  | 限制变量的值就是该字面量的值   | 其本身            |
  | any     | 任意类型                       | *                 |
  | unknown | 类型安全的any                  | *                 |
  | vold    | 没有值（或undefined）          | 空值（undefined） |
  | never   | 不能是任何值                   | 没有值            |
  | object  | 任意的 JS 对象                 | {name:'张三'}     |
  | array   | 任意 JS 数组                   | [1, 2, 3]         |
  | tuple   | 元组、TS新增类型，固定长度数组 | [4, 5]            |
  | enum    | 枚举，TS新增类型               | enum[A, B]        |

## 类型基本使用

- number类型

  ```typescript
  // 使用number进行类型声明
  let a:number;
  a = 10;
  a = 15;
  ```

- string类型

  ```typescript
  // 使用string进行类型声明
  let a:string;
  a = "hello";
  a = "world";
  ```

- booleam类型

  ```typescript
  // 使用boolean进行类型声明
  let a:boolean;
  a = true;
  a = false;
  ```

- 字面量限制类型

  ```typescript
  // 使用字面量进行类型声明
  let a: 10;  // 类似于常量，限制a的值只能是10
  a = 10;
  // a = 15;  // 报错
  ```

- any类型

  ```typescript
  // any表示的是任意类型，一个变量设置类型为any后相当于对该变量关闭了TS的类型检测
   let a:any;
   a = 10;
   a = "hello";
   a = true;
  
   // 声明变量如果不指定类型，则TS解析器会自动判断变量的类型为any（隐式any）
   let b;
   b = 10;
   b = "hello";
   b = "true";
  
  // any类型也可以赋值给任意变量
  let e:any;
  e = true;   // 不影响类型赋值操作
  let d;      // 未先类型声明
  d = e;      // 将any类型赋值给变量
  ```

- unknown未知类型

  ```typescript
  // unknown表示未知类型的值
  let a:unknown;
  a = 10;
  a = "hello";
  a = true;
  
  // unknown 实际上就是一个类型安全的any
  // unknown 类型的变量，不能直接赋值给其他变量（和any的区别）
  let b:string;
  a = "hello"
  // b = a;   // 报错
  
  // 解决赋值方案
  // 第一种
  if(typeof b === "string"){
      b = a;
  }
  
  // 第二种
  // 类型断言，可以用来告诉解析器变量的实际类型（两种方法，效果等价）
  /*
  	语法：
  		变量 as 类型
  		<类型>变量
  */
  b = a as string;
  b = <string>a;
  ```

- void类型

  ```typescript
  // void 用来表示空，以函数为例，就表示没有返回值的函数（只要有返回值就报错）
  function fn():void{
      // return "hello";  // 报错
  }
  ```

- never类型

  ```typescript
  // never 表示永远不会返回结果
  function fn():never{
      throw new Error('报错了！')
  }
  ```

- object类型

  ```typescript
  // object表示一个js对象
  let a:object;
  a = {};
  a = function(){
  
  }
  
  // {}用来指定对象中可以包含哪些属性
  // 语法：{属性名:属性值,属性名:属性值,....}
  // 在属性名后边加上?，表示属性是可选的，否则结构必须一样
  let b :{name:string, age?:number};
  b = {name:'张三'};
  b = {name:'李四',age:18}
  
  // 定义对象结构
  // [变量:string]:类型   中括号表示属性名的类型；分号右边表示属性值的类型；并且可以写多个元素
  let c: {name:string,[propName: string]:any};
  c = {name: '王五', age: 18, gender: '男'}
  
  
  // 定义函数结构
  /* 
  	设置函数结构的类型声明
     语法：(形参:类型,形参:类型,...) => 返回值
  */
  let d:(a:number, b:number) => number
  ```

- tuple类型

  ```typescript
  /* 
     元组：元组就是固定长度的数组
         语法：[类型,类型,类型]
  */
   let a:[string, number]
   a = ['hello', 123]
  ```

- enum类型

  ```typescript
  // enum
  enum Gender{
      // Male = 1      // 也可以作赋值操作
      Male,      
      Female
  }
  
  let a:{name: string, gender:Gender};
  a = {
      name:"张三",
      gender:Gender.Male  // 'male
  }
  console.log(a.gender === Gender.Male);  // true
  ```

## 类型相关的操作

### 联合类型

1. 可以使用 ` | ` 来连接多个类型

   - 语法

     ```typescript
     let 变量: 类型 | 类型 |...
     ```

   - 示例

     ```typescript
     // 可以使用 | 来连接多个类型（联合类型）
     let a: "male" | "female";
     a = "male";
     a = "female";
     // a = "Hello"; // 报错
     
     let b: boolean | string;
     b = true;
     b = "hello";
     ```

2. 可以使用 ` & ` 表示同时要有多个类型（适用于对象）

   - 语法

     ```
     let 变量:{属性名:类型} & {属性名:类型} &...;
     ```

   - 示例

     ```typescript
     // & 表示同时
     let j: {name:string} & {age: number};	// 既要满足name类型，又要满足age类型
     j = {name:'张三', age: 18};
     ```

### 类型的别名

- 语法

  ```typescript
  type 变量 = 类型
  ```

- 示例

  ```typescript
  type myType = 1 | 2 | 3 | 4 | 5;
  let a:myType;
  let b:myType;
  
  a = 2
  b = 5
  ```

### 类型断言

- 有些情况下，变量的类型对于我们来说是很明确的，但是TS编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式

  - 第一种

    ```typescript
    let someValue: unknown = "this is a string";
    let strLength:number = (someValue as string).length;
    ```

  - 第二种

    ```
    let someValue: unknown = "this is a string";
    let strLength:number = (<string>someValue).length;
    ```

# 编译选项

- 自动编译文件

  - 编译文件时，使用 -w 指令后，TS编译器会自动监视文件的变化，并在文件发送变化时对文件解析重新编译

  - 指令

    ```
    tsc xxx.ts -w
    ```

- 自动编译整个项目

  - 使用指令

    - 如果直接使用 `tsc` 指令，则可以自动将当前项目下的所有 ts 文件编译为 js 文件（默认编译所有）
    - 如果使用 `tsc -w`，TS编译器会自动监视文件夹下所有 ts文件 的变化，并全部自动编译

  - 但是能直接使用 tsc 命令的前提时，要先在项目根目录下创建一个 ts 的配置文件 tsconfig.json

    - 或在命令行输入：`tsc init`

  - tsconfig.json是一个 JSON 文件，也是 ts 编译器的配置文件，ts 编译器可以根据它的消息来对代码进行编译，添加配置文件后，只需 tsc 命令即可完成对整个项目的编译

  - tsconfig.json配置选项

    1. include

       - 定义希望被编译文件所在的目录（用来指定哪些ts文件需要被编译）

       - 默认值：`["**/*"]`

         - 路径
           - `*`表示任意文件
           - `**`星表示任意目录

       - 示例

         ```json
         "include":[
             "src/**/*", 	// 路径
             "tests/**/*"
         ]
         ```

         - 上述示例中，所有src目录和tests目录下的文件都会被编译

    2. exclude

       - 定义需要排除在外的目录（用来指定哪些ts文件不需要被编译）

       - 默认值：`["node_modules","bower_components","jspm_packages"]`

       - 示例

         ```
         "extends":"./config/base"
         ```

         - 上述示例中，当前配置文件中会自动包含config目录下base.json中的所有配置信息

    3. file

       - 指定被编译文件的列表，只有需要编译的文件少时才会用到

       - 示例

         ```json
         "file":[
         	"a.ts",
             "b.ts",
             "c.ts",
             "d.ts"
         ]
         ```

         - 列表中的文件都会被TS编译器所编译

    4. compilerOptions

       - 编译选项是配置文件中非常重要也比较复杂的配置选项

       - 在complierOptions中包含多个子选项，用来完成对编译的配置

         - 项目选项

           1. target

              - 设置ts代码编译的目标版本（用来指定ts被编译为的ES的版本）

              - 可选值

                - ES3（默认）、ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2020、ESNext（最新版本）

              - 示例

                ```json
                "compilerOptions":{
                	"target":"ES6"
                }
                ```

                - 如上设置，所编写的 ts 代码会被编译为 ES6 版本的 js 代码

           2. module（指定要使用的模块化的规范）

              - 设置编译后代码使用的模块化系统
              - 可选值

           3. lib（用来指定项目中要使用的库）

              - 指定代码运行时所包含的库（宿主环境）

              - 默认情况就是浏览器的运行环境

              - 可选值

                - .......

              - 示例

                ```json
                "compilerOptions":{
                	"target":"ES6",
                    "lib":["ES6"."DOM"],
                    ...
                }
                ```

           4. outDir（用来指定编译后文件所在的目录）

              - 默认情况下，编译后的js文件会和ts位于相同的目录，设置outDir后可以改变编译后文件的位置

              - 示例

                ```json
                "compilerOptions":{
                	"outDir":"./dist"	// 如没有该目录则创建目录
                }
                ```

           5. outFile（将代码合并为一个文件）

              - 示例

                ```json
                "compilerOptions":{
                    // 设置outFile后，所有的全局作用域中的代码会合并到同一个文件中
                	"outFile":"./dist/app.js"	// 将所有的ts文件合并一起，并合成后文件名为app.js
                }
                ```

           6. allowJs（是否对 js 文件进行编译，默认是false）

              - 示例

                ```json
                "compilerOptions":{
                	"allowJs":true
                }
                ```

           7. checkJs（是否检查 js 代码是否符合语法规范，默认值false）

              - 示例

                ```json
                "compilerOptions":{
                	"checkJs":false
                }
                ```

           8. removeComments（是否移除注释，默认值false）

              - 示例

                ```json
                "compilerOptions":{
                	"removeComments":false
                }
                ```

           9. noEmit（是否生成编译后的文件，默认值false）

              - 示例

                ```json
                "compilerOptions":{
                	"noEmit":true	// true为不生成编译
                }
                ```

           10. noEmitOnError（当有错误时不生成编译后的文件，默认值false）

               - 示例

                 ```json
                 "compilerOptions":{
                 	"noEmitOnError":true
                 }
                 ```

           11. alwaysStrivt（用来设置编译后的文件是否使用严格模式，默认为false）

               - 有模块代码（export）的js文件默认开启严格模式

               - 示例

                 ```json
                 "compilerOptions":{
                 	"alwaysStrivt":true	// 编译后的js文件使用严格模式
                 }
                 ```

           12. noImplicitAny（不允许隐式的any类型）

               - 示例

                 ```json
                 "compilerOptions":{
                 	"noImplicitAny":true	// true为不允许隐式any
                 }
                 ```

           13. noImplicitThis（不允许不明确类型的this，默认值为false）

               - 示例

                 ```json
                 "compilerOptions":{
                 	"noImplicitThis":true	// true为不允许不明确类型的this
                 }
                 ```

               - 例如

                 ```javascript
                 function fn(){
                 	console.log(this)	// 报错
                 }
                 
                 // 可以给this指定类型
                 function fn(this: Window){
                 	console.log(this)	// 报错
                 }
                 ```

           14. strictNullChecks（严格的检查空值，默认值为false）

               - 示例

                 ```json
                 "compilerOptions":{
                 	"strictNullChecks":true
                 }
                 ```

               - 例如

                 ```javascript
                 let box = document.getElememtById('box');
                 // 解决报红的两种方案
                 // 第一种
                 if(box !== null){
                     box.addEventListener('click',function(){
                         alert('hello')
                     })
                 }
                 // 第二种
                 box?.addEventListener('click',function(){
                     alert('hello');
                 })
                 ```

           15. strict（所有严格检查的总开关）

               - 如值为true，alwaysStrivt、noImplicitAny、noImplicitThis、strictNullChecks都会开启

# 接口

> 规定类的结构

1. 作用

   1. 使用 interface 定义一个类结构，用来定义一个类中应该包含哪些属性和方法

   2. 接口也可以当成类型声明去使用

      - 结构

        ```typescript
        interface myInterface{
        	name:string;
            age:number;
        }
        ```

   3. 接口可以在定义类的时候去限制类的结构

      - 接口中的所有的属性都不能有实际的值

      - 接口只定义对象的结构，而不考虑实际值

      - 在接口中所有的方法都是抽象方法

        - 示例

          ```typescript
          interface myInter{
          	name:string;
          	sayHello():void
          }
          ```

2. 接口可以同时定义多个，如定义的接口名一致，则相同的接口会合并

   - 结构

     ```typescript
     interface myInterface{
     	name:string;
         age:number;
     }
     interface myInterface{
     	gender:string;
     }
     
     const obj: myInterface = {
         name:'张三',
         age:18,
         gerder:'男'
     }
     ```

3. 定义类时，可以使类去实现一个接口

   - 实现接口就是使类满足接口的要求

     - 示例

       ```typescript
       interface myInter{
       	name:string;
       	sayHello():void
       }
       
       class MyClass implements myInter{
           name:string;
           constructor(name: string){
               this.name = name
           }
           sayHello(){
               console.log('你好')
           }
       }
       ```

# 属性的封装

- TS可以在属性前添加属性的修饰符

  - pubilc  修饰的属性可以在任意位置访问（修改）默认值

  - private 私有属性，私有属性只能在类内部进行访问（修改）
    - 通过在类中添加方法使得私有属性可以被外部访问

  - protected 受保护的属性，只能在当前类和当前类的子类中访问（修改）

# 泛型