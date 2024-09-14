## String 方法

### 字符串的特点

#### 基本包装类型

为了方便操作基本数据类型，JavaScript通过了三个特殊的引用类型：String、Number 和 Boolean

基本包装类型就是把简单数据类型包装成为复杂数据类型，这样基本数据类型就有了属性和方法

```javascript
let str = 'zs'
console.log(str.length);	// 2
```

按道理基本数据类型是没有属性和方法的，而对象才有属性和方法，但上面代码可以执行，这是因为 js 会把基本数据类型包装为复杂数据类型，其执行过程如下：

```javascript
// 1、生成临时变量，把简单类型包装为复杂数据类型
let temp = new String('zs');
// 2、赋值给声明的字符变量
str = temp;
// 3、销毁临时变量
temp = null;
```

#### 字符串的不可变

指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内容空间

```javascript
let str = 'abc';
str = 'hello';
// 当重新给 str 赋值的时候，常量 'abc' 不会被修改，依然在内存中
// 重新给字符串赋值，会重新在内存中开辟空间，这个特点就是字符串的不可变
// 由于字符串的不可变，在大量拼接字符串的时候会有效率问题

let str = '';
for (let i = 0; i < 100000; i++){
    str += 1;
}
console.log(str);	// 这个结果需要花费大量时间来显示，因为需要不断的开辟新的空间
```

### 根据字符串返回位置

字符串所有的方法，都不会修改字符串本身（字符串是不可变的），操作完成会返回一个新的字符串

| 方法名                                  | 说明                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| indexOf('要查找的字符', [起始位置])     | 返回指定内容在字符串中的位置，如果找不到就返回 -1，开始的位置是 index 索引号 |
| lastIndexOf('要查找的字符', [起始位置]) | 从后往前找，只找第一个匹配的                                 |

- 备注

  ```javascript
  // 如果查询为字符串第一个元素或空，都返回 0
  'abcd'indexOf('a');		// 0
  'abcd'indexOf('');		// 0
  ```


### 根据位置返回字符（重点）

| 方法名            | 说明                                     |
| ----------------- | ---------------------------------------- |
| charAt(index)     | 返回指定位置的字符(index 字符串的索引号) |
| charCodeAt(index) | 获取指定位置的字符的ASCII码(index索引号) |
| str[index]        | 获取指定位置的字符                       |

```javascript
let str = 'zhangsan';
// charAt(index) 根据位置返回字符
console.log(str.charAt(3))	// n

// charCodeAt(index) 返回相应索引号的字符ASCII值
console.log(str.charCodeAt(0));  // 97

// str[index]	H5新增
console.log(str[0]);	// z

```

### 拼接以及截取字符串（重点）

| 方法名                        | 说明                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| concat(str1, str2, str3, ...) | concat()方法用于连接两个或多个字符串。拼接字符串，等效于 +   |
| substr(start, length)         | 从start位置开始（索引号），length取得的  个数                |
| slice(start, end)             | 从start位置开始，截取到end位置，end位置的字符串取不到（start和end都是索引号） |
| substring(start, end)         | 从start位置开始，截取到end位置，end位置的字符串取不到，基本和slice相同，但是不接受负值 |

### 替换字符串以及转换为数组

| 方法名                                          | 说明                                                         |
| ----------------------------------------------- | ------------------------------------------------------------ |
| replace( ' 被替换的字符 ' ,  ' 替换为的字符 ' ) | 替换指定的字符，如果有相同字符，则只替换第一个字符           |
| split( ' 分隔符 ' )                             | 字符转换为数组。注意：原先的字符串变量还是字符串，是没有变的 |

```javascript
let str = 'zhangsan';
console.log(str.replace('a','e'));		// zhengsan

let color1 = 'red,blue,green';
console.log(str.split(','));		// ['red,blue,green']

let color2 = 'red&blue&green';
console.log(str.split('&'));		// ['red,blue,green']
```

### 字符串转换大小写

| 方法名        | 说明                                                         | 返回值                                     |
| ------------- | ------------------------------------------------------------ | ------------------------------------------ |
| toUpperCase() | 将调用该方法的字符串转为大写形式并返回（如果调用该方法的值不是字符串类型会被强制转换） | 一个新的字符串，表示转换为大写的调用字符串 |
| toLowerCase() | 将调用该方法的字符串值转为小写形式，并返回                   | 一个新的字符串，表示转换为小写的调用字符串 |

- 以上方法不会影响原字符串本身的值，因为 JavaScript 中字符串的值是不可改变的

### 判断指定的字符串是否在头部或结尾

| 方法名       | 说明                               | 返回值                                    |
| ------------ | ---------------------------------- | ----------------------------------------- |
| startsWith() | 表示参数字符串是否在原字符串的头部 | 布尔值，满足条件返回 true，否则返回 false |
| endWith()    | 表示参数字符串是否在原字符串的尾部 | 布尔值，满足条件返回 true，否则返回 false |

```javascript
let str = 'Hello world!';
// 判断str字符串是否以 Hello 开头
str.startsSith('Hello');		// true

// 判断str字符串是否以 ! 结尾
str.endsWith('!');		// true
```

### 根据指定次数重复字符串

- `repeat()`将原字符串重复 n 次，并且返回一个新字符串

  ```javascript
  str.repeat(n)		// n 为重复的次数
  ```
  
  ```javascript
  'x'.repeat(3)		// "xxx"
  'hello'.repeat(2)	// "hellohello"
  ```

### trim 去除两端空格

- `trim()`方法会从一个字符串的两端删除空白字符

  ```javascript
  str.trim();
  ```
  
  - trim() 方法并不影响原字符串本身，它返回的是一个新的字符串
  - trim() 方法不能去除字符串中间的空格
  
  ```javascript
  // trim 方法去除字符串两侧空格
  let str = '  zhang  san  '
  console.log(str);		//   zhang  san  
  
  // 使用trim 方法
  let str1 = str.trim();
  console.log(str1);		//zhang  san
  ```
