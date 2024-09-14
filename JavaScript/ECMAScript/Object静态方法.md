# Object 静态方法

> 静态方法就是只有构造函数 Object 可以调用的

## Object.keys 获取对象中所有的属性（键）

`Object.keys`用于获取对象自身所有的属性

```javascript
Object.keys(obj)
```

返回一个由属性名组成的数组

```javascript
// 用于获取对象自身所有的属性
let obj = {
    id: 1,
    pname: '小米',
    price: 1999,
    num: 3000
};
let arr = Object.keys(obj);
console.log(arr);		 // ['id', 'pname', 'price', 'num']
```

## Object.assign 对象拷贝

Object.assign 常用于对象拷贝

```javascript
Object.assign(目标对象, ...被拷贝的对象) `
```

- 比如

  ```javascript
  let obj = {
      id: 1,
      uname: 'zhangsan',
      msg:{
          age: 18
      }
  };
  let o = {};
  
  Object.assign(o, obj);
  console.log(o);
  
  o.msg.age = 20;
  console.log(obj);	
  ```

  - 运行

    ![](http://127.0.0.1:8888/uploads/202304112314002.png) 

## Object.defineProperty

### 增加、修改属性或函数

Object.defineProperty() 定义对象中新属性或修改原有的属性

```javascript
Object.defineProperty(目标对象, 需定义或修改的属性的名字, {descriptor});
```

- descriptor：目标属性所拥有的特性（必需）
  - Object.defineProperty() 第三个参数 descript 说明：以对象形式 { } 书写
    1. value：设置属性的值，默认为 undefined
    2. writable：值是否可以重写。true | false 默认为 false
    3. enumerable：目标属性是否可以被枚举。true | false 默认为 false
    4. configurable：目标属性是否可以被删除 或 是否可以再次修改特性 true | false 默认为 false
    5. 函数：可以定义响应式函数

**通过Object.defineProperty()添加的属性会有默认值，已有属性来修改的没有默认值**

```javascript
let obj = {
    id: 1,
    pname: '小米',
    price: 1999
};

// Object.defineProperty() 定义新属性或修改原有的属性

// 1、添加属性
Object.defineProperty(obj, 'num', {		// 如果没有该属性则添加，有则修改
    value: 1000
    // 通过Object.defineProperty()新添加的属性，enumerable默认为false，若想被遍历则可以修改为 true
});
console.log(obj);		// {id: 1, pname: '小米', price: 1999, num: 1000}


// 2、修改属性
Object.defineProperty(obj, 'price', {		// 如果没有该属性则添加，有则修改
    value: 10
});
console.log(obj);		// {id: 1, pname: '小米', price: 10, num: 1000}


// 3、writable
Object.defineProperty(obj, 'id', {
    // false为不允许修改值，默认也为false，true则允许修改值
    writable: false
})
obj.id = 2;
console.log(obj);       // {id: 1, pname: '小米', price: 10, num: 1000}


// 4、enumerable
 Object.defineProperty(obj, 'color', {
     value: 'blue',
     // enumerable默认为false
     enumerable: false
 })
console.log(Object.keys(obj));      // enumerable 为 false 时输出：['id', 'pname', 'price']
									// enumerable 为 true 时输出：['id', 'pname', 'price', 'color']

// 5、configurable
Object.defineProperty(obj, 'pname', {
    configurable: false,    // configurable 如果为false，则不允许删除这个属性，不允许在修改参数里面的特性
    writable: false
})

// 在之前configurable为false的情况下，参数里的特性不允许修改
/*Object.defineProperty(obj, 'pname', {
       configurable: true,
       writable: true
})*/

delete obj.pname;       // 删除 pname 属性
obj.pname = '华为';     // 修改 pname 属性值
console.log(obj);   // {id: 1, pname: '小米', price: 10, num: 1000, color: 'blue'}
```

### 数据代理（数据劫持）

> 数据代理指的是在访问或者修改对象的某个属性时，通过一段代码拦截这个行为，进行额外的操作或者修改返回结果

数据代理：通过一个对象代理对另一个对象中的属性操作（读/写）

- 简单的数据代理示例1

  ```javascript
   let obj = {x:100};
   let obj = {y:200};
   // 通过一个对象代理；对另一个对象中属性的操作
   Object.defineProperty(obj2,'x',{
       get(){
           return obj.x
       },
       set(value){
           obj.x = value
       }
   })
  ```

- 示例2

  ```javascript
  let number = 18;
  let person = {
      name:'张三',
      gender:'男',
  }
  Object.defineProerty(person,'age',{
      // 函数格式
  /*     get:function [函数名](){
          console.log('有人读取age属性')
          return number;
      }
  */ 
      // 简写形式
      // 当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
      get(){
          console.log('有人读取age属性')
          return number;
      }
      // 当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
      set(value){
      	console.log('有人修改了age属性，且值是',value)
  	}
  })
  console.log(person.age);
  ```
