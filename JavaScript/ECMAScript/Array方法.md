# Array方法

## 数组的基本使用

### 创建数组对象

数组中可以存放容易类型的数据，例如字符串，数字，布尔值等

创建数组对象的两种方式

1. 字面量方式

   ```javascript
   // 利用数组字面量创建数组
   let arr = [1, 2, 3];
   ```

2. `new Array()`创建

   ```javascript
   // 利用 new Array()
   let arr1 = new Array();		// 创建了一个空的数组
   let arr2 = new Array(2);	// 括号里的 2 表示数组的长度为2，里面有2个空的数组元素
   let arr3 = new Array(2,3)	// 等价于字面量创建的[2,3]，这样写表示里面有2个数组元素是 2 和 3
   ```

### 数组的索引

数组是按顺序保存，每个数据都有自己的编号，可以通过 **索引**（**下标**） 获取元素

> 下标：数组中数据的编号
>
> 元素：数组中保存的每个数据都叫数组元素

```javascript
let arr = ['A','B','C','D','E'];
// A 索引为 0，B 索引为 1，以此类推

// 数组名[ 下标 ]
let arr = ['A','B',4,true,'C','9'];
console.log(arr[2]);	// 4
console.log(arr[0]);	// A
```

### 获取数组的长度

数组的长度可使用 length 函数获取

数组的长度是元素个数，不要跟索引号混淆

```javascript
// 数组长度 数组名.length
let num =[1,2,3,4,5,6];
console.log(num.length);	// 输出结果：6
// 利用.length 进行数组遍历
for(let i = 0; i < arr.length; i++){
    console.log(arr[i]);	// 输出结果：A B 4 true C 9
}
```

### 数组新增元素

通过修改 length 长度新增数组元素

可以通过修改 length长度来实现数组扩容的目的

length属性是可读写的

```javascript
// 新增数组元素，修改 length 长度
let arr = ['A','B','C','D'];
arr.length = 6;		// 数组的长度修改为 6,里面应该有 6 个元素
console.log(arr);

// 其中索引号是 4，5 的空间没有给值，就是声明变量未给值，默认值就是 undefined
console.log(arr[4]);	// undefined
console.log(arr[5]);	// undefined 
```

- 代码输出结果

  ![](http://127.0.0.1:8888/uploads/202302272302435.png) 

### 通过修改数组索引新增数组元素

可以通过修改数组索引的方式追加数组元素

不能直接给数组名赋值，否则会覆盖掉以前的数据

```javascript
// 新增数组元素，修改索引号 追加数组元素
let arr = ['A','B','C','D'];

arr[4] = 'E';
console.log(arr); 	// 输出结果：A B C D E

arr[0] = 'Hello';
conso;e.log(arr);	// 输出结果：Hello B C D E

arr = 'Array';		// 直接给 数组名赋值 里面的数组元素都没有了
console.log(arr);	// 输出结果： Array
```


## 检测是否为数组

| 方法&运算符     | 说明                            | 返回值                                         |
| --------------- | ------------------------------- | ---------------------------------------------- |
| instanceof      | 可以用来检测是否为数组          | 如果检测的对象为Array，则返回true，否则为false |
| Array.isArray() | 用于确定传递的值是否是一个Array | 如果检测的对象为Array，则返回true，否则为false |

```javascript
// instanceof运算符
let arr = [];
let obj = {};
console.log(arr instanceof Array);	// true
console.log(obj instanceof Array);	// false

// Array.isArray() 方法
let arr = [];
let obj = {};
console.log(Array.isArray(arr));	// true
console.log(Array.isArray(obj));	// false
```

## 添加删除数组元素的方法

| 方法名              | 说明                                                      | 返回值             |
| ------------------- | --------------------------------------------------------- | ------------------ |
| push(参数1, ...)    | 末尾添加一个或多个元素，会修改原数组                      | 返回新的数组长度   |
| pop()               | 删除数组最后一个元素，把数组长度减1，无参数，会修改原数组 | 返回删除的元素的值 |
| unshift(参数1, ...) | 向数组的开头添加一个或更多元素，会修改原数组              | 返回新的数组长度   |
| shift()             | 删除数组的第一个元素，数组长度减 1，无参数，会修改原数组  | 返回第一个元素的值 |

## 数组排序、翻转

| 方法名    | 说明                                                         | 补充                               |
| --------- | ------------------------------------------------------------ | ---------------------------------- |
| reverse() | 翻转数组中元素的顺序，无参数                                 | 该方法会改变原来的数组，返回新数组 |
| sort()    | 对数组的元素进行排序，并返回数组（冒泡排序）。默认排序顺序是在将元素转换为字符串，然后比较它们的 UTF-16 代码单元值序列时构建的 | 该方法会改变原来的数组，返回新数组 |

sort()方法排序问题

- 要比较数字而非字符串，比较函数可以简单的以 a 减 b（升序） 或 b 减 a（降序）

  ```javascript
  let arr = [13, 4, 77, 1, 7];
  // 默认排序
  arr.sort();	// 输出结果：[1, 13, 4, 7, 77]
  
  // 比较函数
  arr.sort(function(a, b){
    //return a - b;	// 升序	[1, 4, 7, 13, 77]
      return b - a;	// 降序	[77, 13, 7, 4, 1]
  })
  ```

## 数组索引方法

| 方法名        | 说明                                                         | 返回值                                    |
| ------------- | ------------------------------------------------------------ | ----------------------------------------- |
| indexOf()     | 返回该数组元素的索引号，从前面开始查找，如果数组存在相同元素，则返回第一个满足条件的索引号 | 如果存在返回索引号，如果不存在，则返回 -1 |
| lastIndexOf() | 返回该数组元素的索引号，从后面开始查找，如果数组存在相同元素，则返回最后一个满足条件的索引号 | 如果存在返回索引号，如果不存在，则返回 -1 |

```javascript
let arr = ['red', 'green', 'blue', 'pink', 'blue'];
console.log(arr.indexOf('blue'));	// 输出结果：2

console.log(arr.lastIndexOf('blue'));	// 输出结果：4
```

## 数组转换为字符串

| 方法名           | 说明                                                         | 返回值         |
| ---------------- | ------------------------------------------------------------ | -------------- |
| toString()       | 把数组转换成字符串，逗号分隔每一项                           | 返回一个字符串 |
| join(' 分隔符 ') | 方法用于把数组中的所有元素转换为一个字符串<br>通过参数类名指定的分隔符进行分隔的，分隔符省略则默认逗号分隔 | 返回一个字符串 |

```javascript
// toString()数组转换为字符串
let arr1 = [1, 2, 3];
console.log(arr.toString());	// 1,2,3

// join()
let arr2 = ['red', 'blue', 'green']
console.log(arr.join());	// red,blue,green
// 指定分隔符
console.log(arr.join('-'))	// red-blue-green
console.log(arr.join('&'))	// red&blue&green
// 空字符串('')，则所有元素之间都没有任何字符
console.log(arr.join(''));	// redbluegreen
```

## 截取或合并数组

| 方法名   | 说明                                                         | 返回值                       |
| -------- | ------------------------------------------------------------ | ---------------------------- |
| concat() | 用于合并两个或多个数组。此方法不会更改现有数组               | 返回一个新数组               |
| slice()  | 数组截取slice(begin,end)，由 `begin` 和 `end` 决定的原数组的**浅拷贝**（包括 `begin`，不包括`end`），原始数组不会被改变 | 返回被截取的元素的新数组对象 |
| splice() | 通过删除或替换现有元素或者原地添加新的元素来修改数组，此方法会改变原数组 | 数组形式返回被修改的内容     |

splice()方法（重点）

```javascript
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
console.log(months);    // ['Jan', 'Feb', 'March', 'April', 'June']


months.splice(4, 1, 'May');
console.log(months);    // ['Jan', 'Feb', 'March', 'April', 'May']

```

## Array.from 伪数组转为数组

`Array.from()`可以将伪数组转为数组

`Array.from()`可以接收一个伪数组作为参数，返回一个数组

```javascript
Array.from(arrayLike, [mapFn]);
```

> arrayLike：想要转换成数组的类数组或可迭代对象
>
> mapFn：调用数组每个元素的函数。如果提供，每个将要添加到数组中的值首先会传递给该函数，然后将 `mapFn` 的返回值增加到数组中。使用以下参数调用该函数：
>
> 1. element：数组当前的项的值
> 2. index：数组当前项的索引

```javascript
let arrLike = {
    "0": "1",
    "1": "2",
    "2": "3",
    "length": 3
};
let newAry = Array.from(arrLike, item => item *2);
console.log(newAry);		// [2, 4, 6]
```

## 返回满足条件的元素

### find

`find`用返回数组中满足函数中的第一个元素的值，否则返回undefined

```javascript
find(执行的函数([数组当前项的值],[数组当前项的索引],[数组对象本身]));
```

```javascript
let arr = [{
	id: 1,
	name: '张三'
	}, {
		id: 2,
		name: '李四'
}];
let target = arr.find(value => value.id == 2);
console.log(target);    // {id: 2, name: '李四'}
```

### findIndex

`findIndex`用于找出第一个符合条件的数组元素的位置，如果没有找到返回 -1

```javascript
findIndex(执行的函数([数组当前项的值],[数组当前项的索引],[数组对象本身]))
```

```javascript
let arr = [10, 20, 60];
let index = arr.findIndex(value => value > 15);
console.log(index);		// 1
```

### some

`some `方法用于检测数组中的元素是否满足指定条件

```javascript
array.some(执行的函数(数组当前项的值, 数组当前项的索引, 数组对象本身));
```

**some 的返回值是布尔值，如果查找到这个元素，就返回true，如果查找不到就返回 false**

**如果找到第一个满足条件的元素，则终止循环，不再继续查找**

在 some 里面遇到 return true 就是终止遍历

```javascript
let arr = [30, 3, 44, 67, 23];
let flag = arr.some(function(value){
    return value >= 20;
})
console.log(flag);      // true



let arr1 = ['red', 'blue', 'pink'];
let flag1 = arr1.some(function(value){
    return value == 'green';
})
console.log(flag1);     // false
```


## 判断数组是否包含指定的值

`includes()`方法用来判断一个数组是否包含一个指定的值，如果包含则返回 `true`，否则返回 `false`

`includes()`比较字符串和字符时是区分大小写的

```javascript
includes(需要查找的元素值, [index]);
```

> index：从 index 索引处开始查找 element。如果为负值，则按升序从 `array.length + index` 的索引开始搜（即使从末尾开始往前跳 index 的绝对值个索引，然后往后搜寻）。默认为 0

如果 index 大于等于数组的长度，则将直接返回 `false`，且不搜索该数组

```
// 数组是否包含 2
[1, 2, 3].includes(2);	// true
// 数组是否包含 4
[1, 2, 3].includes(4);	// false
```

## 遍历数组方法

### forEach迭代

`forEach` 可以遍历数组**处理数据**，没有返回值

```javascript
  array.forEach(执行的函数([数组当前项的值], [数组当前项的索引], [数组对象本身]))
```

在 forEach 里面遇到 return true 不会终止迭代（退出 return true 本次循环，但没有结束整个循环）

**如果不带任何形参，则根据数组的长度循环输出数组本身**

```javascript
  // forEach迭代（遍历）数组
  let arr = [1, 2, 3];
  arr.forEach(function () {
      console.log(arr);	// [1, 2, 3]	[1, 2, 3]	[1, 2, 3]
  })
```

forEach迭代

```javascript
  // forEach迭代（遍历）数组
  let arr = [1, 2, 3];
  // 数组元素的累加（定义变量）
  let sum = 0;
  arr.forEach(function(value, index, array){
      console.log('每个数组元素：' + value);
      console.log('每个数组元素的索引号：' + index);
      console.log('数组本身：' + array);
      sum += value;
  })
  console.log('累加的和为：' + sum);		// 累加的和为：6
```

  - 运行

    ![](http://127.0.0.1:8888/uploads/202304081650191.png) 

### map遍历

`map`可以遍历数组**处理数据**，并**且返回新的数组**

> map也称为映射。映射是个术语，指两个元素的集之间相互 “ 对应 ” 的关系

```javascript
map(执行的函数([数组当前项的值],[数组当前项的索引],[数组对象本身]));
```

```javascript
 let arr = ['red', 'blue', 'pink'];
 const newArr = arr.map(function(ele, index, ary){
 // console.log(ele);       // 数组当前的元素
 // console.log(index);     // 数组当前的索引
 // console.log(ary);       // 数组对象本身
 return ele + '颜色';
 })
 console.log(newArr);	 // ['red颜色', 'blue颜色', 'pink颜色']
```

### filter筛选

`filter `方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素，**主要用于筛选数组**

```javascript
  array.filter(执行的函数([数组当前项的值], [数组当前项的索引], [数组对象本身]));
```

**filter() 查找满足条件的元素，返回一个新数组，而且是把所有满足条件的元素返回回来**

filter 里面 return 不会终止迭代

```javascript
// filter()筛选数组
let arr = [3, 90, 47, 20, 10];
let newArr = arr.filter(function(value){
    return value % 2 === 0;     // 筛选出数组所有的偶数
});
console.log(newArr);    // [90, 20, 10]
```


## reduce

`reduce()` 返回累**计处理的结果**，经常用于**求和等**

```javascript
数组.reduce(function(){}, 起始值);
数组.reduce(function(上一次值, 当前值, 索引){}, 初始值);
```

如果**有起始值**，则把初始值累加到里面

```javascript
const arr = [1, 5, 8];
// 没有初始值
const total = arr.reduce(function(prev, current) {
    return prev + current
});
console.log(total);		// 14


// 有初始值
const sum = arr.reduce(function(prev, current) {
    return prev + current
}, 10);
console.log(sum);		// 24
```

## every 指定条件检测数组所有元素

`every()`检测数组所有元素是否都符合指定的条件，如果所有元素都通过检测返回 `true`，否则返回 `false`

```javascript
数组.every(执行的函数(数组当前项的值,[数组当前项的索引],[数组对象本身]))
```

```javascript
const isArr = (element, index, arr) => element >= 10;
const arr = [12, 20, 40, 10, 66];
console.log(arr.every(isArr));		// true

// 在稀疏数组上使用 every()
// every() 不会在空槽上运行它的断言函数
console.log([1, , 3].every((x) => x !== undefined)); // true
console.log([2, , 2].every((x) => x === 2)); // true
```
