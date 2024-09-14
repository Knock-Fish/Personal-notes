# util.promisify方法进行promise风格转化

- 示例

  ```javascript
  // 引入 util 模块
  const util = require('util');
  // 引入 fs 模块
  const fs = require('fs')
  // 返回一个新的函数
  let mineReadFile = util.promisify(fs.readFile);
  
  mineReadFile('./xxx.txt').then(value => {
      console.log(value.toString())
  })
  ```

# promise

## 状态

promise 的状态是实例对象中的一个`PromiseState`属性

- 状态
  - pending 未决定的
  - resolved / fullfilled 成功
  - rejected 失败

promise 的状态改变

- pending 变为 resolved
- pending 变为 rejected
  - 不会从成功变为失败，或失败变为成功

说明：
- 只有这2中，且一个 promise 对象只能改变一次
  1. 无论变为成功还是失败，都会有一个结果数据
  2. 成功的结果数据一般称为 value，失败的结果数据一般称为 reason

## 对象的值

1. Promise 对象的值是实例对象中的一个`PromiseResult`属性
2. 保存着异步任务（成功/失败）的结果
3. 只有通过 resolve 和 reject 修改

# API

1. Promise 构造函数：`Promise(executor){}`

   1. executor 函数： 执行器 (resolve, reject) => ()
   2. resolve 函数：内部定义成功时调用的函数 value => {}
   3. reject 函数：内部定义失败时定义的函数 reason => {}
   4. 说明：executor 会在Promise 内部立即同步调用，异步操作在执行器中执行

2. Promise.prototype.then 方法（指定回调）：(onResolved, onRejected) => {}

   1. onResolved函数：成功的回调函数 (value) => {}
   2. onRejected函数：失败的回调函数 (reason) => {}
   3. 说明：指定用于得到成功 value 的成功回调和用于得到失败 reason 的失败回调，会返回一个新的 promise对象

3. Promise.prototype.catch方法：(onRejected) => {}

   - onRejected函数：失败的回调函数 (reason) => {}
   - 说明：then()的语法糖，相当于：then(undefined, onRejected)

4. （属于Promise函数对象）Promise.resolve 方法：(value) => {}

   - value：成功的数据 或 promise 对象

   - 说明：返回一个成功 / 失败的 promise 对象

     ```javascript
     let p1 = Promise.resolve(10);
     // 如果传入的参数为 非Promise类型的对象，则返回的结果为成功promise对象
     // 如果传入的参数为 Promise 对象，则参数的结果决定了 resolve 的结果
     let p2 = Promise.resolve(new Promise((resolve, reject) => {
         // resolve('OK');
         reject('Error');
     }))
     console.log(p1);
     console.log(p2);
     
     p2.catch(reason => {
         console.log(reason);
     })
     ```
   
5. （属于Promise函数对象）Promise.reject 方法：(reason) => {}

   - reason：失败的原因

   - 说明：返回一个失败的 promise 对象

     ```javascript
     // 无论传入什么，都返回一个失败的 promise 对象
     let p = Promise.reject(10);
     let p2 = Promise.reject('abc');
     let p3 = Promise.reject(new Promise((resolve, reject) => {
         resolve('OK');
     }))
     console.log(p);
     console.log(p2);
     console.log(p3);
     ```
   
6. （属于Promise函数对象）Promise.all 方法：(promises) => {}

   - promises：包含 n 个 promise 的数组

   - 说明：返回一个新的 promise，只有所有 promise 都成功才成功，只要有一个失败了就直接失败

     ```javascript
     let p1 = new Promise((resolve, reject) => {
         resolve('OK');
     })
     let p2 = Promise.resolve('Yes');
     let p3 = Promise.resolve(10);
     
     let p4 = Promise.reject('Error');
     // 将成功的Promise对象放入result数组
     const result = Promise.all([p1, p2, p3]);
     console.log(result);
     
     // 如果有一个失败了就直接失败
     const errorResult = Promise.all([p1, p2, p3, p4]);
     console.log(errorResult);
     ```
   
7. Promise.race 方法：(promises) => {}

   - promises：包含 n 个promise 的数组

   - 说明：返回一个新的 promise，第一个完成的 promise 的结果状态就是最终的结果状态

     ```javascript
     let p1 = new Promise((resolve, reject) => {
         setTimeout(() => {
             resolve('OK');
         },1000)
     })
     let p2 = Promise.resolve('Yes');
     let p3 = Promise.resolve(10);
     
     let p4 = Promise.resolve('Error');
     
     const result = Promise.race([p1, p2, p3]);
     console.log(result);  
     ```
   
8. 如何改变 promise 的状态

   - resolve(value)：如果当前是pending就会变为 resolved
   
   
      - reject(reason)：如果当前是 pending 就会变为 rejected
   
   
      - 抛出异常（throw）：如果当前是pending就会变为 rejected
   
        ```javascript
        const p = new Promise((resolve, reject) => {
            // reject('Error');
            // resolve('OK');
            throw '错误'
        
        })
        console.log(p)
        ```
        
   

9. 如果一个promise指定多个成功 / 失败回调函数，当 promise 改变为对应状态时都会调用

   ```javascript
   const p = new Promise((resolve, reject) => {
       resolve('OK')
   })
   
   // 指定回调1
   p.then((value)=>{
       console.log(value)
   })
   // 指定回调2
   p.then((value)=>{
       alert(value)
   })
   ```

10. 改变 promise 状态和指定回调函数顺序

    - 谁前后都有可能，正常情况下先指定回调再改变状态，但也可以先改变状态再指定回调
    - 如何先改变状态再指定回调
      - 在执行器中直接调用 resolve() / reject()
      - 延迟更长时间才调用 then()
    - 什么时候才能得到数据
      - 如果先指定的回调，那当状态发生改变时，回调函数就会调用，得到数据
      - 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据
        - 有异步任务时先then先执行，同步则状态先改变
        - 指定：在then中，放入成功或者失败状态，对应状态有特定的回调函数，对象关系，但是没实施

11. promise.then() 返回的新promise的结果状态由上面决定

    1. 由then()指定的回调函数执行的结果决定

    2. 如果抛出异常，新 promise 变为 rejected，reason为抛出的异常

    3. 如果返回的是非 promise 的任意值，新 promise 变为 resolved，value为返回的值

    4. 如果返回的是另一个新 reomise，此 promise 的结果就会成为新 promise 的结果

       ```javascript
       const p = new Promise((resolve, reject) => {
           resolve('OK')
       })
       let result = p.then((value) => {
           // console.log(value);
           // 1、抛出错误
           // throw '错误'
           // 2、返回结果是非 Promise 类型的对象
           // return 10;
           // 3、返回结果是 Promise 对象
           return new Promise((resolve, reject) => { 
               // resolve('success');
               reject('error');
           })
       }, (reason) => {
           console.warn(reason)
       })
       ```
    
12. promise 串连多个操作任务

    - promise的then()返回一个新的promise，可以开成then()的链式调用

    - 通过 then 的链式调用串连多个同步 / 异步任务

      ```javascript
      const p = new Promise((resolve, reject) => {
          setTimeout(() => {
              resolve('OK')
          },1000)
      })
      p.then(value => {
          console.log(value)
          new Promise((resolve, reject) => {
              resolve('success')
          });
      }).then(value => {
          console.log(value)
      }).then(value => {
          console.log(value)
      })
      ```
    
13. promise 异常穿透

    - 当使用 promise 的then链式调用时，可以在最后指定失败的问题

    - 办法：在回调函数中返回一个pendding状态的promise对象

      ```javascript
       const p = new Promise((resolve, reject)=>{
           setTimeout(()=>{
               resolve('OK')
               // reject('Error')
           },1000)
       })
       p.then(value => {
           // console.log(111);
           throw '错误'
       }).then(value => {
           console.log(222);
       }).then(value => {
           console.log(333);
       }).catch(reason => {		// 如前面发生了错误，则执行此回调
           console.log(reason);
       })
      ```
    
14. 如何中断promise链

    - 当使用promise的then链式调用时，在中间中断，不再调用后面的回调函数

    - 办法：在回调函数中返回一个promise状态的promise对象

      ```javascript
      const p = new Promise((resolve, reject)=>{
          setTimeout(()=>{
              resolve('OK')
          },1000)
      })
      p.then(value => {
          console.log(111);
          return new Promise(()=>{})
      }).then(value => {
          console.log(222);
      }).then(value => {
          console.log(333);
      }).catch(reason => {
          console.log(reason);
      })
      ```

# asyns 函数

函数的返回值为 promise 对象

promise对象的结果由 async 函数执行的返回值决定

```javascript
async function main(){
    // 返回值是一个非Promise类型的数据
    // return 10;
    // 返回的是一个Promise对象
    /*return new Promise((resolve, reject) => {
        //resolve('OK');
        reject('Error')
    })*/
    // 抛出异常
    throw 'No'
}
let result = main();
console.log(result);
```

# await表达式

1. await右侧的表达式一般为 promise 对象，但也可以是其他的值
2. 如果表达式是 promise 对象，await 返回的是 promise 成功的值
3. 如果表达式是其他值，直接将此值作为 await 的返回值
4. 注意
   - await 必须写在 async 函数中，但 async 函数中可以没有 await
   - 如果 await 的promise 失败了，就会抛出异常，需要通过 try...catch 捕获处理
