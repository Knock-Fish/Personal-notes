# main函数

1. main函数是程序的入口

2. main函数有且仅有一个

3. 一个工程中main函数有且仅有一个

```c
//std - 标准	i - input	o - output

#include <stdio.h>
// 标准的主函数写法
int main()
{
    printf("hello");	// printf是一个库函数，专门用来打印数据的
    return 0
}
```

# 数据

## 数据类型

| 类型      | 说明         | 占用的字节    |
| --------- | ------------ | ------------- |
| char      | 字符数据类型 | 1字节 / 8bit  |
| short     | 短整型       | 2字节 / 16bit |
| int       | 整型         | 4字节 / 32bit |
| long      | 长整型       | 4字节 / 32bit |
| long long | 更长的整型   | 8字节 / 64bit |
| float     | 单精度浮点数 | 4字节 / 32bit |
| double    | 双精度浮点数 | 8字节 / 64bit |

每种数据类型的大小

```c
#include <stdio.h>
int main()
{
	printf("%zu\n", sizeof(char));			// 1	8bit
	printf("%zu\n", sizeof(short));			// 2	16bit
	printf("%zu\n", sizeof(int));			// 4	32bit
	printf("%zu\n", sizeof(long));			// 4	32bit
	printf("%zu\n", sizeof(long long));		// 8	64bit
	printf("%zu\n", sizeof(float));			// 4	32bit
	printf("%zu\n", sizeof(double));		// 8	64bit
    
    return 0;
}
```

> 计算机中的单位
>
> 1. bit 比特位
> 2. byte 字节（8个比特位 = 1个字节）
> 3. kb（1024字节 = 1kb）
> 4. mb（1024kb = 1mb）
> 5. gb（1024mb = 1gb）
> 6. tb（1024gb = 1tb）
> 7. pb（1024tb = 1pb）

格式化打印

| 打印 | 说明                 |
| ---- | -------------------- |
| %d   | 打印整型             |
| %c   | 打印字符             |
| %s   | 打印字符串           |
| %f   | 打印float类型的数据  |
| %lf  | 打印double类型的数据 |
| %zu  | 打印sizeof的返回值   |

## 数据在内存中的存储

### 类型的基本归类

整型家族（字符的本质是ASCII码值，是整型，所以划分到整型家装）

```c
char
	unsigned char
	signed char
short
	unsigned short [int]
	signed short [int]
int
	unsigned int	 // unsigned int表示无符号
	signed int	// 平时声明int a; 等价于 signed int a;
long
	unsigned long [int]
	signed long [int]
long long
	unsigned long [int]
	signed long [int]
```

> 如果表示有正负的值，可以使用signed int
>
> 如果表示没有负数的值，可以使用unsigned int（统一正数）
>
> 无符号的char的取值范围是0~255

浮点型家族

```
float
double
```

构造类型（自定义类型）

```
数组类型
结构体类型 struct
枚举类型 enum
联合类型 union
```

指针类型

```
int *pi;
char *pc;
float* pf;
void* pv;
```

空类型

> void表示空类型（无类型）
>
> 通常应用于函数的返回类型、函数的参数、指针类型

```c
// 第一个void表示函数不会返回值
// 第二个void表示函数不需要传任何参数
#include <stdio.h>
void test(void)
{
	printf("abc\n");	
}
int main()
{
    test();
    return 0;
}
```

### 整形在内存中的存储

整数内存中存放的是补码的二进制序列

整数的2进制表示也有三种表示形式：

> 1. 正的整数，原码、反码、补码相同
> 2. 负的整数，原码、反码、补码是需要计算的

#### 原码、反码、补码

计算机中的整数有三种2进制表示方法，即原码、反码和补码

三种表示方法均有**符号位**和**数值位**两部分，符号位都是用 0 表示“正”，用1表示“ 负 ”，而数值位：

> 正数的原、反、补码都相同
>
> 负整数的三种表示方法各不相同
>
> 1. 原码：直接将数值按照正负数的形式翻译成二进制序列就是原码
> 2. 反码：原码的符号位不变，其他位依次按位取反得到的就是反码
> 3. 补码：反码+1就是补码

**对于整形来说：数据存放内存中其实存放的是补码**

为什么呢

> 在计算机系统中，数值一律用补码来表示和存储。原因在于，使用补码，可以将符号位和数值域统一处理
>
> 同时，加法和减法也可以统一处理（CPU只有加法器）此外，补码与原码相互转换（原码可以通过取反加1得到补码，相反，也可以通过补码取反加1得到原码），其运算过程是相同的，不需要额外的硬件电路

### 大小端

大端字节序存储：把一个数据的高位字节序的内容存放在**低地址**处，把低位字节序的内容放在**高地址**处，就是大端字节序存储

小端字节序存储：把一个数据的高位字节序的内容存放在**高地址**处，把低位字节序的内容放在**低地址**处，就是小端字节序存储

### 浮点型在内存中的存储

常见的浮点数

> 3.14159
>
> 1E10
>
> 浮点数家族包括：float、double、long double 类型
>
> 浮点数表示的范围：float.h中定义

#### 浮点数存储规则

根据国际标准IEEE（电气和电子工程协会）754，任意一个二进制浮点数V可以表示成下面的形式；

> - (-1)^S * M * 2^E
> - (-1)^S表示符号位，当S=0，V为正数；当S=1，V为负数
> - M表示有效数字，大于等于1，小于2
> - 2^E表示指数位

例如

十进制的5.0，写出二进制是 `101.0`，相当于`1.01x2^2`

那么，按照上面V的格式，可以得出S=0，M=1.01，E=2

十进制的 - 5.0，写成二进制是`-101.0`，相当于`-1.01x2^2`。

那么，S=1，M=1.01，E=2



IEEE 754规定：

对于32位的浮点数，最高的 1 位是符号位S，接着的 8 位是指数E，剩下的 23 位为有效数字M



对于64位的浮点数，最高的 1 位是符号位S，接着的 11 位是指数E，剩下的52位为有效数字M



**IEEE 754对有效数字M和指数E，还有一些特别规定**

`1≤M<2`也就是说，M可以写成1.xxxxxx的形式，其中xxxxxx表示小数部分

IEEE 754规定，在计算机内部保存M时，默认这个数的第一位总是1，因此可以被舍去，只保存后面的xxxxxx部分。

比如保存1.01的时候，只保存01，等到读取的时候，再把第一位的1加上去，这样做的目的，是节省1位有效数字。以32位浮点数为例，留给M只有23位

将第一位的 1 舍去以后，等于可以保存24位有效数字



**指数E的情况**

**E为一个无符号整数（unsigned int）**

这意味着，如果E为8位，它的取值范围为 0\~255；如果E为11位，它的取值范围为0~2047。但是，科学计数法中的E是可以算出现负数的，所以IEEE 754规定，存入内存时E的真实值必须再加上一个中间数，对于8位的E，这个中间数是 127；对于11位的E，这个中间数是1023。比如，2^10的E是10，所以保存成32位浮点数时，必须保存成10 + 127 = 137，即10001001

```
V = 0.5f
  = 0.1
  = 1.0 * 2^-1
  = (-1)^0 * 1.0 * 2^(-1)
  	S=0		M=1.0	E=-1
    
    float -> E(真实值)+127(中间值) -> 126 - 存储
    double -> E(真实值)+1023(中间值) -> 1022 - 存储
```

指数E从内存中取出还可以再分成三种情况：

**E不全为0或不全为1**

> 这时，浮点数就采用下面的规则表示，即指数E的计算值减去127（或1023），得到真实值，再将有效数字M前加上第一位的1
>
> 比如：
>
> 0.5（1/2）的二进制形式为0.1，由于规定正数部分必须为1，即将小数点右移1位，则为
>
> 1.0*2^(-1)，其阶码为 -1+127=126，表示为
>
> 01111110，而尾数1.0去掉整数部分为0，补齐0到23位0000000000000000000，则其二进制表示形式为：
>
> ```c
> 0 01111110 0000000000000000000
> ```

**E全为0**

> 这时，浮点数的指数E等于1-127（或者1-1023）即为真实值
>
> 有效数字M不再加上第一位的1，而是还原为0.xxxxxx的小数。这样做是为了表示±0，以及接近于0的很小的数字

**E全为1**

> 这时，如果有效数字M全为0，表示±无穷大（正负取决于符号位S）

# 变量、常量

## 变量

> 不变的值，C语言中用**常量**的概念来表示，变得值C语言中用**变量**来表示
>
> 创建变量的本质是向内存申请空间

定义变量的方法

```c
// 初始化（创建变量同时赋值，不赋值默认值为随机值）
char ch = 'w';
int age = 20;
double price = 66.6;
```

变量的分类

> 1. 局部变量 - { }内部定义的变量
> 2. 全局变量 - { } 外部定义的变量

```c
#include <stdio.h>
int global = 2023;	// 全局变量
int main()
{
	int local = 2022;	// 局部变量
	int global = 2024;	// 局部变量
    // 当全局变量和局部变量名字相同的情况下，局部优先
	printf("global = %d\n", global);	// global = 2024
	return 0;
}
```

## 变量的作用域和生命周期

作用域（scope）

> 是程序设计概念，通常来说，一段程序代码中所用到的名字并不总是有效/可用的
>
> 而限定这个名字的可以性的代码范围就是这个名字的作用域：
>
> 1. 局部变量的作用域是变量所在的局部范围
> 2. 全局变量的作用域是整个工程（指包括extern声明来自外部的符号）

```c
int a = 10;	// 全局变量
void test()
{
	printf("test--->%d\n", a);	// test--->10
}
int main()
{
	int b = 20;	// b的作用域就是b所在的局部范围
	test();
	{
		printf("a=%d\n", a);	// a=10
	}
	printf("b=%d\n", b);	// b=20

	return 0;
}
```

生命周期

> 变量的生命周期指的是变量的创建到变量的销毁之间的一个时间段
>
> 1. 局部变量的生命周期是：进入作用域生命周期开始，出作用域生命周期结束
> 2. 全局变量的生命周期是：整个程序的生命周期

## 常量

C语言中的常量和变量的定义的形式有所差异

常量是一种类型（常量符号），只有当使用这个类型创建变量时，才会向内存申请空间

C语言中的常量分为以下几种：

1. 字面常量

   ```c
   int main()
   {
       // 1、字面常量
   	30;
       'w';	// 字符
       "abc";
   }
   ```

2. `const`修饰的常变量

   ```c
   int main()
   {
       // const 修饰的常变量（有常量属性的变量）
       const int a = 10;	// 在C语言中，const修饰的，本质是变量，但不能直接修改，有常量的属性
   }
   ```

3. `#define` 定义的标识符常量

   ```c
   // #define 定义的标识符常量
   #define MAX 100
   #define STR "abcd"
   int main()
   {
       printf("%d\n", MAX);	// 100
       int a = MAX;
       printf("%d\n", a);	// 100
       printf("%s\n", STR);	// abcd
       return 0;
   }
   ```

4. 枚举常量

   ```c
   // 枚举常量
   enum Color	// 常量符号创建的类型，当只有使用这个类型去创建变量时，才会申请内存空间
   {
       // 枚举常量
       RED,
       GREEN,
       BLUE
   };
   
   int main()
   {
       enum Color c = RED;	// 创建
       
       return 0;
   }
   ```

# 字符串+转义字符+注释

## 字符串

由双引号（Double Quote）引起来的一串字符为字符串字面值（String Literal） ,或者简称字符串（<span style="color:red">在C语言中是没有字符串类型的</span>）

```c
"hello bit.\n"
```

> 注：**字符串的结束标志**是一个`\0`的转义字符。在计算字符串长度时`\0`是结束标志，不算作字符串内容

```c
int main() 
{
	char arr1[] = "abcdef";		// 7
	char arr2[] = {'a','b','c','d','e','f'};
	// char arr2[] = {'a','b','c','d','e','f','\0};	// 手动添加\0

	printf("%d\n",strlen(arr1));	// 6
	printf("%d\n",strlen(arr2));

	printf("%s\n", arr1);
	printf("%s\n", arr2);

	return 0;
}
```

![](http://127.0.0.1:8888/uploads/202307071527558.png)

## 转义字符

| 转义字符 | 释义                                                         |
| -------- | ------------------------------------------------------------ |
| `\?`     | 在书写连续多个问号时使用，防止它们被解析成三字母词           |
| `\'`     | 用于表示字符常量 '                                           |
| `\"`     | 用于表示一个字符串内部的双引号                               |
| `\\`     | 用于表示一个反斜扛，防止它被解释为一个转义序列符             |
| `\a`     | 警告字符，蜂鸣                                               |
| `\b`     | 退格符                                                       |
| `\f`     | 进纸符                                                       |
| `\n`     | 换行                                                         |
| `\r`     | 回车                                                         |
| `\t`     | 水平制表符                                                   |
| `\v`     | 垂直制表符                                                   |
| `\ddd`   | ddd表示1~3个八进制的数字。如：\130 会打印十进制对应的ascii编码字符 ：X |
| `\xdd`   | dd表示2个十六进制数字。如：\x30 会打印十进制对应的ascii编码字符 ：0 |
| `\0`     | 字符串结束标志                                               |

## 注释

代码中有不需要的代码可以直接删除，页可以注释掉

代码中有些比较难懂，可以加一下注释文字

```c
int main()
{
    // int num = 10;
    
    /*
    	char ch1 = 'w';
    	char ch2 = 'f';
    */
	return 0;
}
```

> 注释有两种风格：
>
> - C语言风格的注释`/* xxxx */`
>   - 缺陷：不能嵌套注释
> - C++风格的注释`// xxxx`
>   - 可以注释一行也可以注释多行

# 什么是语句

C语句可分为以下五类：

1. 表达式语句
2. 函数调用语句
3. 控制语句
   - 控制语句用于控制程序的执行流程，以实现程序的各种结构方式，它们由特定的语句定义符组成，C语言有九种控制语句
     - 可以分为以下三类:
       1. 条件判断语句也叫分支语句：if语句、switch语句
       2. 循环执行语句：do while语句、while语句、for语句
       3. 转向语句：break语句、goto语句、contine语句、return语句
4. 复合语句
5. 空语句

# 分支语句（选择结构）

C语言实现选择

```c
if else
switch ...
```

```c
#include <stdio.h>
int main()
{
	int coding = 0;
	printf("你会去学习嘛？（选择1 or 0）:>");
    scanf("%d",&coding);
    if(coding == 1)
    {	// 多条语句使用大括号包裹
        printf("请继续坚持下去！\n");
        printf("加油！\n");
    }
    else
    {
        printf("请赶快去学习！\n");
        printf("加油！\n");
    }
    return 0;
}
```

## if语句

if语句中表达式的结果为真，则语句执行

> 0表示假，非0表示真

语法格式

```c
if(表达式)
	语句;


if(表达式)
    语句1;
else
    语句2;


// 多分支
if(表达式)
    语句1;
else if(表达式)
    语句2;
else
    语句3;
```

如果条件成立，要执行多条语句，使用 { } 括起来

```c
int main()
{
    if(表达式)
    {
        语句列表1;
    }
    else
    {
        语句列表2;
    }
    return 0;
}
```

> else的匹配：else是和它离的最近的if匹配的

## switch 语句

语法格式

```c
switch(整型表达式)
{
	// 语句项
    case 整形常量表达式:
        语句;
        break;
}
```

break语句的实际效果是把语句列表划分为不同的分支部分

```c
switch(1+1)
{
    case 1:
    case 2:
    case 3:
        printf("hi\n");
       	break;
    case 4:
        printf("hehe\n");
        break;
}
```

### default子句

> - 当`switch`表达式的值并不匹配所有`case`标签的值时，这个`default`子句后面的语句就会执行
> - 每个`switch`语句中只能出现一条`default`子句
> - 但是它可以出现在语句列表的任何位置，而且语句流会像执行一个case标签一样执行default子句

# 循环语句

> C语言实现循环的方式

## while 语句

语法

```c
// while 语法结构
while(表达式)
    循环语句;
```

```c
int main()
{
    int line = 0;
    while (line <= 100)
    {
        printf("行：%d\n",line);
        line++;
    }
}
```

## for 语句

语法

```c
for(表达式1; 表达式2; 表达式3)
    循环语句;
```

> - 表达式1：为初始化部分，用于初始化循环变量的
> - 表达式2：为条件判断部分，用于判断循环时候终止
> - 表达式3：为调整部分，用于循环条件的调整

```c
int main()
{
    int i = 0;
    for (i = 1; i <= 10; i++)
    {
        printf("%d", i);
        printg("hi\n");
    }
}
```

`for`循环的判断部分省略意味这判断会恒成立（死循环）

```c
for(;;)
{
    printf("hi\n")
}
```

## do ... while 语句

语法

```c
do
    循环语句;
while(表达式);
```

> `break`和`continue`在`for`循环中

# 数组

## 一维数组

### 一维数组的创建

C语言中给了数组的定义：一组相同类型元素的集合

数组的创建方式：

```c
type_t arr_name [const_n];
// type_t 是指数组的元素类型
// const_n 是一个常量表达式，用来指定数组的大小

// 定义一个整形数组，最多放10个元素
int arr[10] = {1,2,3,4,5,6,7,8,9,10};
```

```c
// 代码1
int arr[10];

// 代码2
int count = 10;
// 这样创建的数组不能初始化
int arr2[count];	// 数组创建，在C99标准之前，[]中要给一个常量才可以，不能使用变量。在C99标准支持了变长数组的概念
```

### 一维数组的初始

数组的初始化是指，在创建数组的同时给数组的内容以下合理初始值（初始化）

```c
// 不完全初始化，剩余的元素默认初始化为0
int arr1[10] = {1,2,3};
int arr2[] = {1,2,3,4};
int arr2[5] = {1,2,3,4,5};
char arr4[3] = {'a', 98, 'c'};
char arr5[] = {'a','b','c'};
char arr6[] = "abcdef";
```

数组在创建的时候如果想不指定数组的确定的大小就得初始化。数组的元素个数根据初始化的内容来确定。

但是对于下面的代码要区分，内存中如何分配

```c
char arr1[] = "abc";
char arr[3] = {'a','b','c'};
```

### 一维数组的使用

> 1. 数组是使用下标来访问的，下标是从0开始
>
> 2. 数组可以通过下标来访问的
>
> 3. `[]`下标引用操作符，它其实就是数组访问的操作符
>
> 4. 数组的大小可以通过计算得到

```c
int arr[10] = {1,2,3,4,5,6,7,8,9,10};
// 分别对应的下标是：0 1 2 3 4 5 6 7 8 9

// 计算数组的元素个数
int sz = sizeof(arr) / sizeof(arr[0]);	// 10
// sizeof(arr)返回所有数值元素的总字节（40字节），除以数组首元素字节（4字节）
```

遍历数组

```c
int main()
{
	int i = 0;
	int arr[10] = {1,2,3,4,5,6,7,8,9,10};
	while(i < 10){
		printf("%d\n",arr[i]);
        i++;
	}
    return 0;
}
```

### 一维数组在内存的存储

数组在内存是连续的一块空间

随着数组下标的增长，元素的地址，也在有规律的递增：数组在内存中是连续存放的

```c
#include <stdio.h>
int main()
{
    int arr[10] = {1,2,3,4,5,6,7,8,9,10};
    // 计算数组的元素个数
    int sz = sizeof(arr) / sizeof(arr[0]);
    // 对数组内容赋值，数组是使用下标来访问的，下标从0开始
    int i = 0;
    for(i = 0; i < sz; i++)
    {
        printf("&arr[%d] = %p\n", i, &arr[i]);
    }
    return 0;
}
```

## 二维数组

### 二维数组的创建

```c
// 数组创建
// 1 2 3 4
// 2 3 4 5
// 3 4 5 6
int arr[3][4];	// 3表示行，4表示列（一行可以放4个字符）
char arr[3][5];
double arr[2][4];
```

### 二维数组的初始化

```c
// 数组初始化
// 1 2 3 4
// 0 0 0 0
// 0 0 0 0
int arr[3][4] = {1,2,3,4};	// 元素依次放入，剩下的空位则补零
int arr[3][4] = {1,2,3,4,2,3.4.5.3.4.5,6};

// 1 2 0 0
// 3 4 0 0
// 5 6 0 0
int arr[3][4] = {{1,2},{3,4},{5,6}};	// 可以先给元素进行分组，不够的地方补零

// 1 2 3 4
// 4 5 0 0
int arr[][4] = {{1,2,3,4},{4,5}};	// 二维数组如果有初始化，行可以省略，列不能省略
```

### 二维数组的使用

二维数组的使用也是通过下标的方式

```c
int arr[3][4] = {1,2,3,4,2,3,4,5,3,4,5,6};
int i = 0;
for(i = 0; i < 3; i++)
{
    int j = 0;
    for(j = 0; j < 4; j++)
    {
        printf("%d ", arr[i][j]);
    }
    printf("\n");
}
```

### 二维数组在内存的存储

二维数组在内存是连续的一块空间

```c
int main()
{
	int arr[3][4] = { 1,2,3,4,2,3,4,5,3,4,5,6 };
	int i = 0;
	for (i = 0; i < 3; i++)
	{
		int j = 0;
		for (j = 0; j < 4; j++)
		{
			printf("arr[%d][%d] = %p\n", i, j, &arr[i][j]);
		}
	}
	return 0;
}
```

### 计算二维数组的行和列

```c
int main()
{
    int arr[3][4] = {0};
    printf("%d\n", sizeof(arr) / sizeof(arr[0]));	// 3
    printf("%d\n", sizeof(arr[0]) / sizeof(arr[0][0]));	// 4
}
```

## 数组越界

数组的下标是有范围限制的

数组的下规定是从0开始的，如果数组有n个元素，最后一个元素的下标就是 n-1

所以数组的下标如果小于0，或者大于 n-1，就是数组越界访问了，超出了数组合法空间的访问

C语言本身是不做数组下标的越界检查，编译器也不一定报错，但是编译器不报错，并不意味着程序就是正确的

所以程序员写代码时，最好自己做越界的检查

```c
int main()
{
    int arr[10] = {1,2,3,4,5,6,7,8,9,10};
    int i = 0;
    for(i = 0; i <= 10; i++)
    {
        printf("%d\n", arr[i]);	// 当i等于10的时候，越界访问了
    }
    return 0;
}
```

二维数组的行和列也可能存在越界

## 数组作为函数参数

代码

```c
// 数组冒泡排序
void b_sort(int arr[],int sz)
{
	int i = 0;
	for (i = 0; i < sz-1; i++)
	{
		int j = 0;
		for (j=0; j < sz-1-i; j++)
		{
			if (arr[j] > arr[j + 1])
			{
				int tmp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = tmp;
			}
		}
	}
}

int main()
{
	int arr[] = { 9,8,7,6,5,4,3,2,1,0 };
	int sz = sizeof(arr) / sizeof(arr[0]);
	b_sort(arr,sz);
	int i = 0;
	for (i = 0; i < sz; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}
```

## 数组名

数组名能表示首元素的地址

但有2个例外

1. `sizeof(数组名)`，计算整个数组的大小，sizeof内部单独放一个数组名，数组名表示整个数组，单位是字节

   ```c
   int arr[10] = {0};
   printf("%d\n", sizeof(arr));	// 40
   ```

2. `&数组名`，数组名表示整个数组，取出的是整个数组的地址

```c
int main()
{
    int arr[10] = {0};
    printf("%p\n", arr);	// arr就是首元素的地址
    printf("%p\n", arr+1);
    
    printf("%p\n", &arr[0]);	// 首元素地址
    printf("%p\n", &arr[0]+1);
    
    printf("%p\n", &arr);	// 数组的地址
    print("%p\n", &arr+1);
    
    return 0;
}
```

二维数组的数组名的理解

```c
int main()
{
	int arr[3][4] = {0};
	printf("%p\n", arr);	// 二维数组的数组名也表示数组首元素（第一行）的地址
    printf("%p\n", arr+1);	// 加一指向第二行
}
```

# 操作符

## 算术操作符

```
+ 加
- 减
* 乘
/ 整除
% 取模
```

> 1. 除了`%`操作符之外，其他的几个操作符可以作用于整数和浮点数
>
> 2. 对于`/`操作符如果两个操作符都为整数，执行整数除法。而只要浮点数执行的就是浮点数除法
>
> 3. `%`操作符的两个操作数必须为整数。返回的是整除之后的余数

## 移位操作符

移位操作符，移动的是二进制位

注：移位操作符的操作数只能是整数

```
>> 左移操作符
<< 右移操作符
```

### 左移操作符

移位规则：

> 左边丢弃、右边补0

### 右移操作符

移位规则：

首先右移运算分两种：

> 1. 逻辑移位：左边用0填充，右边丢弃
> 2. 算术移位：左边补原该值的符号位，右边丢弃

对于移位运算符，不要移动负数位，这个是标准未定义的

例如

```c
int num = 10;
num >> -1; //erroe
```

## 位操作符

```c
& 	// 按（2进制）位与 （有0则为0，同时为1才为1）
^ 	// 按（2进制）位异或（相同为0，相异为1）
|	// 按（2进制）位或	（有1则为1，同时为0才为0）
注：它们的操作数必须是整数 
```

```c
int a = 3;
int b = -5;
a & b;
a | b;
a ^ b;
// 0000 0000 0000 0000 0000 0000 0000 0011		3的补码
// 1111 1111 1111 1111 1111 1111 1111 1011		-5的补码

// 与 后的补码
// 0000 0000 0000 0000 0000 0000 0000 0011
// 或 后的补码
// 1111 1111 1111 1111 1111 1111 1111 1011
// 异或 或的补码
// 1111 1111 1111 1111 1111 1111 1111 1000
```

## 赋值操作符

```
=
```

```c
int a = 10;
a = 20;	// 重新赋值
```

连续赋值

```c
int a = 10;
int x = 0;
int y = 20;
a = x = y + 1;	// 连续赋值	（不推荐）

// 同样的语义
x = y + 1;
a = x;
```

复合赋值符

```
  +=  -=  *=  &=  ^=  |=  >>=  <<=  %=  /=
```

## 单目操作符

> 双目操作数：指有操作符有两个操作数，例如 a + b，+ 号两边都有操作数
>
> 单目操作数：指只有一个操作数

```c
!			逻辑反操作
-			负值
+			正值
&			取地址	（取出变量在内存中的起始地址）
sizeof		操作数的类型长度（以字节为单位，计算类型所创建的变量所占内存空间的大小）
~			对一个数的二进制按位取反
--			前置、后置--
++			前置、后置++
*			间接访问操作符（解引用操作符）
(类型)		强制类型转换	例如：int a = (int)3.14;	//3.14字面浮点数，编辑器默认理解为double类型
```

~是按二进制位取反

```c
int a = 0;
// ~ 是按二进制位取反
// 00000000000000000000000000000000 - 补码
// 11111111111111111111111111111111 -> ~a（补码）
// 11111111111111111111111111111110 -> 反码
// 10000000000000000000000000000001 -> 原码  -1
```

强制类型转换

```c
// 3.14会被编译器默认为double类型
int a = (int)3.14;
printf("%d\n", a);	// 3
```

sizeof

```c
int a = 0;
printf("%d\n", sizeof(a));	// 4
printf("%d\n", sizeof(int));	// 4

printf("%d\n", sizeof(a));	// 4
// printf("%d\n", sizeof(a));	//err
```

## 关系操作符

```
>
>=
<
<=
!=
==
```

## 逻辑操作符

```
&&		逻辑与
||		逻辑或
```

```c
#include <stdio.h>
int main() {
	int a1 = 3;
	int a2 = 0;
	int a3 = 2;
	// && 都真为真，一假则假
	int b1 = a1&& a2;	// 0
	int b2 = a1&& a3;	// 1
	printf("%d %d\n", b1, b2);
	// || 只要有一个为真则为真
	int b3 = a1 || a2;	// 1
	int b4 = a2 || a3;	// 1
	printf("%d %d\n",b3, b4);

	// 短路运算
	// && 左边为假，右边就不计算了
	// || 左边为真，右边就不计算了
	return 0;
}
```

## 条件操作符（三目操作符）

```
exp1 ? exp2 : exp3
```

## 逗号表达式

逗号表达式就是逗号隔开的一串表达式

逗号表达式的特点：从左向右依次计算，整个表达式的结果是最后一个表达式的结果

```c
int main()
{
	int a = 10;
    int b = 20;
    int c = 0;
    //			c = 8	a = 28		5
    int d = (c = a - 2, a = b + c, c -3);
    printf("%d\n", d);	// 5
    
    return 0;
}
```

## 下标引用、函数调用和结构成员

`[ ]`下标引用操作符

> 操作数：一个数组名 + 一个索引值

```c
int arr [10] = {0};
arr[7] = 8;
7[arr] = 9;

// arr[7] --> *(arr+7)
// arr是数组首元素的地址
// arr+7就是跳过7个元素，指向了第8个元素
// *(arr+7) 就是第8个元素
```

`( )`函数调用操作符

> 接受一个或者多个操作数：第一个操作数是函数名，剩余的操作数就是传递给函数的参数

```c
// 函数定义
int Add(int x, int y)
{
    return x + y;
}
int main()
{
    int a = 10;
    int b = 20;
    // 函数调用
    int c = Add(a, b); // ()就是函数调用操作符
}
```

访问一个结构的成员

> `.`结构体.成员名
>
> `->`	结构体指针->成员名

```c
struct Stu
{
    char name[10];
    int age;
    char gender[5];
    doule score;
};
void set_agel(struct Stu* ps)
{
    // strcpy((*ps).name,"zhangsan");
    // (*ps).age = 20;
    // (*ps).score = 100.0;
	// ps->age 等价于 (*ps).age
    strcpy(ps->name,"zhangsan");
    ps->age = 20;
    ps->score = 100.0;
};
void print_stu(struct Stu ss)
{
    printf("%s %d %lf \n",ss.name,ss.age,ss.score);
}

int main()
{
	struct Stu s = {0};
    set_stu(&s);
    print_stu(s);
    
    return 0;
}
```

# 常见关键字

```c
auto	break	case	char	const	continue	default		do		double		else	enum	extern		float	for		goto	if	int	long	register	return		short	signed		sizeof		static	struct	switch	typedef		union	unsigned	void	volatile	while
```

> C语言提供了丰富的关键字，这些关键字都是语言本身预先设定好的，用户自己是不能创造关键字的

## 关键字 typedef

> typedef 类型定义，类型重命名

比如

```c
// 将unsigned int 重命名为uint_32，所以uint_32也是一个类型名
typedef unsigned int uint uint_32;

int main()
{
    // 观察 num1 和 num2，这两个变量的类型是一样的
    unsigned int num1 = 0;
    uint_32 num2 = 0;
    return 0;
}
```

## 关键字 static

> 在C语言中：
>
> static是用来修饰变量和函数的
>
> 1. 修饰局部变量-称为静态局部变量
> 2. 修饰全局变量-称为静态全局变量
> 3. 修饰函数-称为静态函数

### 修饰局部变量

static修饰局部变量的时候，局部变量出了作用域，不销毁的。本质上，static修饰局部变量的时候，改变了变量的存储位置的

影响了变量的生命周期，生命周期变长，和程序的生命周期一样

> 一个内存包含有栈区、堆区、静态区：
>
> 1. 栈区：专门存放局部变量
> 2. 堆区：动态内存管理
> 3. 静态区：专门存放静态变量（static修饰的变量都放在静态区，静态区的变量生命周期和程序生命周期一样）

```c
#include <stdio.h>
void test()
{
    static int a = 1;
    a++;
    printf("%d",a);
}
int main()
{
    int i = 0;
    while (i < 10)
    {
        test();		// 2 3 4 5 6 7 8 9 10 11
        i++;
    }
    
    return 0;
}
```

### 修饰全局变量

static 修饰全局变量的时候，这个全局变量的外部链接属性就变成了内部链接属性。其他源文件（.c）就不能再使用到这个全局变量了

```c
// test.c文件
// 全局变量
static int g_val = 2023;

// demo.c文件
// 声明外部符号
extern int g_val;
int main()
{
    printf("%d\n",g_val);	// 报错
    
    return 0;
}
```

### 修饰函数

一个函数本来是具有外部链接属性的，但是被 static 修饰的时候，外部链接属性就变成了内部链接属性，其他源文件（.c）就无法使用了

```c
// test.c文件
// 函数
static int Add(int x, int y)
{
    return x + y;
}

// demo.c文件
// 声明外部符号
extern int Add(int x, int y);
int main()
{
    int a = 10;
    int b = 20;
    
    int z = Add(a, b);
    printf("%d\n",z);
    
    return 0;
}
```

# 函数

C语言中函数的分类：

> 1. 库函数
>
>    - 使用库函数，必须包含`#include`对应的头文件
>
> 2. 自定义函数
>
>    - 自定义函数和库函数一样，有函数名，返回值类型和函数参数
>
>    - 函数的组成

```c
ret_type fun_name(paral, *)
{
    statement;	// 语句项
}

ret_type 返回类型
fun_name 函数名
paral	函数参数
```

```c
// int返回类型	Add函数名	int x, int y函数参数
int Add(int x, int y) {
    // 函数体
	int z = 0;
	z = x + y;
	return z;
}

int main()
{
	int n1 = 0;
	int n2 = 0;
	scanf("%d %d", &n1, &n2);

	int sum = Add(n1, n2);
	printf("%d\n", sum);
}
```

## 函数的参数

> 实际参数（实参）
>
> 1. 真实传给函数的参数，叫实参
> 2. 实参可以是：常量、变量、表达式、函数等
> 3. 无论实参是何种类型的量，在进行函数调用时，它们都必须有确定的值，以便把这些值传给形参
>
> 形式参数(形参）：
>
> - 形式参数是指函数名后括号中的变量，因为形式参数只有在函数被调用的过程中才实例化（分配内存单元），所有叫形式参数。形式参数当函数调用完成之后就自动销毁了。因此形式参数只在函数中有效

## 函数的调用

传值调用

> 函数的形参和实参分别占有不同内存块，对形参的修改不会影响实参

```c
void Swap(int x, int y)
{
    int z = 0;
    z = x;
    x = y;
    y = z;
}

int main()
{
    int a = 10;
    int b = 20;
    Swap(a, b);
}
```

传址调用

> 传址调用是把函数外部创建变量的内存地址传递给参数的一种调用函数的方式
>
> 这种参数方式可以让函数和函数外边的变量建立起真正的联系，也就是函数内部可以直接操作函数外部的变量

```c
void Swap(int* x, int* y)
{
    int z = 0;
    z = x;
    x = y;
    y = z;
}

int main()
{
    int a = 10;
    int b = 20;
    Swap(&a, &b);
}
```

## 函数的嵌套调用和链式访问

函数和函数之间可以根据实际的需求进行组合的，也就是互相调用的

### 嵌套调用

```c
#include <stdio.h>
void new_line()
{
    printf("hehe\n");
}
void three_line()
{
    int i = 0;
    for(i = 0; i < 3; i++)
    {
        new_line();
    }
}
int main()
{
    three_line();
    return 0;
}
```

函数可以嵌套调用，但是不能嵌套定义

### 链式访问

把一个函数的返回值作为另外一个函数的参数

```c
int main()
{
    // 链式访问
    // strlen返回值做了printf函数的参数
    printf("%d\n", strlen("abcdef"));	// 6
    
    printf("%d",printf("%d",print("%d",43)));	// 4321
    // 先打印43，第二个打印值的字符数量：2，最后再打印 2 的字符数量：1
}
```

## 函数的声明和定义

### 函数声明

> 函数的声明一般出现在函数的使用之前。要满足先声明后使用
>
> 函数声明一般要放在头文件中的

### 函数定义

函数的定义是指函数的具体实现，交待函数的功能实现

## 函数递归

### 什么是递归

> 一种方法,它通常把一个大型复杂的问题层层转化为一个原问题相似的规模较小的问题来求解

### 递归的两个必要条件

> - 存在限制条件，当满足这个限制条件的时候，递归便不再继续
> - 每次递归调用之后越来越接近这个限制条件

# #define

## #define定义标识符

```c
// 语法
#define name stuff
```

```c
#define MAX 1000
#define reg register			// 为 register这个关键字，创建一个简短的名字
#define do_forever for(;;)		// 用更形象的符号来替换一种实现
#define CASE break;case			// 在写case语句的时候自动把break写上
// 如果定义的stuff过长，可以分成几行写，除了最后一行外，每行的后面都加一个反斜杠
#define DEBUG_PRINT printf("file:%s\tline:%d\t \
                            date:%s\ttime:%s\n" , \
                            __FILE__,__LINE__,	\
                            __DATE__,__TIME__)
```

## #define 定义宏

> #define机制包括了一个规定，允许把参数替换到文本中，这种实现通常称为宏（macro）或定义宏（define macro）

宏的声明方式

```c
#define name(parament0list) stuff
```

其中的`parament-list`是一个由逗号隔开的符号表，它们可能出现在stuff中

> 注意
>
> 1. 参数列表的左括号必须与name紧邻
> 2. 如果两者之间有任何空白存在，参数列表就会被解释为stuff的一部分

如

```c
#define SQUARE(X) ((X)*(X))	// 宏完成替换操作
int r = SQUARE(5);
printf("%d\n", r);	// 25

int k = 10*SQUARE(2);
printf("%d\n", k);	// 40

// #define SQUARE(X) X*X	// 最好加上括号，不然
// int n = SQUARE(5+1);	// int n = 5 + 1 * 5 + 1
// printf("%d\n", n);	// 11
```

## #define替换规则

> 在程序中扩展#define定义符号和宏时，需要涉及几个步骤
>
> 1. 在调用宏时，首先对参数进行检查，看看是否包含任何由#define定义的符号。如果是，它们首先被替换
> 2. 替换文本随后被插入到程序中原来文本的位置。对于宏，参数名被他们的值所替换
> 3. 最后，再次对结果文件进行扫描，看看它是否包含任何由#define定义的符号。如果是，就重复上述处理过程
>
> 注意：
>
> 1. 宏参数和#define定义中可以出现其他#define定义的符号。但是对于宏，不能出现递归
> 2. 当预处理器搜索#define定义的符号的时候，字符串常量的内容并不被搜索

```c
// define定义标识符常量
#define MAX 1000

// define定义宏
// 宏是有参数的
#define ADD(x, y) ((x)+(y))
#include <stdio.h>

int main()
{
    int sum = ADD(2, 3);
    printf("sum = %d\n", sum);
    
    sum = 10 * ADD(2, 3);
    printf("sum = %d\n", sum);
    
    return 0;
}
```

## #和##

> #把参数插入字符串中

```c
#define PRINT(N) printf("the value of "#N" is %d\n", N)
int main()
{
    int a = 10;
    PRINT(a);	// printf("the value of ""a"" is %d\n", a)
    
    int b = 20;
    PRINT(b);
}
```

##的作用

> ##可以把位于它两边的符号合成一个符号
>
> 它允许宏定义从分离的文本片段创建标识符

```c
#define CAT(Class, Num) Class##Num
int main()
{
    int Class106 = 100;
    printf("%d\n", CAT(Class, 106));	// 100
    
    return 0;
}
```

注：这样的连接必须产生一个合法的标识符。否则其结果就是未定义的

## 带副作用的宏参数

当宏参数在宏的定义中出现超过一次的时候，如果参数带有副作用，那么你在使用这个宏的时候就可能出现危险，导致不可预测的后果。副作用就是表达式求值的时候出现的永久性效果

```c
x+1;	// 不带副作用
x++;	// 带有副作用
```

```c
#define MAX(x,y) ((x)>(y)?(x):(y))
int main()
{
    int a = 5;
    int b = 4;
    int m = MAX(a++, b++);
    // int m = ((a++) > (b++) ? (a++) : (b++));
    printf("m=%d ", m);		// 6
    printf("a=%d b=%d\n", a, b);	// 7 5
    return 0;
}
```

## #undef

这条指令用于移除一个宏定义

```c
#undef NAME
// 如果现存的一个名字需要被重新定义，那么它的旧名字首先要被移除
```

```c
#define M 100
int main()
{
    printf("%d\n", M);
#undef M
    printf("%d\n", M);	// err
    
    return 0;
}
```

## 命令行定义

许多C的编译器提供了一种能力，允许在命令行中定义符号。用于启动编译过程

# 指针

## 内存

> 内存是电脑上特别重要的存储器，计算机中程序的运行都是在内存中进行的。所以为了有效的使用内存，就把内存划分成一个个小的内存单元，每个内存单元的大小是**1个字节**。为了能够有效的访问到内存的每个单元，就给内存单元进行了编号，这些编号被称为该**内存单元的地址**（地址也被称为指针）

```c
int main(){
    int a = 10;	// 向内存申请4个字节，存储10
    // &a;	//取地址操作符
    // printf("%p\n",&a);
    
    // p是指针变量	存放指针（地址）的变量就是指针变量
    int* p = &a;	
    // * 说明p是指针变量	int说明p指向的对象是int类型
    
    *p = 20;	// 解引用操作符，意思就是通过p中存放的地址，找到p所指向的对象，*p就是p指向的对象
    printf("%d\n", a);	// 20
    
    return 0;
}
```

地址的存储，需要定义指针变量

```c
int num = 10;
int *p;	// p 为一个整形指针变量
p = &num
```

> 在32位的机器上，地址是32个0或者1组成二进制序列，那地址就得用4个字节的空间来存储，所以一个指针变量的大小就应该是4个字节
>
> 那如果在64位机器上，一个指针变量的大小是8个字节，才能存放一个地址
>
> 总结：
>
> 1. 指针变量是用来存放地址的，地址是唯一标识一块地址空间的
> 2. 指针的大小在32位平台是4个字节，在64位平台是8个字节

## 指针是什么

> 指针是内存中一个最小单元的编号，也就是地址（一个单元一个字）
>
> 本质上指针就是地址
>
> 口语中说的指针，其实是指针变量，指针变量就是一个变量，指针变量用来存放地址的一个变量

## 指针变量

指针变量

> 存放的是地址，而通过这个地址，就可以找到一个内存单元
>
> 可以通过&（取地址操作符）取出变量的内存其实是地址，把地址可以存放到一个变量中，这个变量就是指针变量

```c
#include <stdion.h>
int main()
{
    int a = 10; 	// 在内存中开辟一块空间
    int *p = &a; 	// 这里对变量a，取出它的地址，可以使用&操作符
    				// a 变量占有4个字节的空间，这里是将a的4个字节的第一个低价的地址存放在p变量中，p就是一个指针变量
    return 0;
}
```

指针变量的大小

指针大小在32位平台是4个字节，64位平台是8个字节

```c
#include <stdio.h>
// 指针变量的大小取决地址的大小
// 32位平台下地址是32个bit位（即4个字节）
// 64位平台下地址是64个bit位（即8个字节）
int main()
{
    // 不管是什么类型的指针，都是在创建指针变量
    // 指针变量是用来存放地址的
    // 指针变量的大小取决于一个地址存放的时候需要多大空间
    // 32位机器上的地址：32bit位 - 4byte，
    // 64位机器上的地址：64bit位 - 8byte
    printf("%d\n", sizeof(char *));		// 4
    printf("%d\n", sizeof(short *));	// 4
    printf("%d\n", sizeof(int *));		// 4
    printf("%d\n", sizeof(double *));	// 4
    return 0;
}
```

## 指针和指针类型

```c
int a = 0x11223344;
// int* pa = &a;
// *pa = 0;

char* px = (char*)&a;	// int*
*pc = 0;

// 结论
// 指针类型决定了指针在被解引用的时候访问几个字节
// 如果是 int* 的指针，解引用访问4个字节
// 如果是 char* 的指针，解引用访问1个字节
// ...
```

```c
int a = 10;
char *pc = (char*)&a;
int *pi = &a;

printf("%p\n",&a);
printf("%p\n",pc);
printf("%p\n",pi);
// 指针加减整数的时候
// 指针的类型决定了指针向前或者向后走一步有多大
printf("%p\n",pc+1);	//char*的指针解引用就只能访问1个字节
printf("%p\n",pi+1);	//int*的指针解引用能访问4个字节
```

void* 是无具体类型的指针，可以接受任意类型的地址，所以不能解引用操作，也不能+-整数

```c
int main()
{
	int a = 10;
    // char* pa = &a; //int*
    void* pv = &a;	// void* 是无具体类型的指针，可以接受任意类型的地址
    
    return 0;
}
```

## 野指针

> 概念：野指针就是指针指向的位置是不可知的（随机的、不正确的、没有明确限制的）

野指针成因：

1. 指针未初始化

   ```c
   int *p;	// 局部变量指针未初始化，默认为随机值
   *p = 20;// 非法访问内存，这里的p就是野指针
   ```

2. 指针越界访问

   ```{c
   int arr[10] = {0};
   int *p = arr;
   int i = 0;
   for(i=0; i<=11; i++)
   {
       // 当指针指向的范围超出数组arr的范围时，p就是野指针
       *(p++) = i;
   }
   ```

3. 指针指向的空间释放

   ```c
   int* test()
   {
   	int a = 10;
       return &a;
   }
   
   int main()
   {
       int *p = test();
       return 0;
   }
   ```

## 指针运算

### 指针 +- 整数

```c
// 指针 + 整数
int arr[10] = {0}
int i = 0;
int sz = sizeof(arr) / sizeof(arr[0]);

int* p = arr;
for (i = 0; i < sz; i++) // 给arr每个地址添加1
{
    //*p++ = 0;
    *(p + i) = 1;
}

```

### 指针 - 指针

> 指针 - 指针的绝对值得到的指针和指针之间元素的个数
>
> 不是所有的指针都能相减，指向同一块空间的2个指针才能相减

### 指针的关系运算

## 指针和数组

例子

```c
#include <studio.h>
int main()
{
    int arr[10] = {1,2,3,4,5,6,7,8,9,0};
    printf("%p\n",arr);
    printf("%p\n",&arr[0]);
    return 0;
}
```

数组名和数组首元素的地址是一样的

```c
int arr[10] = {1,2,3,4,5,6,7,8,9,0};
int *p = arr; // p存放的是数组首元素的地址
// 通过指针来访问数组
int sz = sizeof(arr) / sizeof(arr[0]);
int i = 0;
for(i = 0; i < sz; i++)
{
    printf("%d ", *(p + i));
}
```

## 二级指针

二级指针变量是用来存放一级指针变量的地址的

例子

```c
int main()
{
    int a = 10;
    int* pa = &a;	// pa 是一个指针变量，一级指针变量
    int** ppa = &pa;	// ppa是一个二级指针变量
    // *说明ppa是指针
    // int* 是说明ppa指向的对象是int* 类型
    return 0;
}
```

## 存放指针的数组

指针数组是存放指针的数组

```c
int main()
{
    int a = 10;
    int b = 20;
    int c = 30;
    // 指针数组
    int* parr[10] = {&a,&b.&c}
}
```

```c
int arr1[4] = {1,2,3,4};
int arr2[4] = {2,3,4,5};
int arr3[4] = {3,4,5,6};

int* parr[3] = {arr1, arr2, arr3};
int i = 0;
for(i = 0; i < 3; i++)
{
    int j = 0;
    for (j = 0; j < 4; j++)
    {
        printf("%d ", parr[i][j]);
    }
    printf("\n");
}
```

## const修饰指针变量

```c
int main()
{
	const int num = 10;
    const int* p = &num;
    int n = 100;
    // 1、const 放在*的左边
    // p指向的对象不能通过p来改变了，但是p变量本身的值是可以改变的
    // *p = 20; //err
    // p = &n; // ok
    
    // 2、const放在*右边
    // p指向的对象是可以通过p来改变的，但是不能修改p变量本身的值
    int* const p = &num;
    *p = 0;	// ok
    int n = 100;
    p = &n;	//err
    
    // const int* const p = &num;
    // 两种等价写法
    // const int* p = &num;
    // int const* p = &num;
    
    
    printf("%d\n",num);	
    
    return 0;
}
```

## 字符指针

```c
//char ch = 'w';
//char* pc = &ch;
//*pc = 'b';

char* p ="abcdef";	// 把字符串首字符a的地址，赋值给了p
printf("%s\n",p);
```

```c
const char* p1 = "abcdef";
const char* p2 = "abcdef";

char arr1[] = "abcdef";
char arr2[] = "abcdef";

if(p1 == p2)
    printf("p1==p2\n");		// √
else
    printf("p1!=p2\n");

if(arr1 == arr2)
    printf("arr1 == arr2\n");
else
    printf("arr1 != arr2\n"); // √
```

这里p1和p2指向的是同一个常量字符串。C/C++会把常量字符串存储到单独的一个内存区域，当几个指针。指向同一个字符串的时候，他们实际会指向同一块内存。但是用相同的常量字符串去初始化不同的数组的时候就会开辟出不同的内存块。所以p1和p2相同，arr1和arr2不同

## 指针数组

```c
int* arr1[10]; // 整形指针的数组
char *arr2[4];	// 一级字符指针的数组
char **arr3[5];	// 二级字符指针的数组
```

```c
int arr1[] = {1,2,3,4,5};
int arr2[] = {2,3,4,5,6};
int arr3[] = {3,4,5,6,7};

int* parr[3] = {arr1,arr2,arr3};	//parr存放数组的首元素地址

int i = 0;
for(i = 0; i < 3; i++)
{
    int j = 0;
    for(j = 0; j <  5; j++)
    {
        // printf("%d ", *(parr[i] + j));
        printf("%d ", parr[i][j]);
    }
    printf("\n");
}
```

```c
int *p1[10];	// p1是指针数组
int (*p2)[10];	// p2是数组指针，p先和*结合，说明p是一个大小为10个整型的数组，p2指向一个数组，该数组有10个元素，每个元素是int类型
// 注意：[]的优先级要高于*号的，所以必须加上()来保证p先和*结合
```

数组名通常表示的都是首元素的地址

但是有2个例外

1. sizeof(数组名)，这里的数组名表示整个数组，计算的是整个数组的大小
2. &数组名，这里的数组名表示的依然是整个数组，所以&数组名取出的是整个数组的地址（单位是字节）

```c
int arr[10] = { 0 };
printf("%p\n", arr);
printf("%p\n", arr+1);	// 4个字节

printf("%p\n", &arr[0]);
printf("%p\n", &arr[0]+1);	// 4个字节

printf("%p\n", &arr);
// &arr取出的是整个数组，加1则跳过这个数组
printf("%p\n", &arr+1);	// 40个字节

// 数组的类型决定加一的操作
// &arr -> int(*)[10]	// 加40
// arr -> int*	// 加1
```

## 数组指针

> 整型指针是用来存放整型的地址
>
> 字符指针是用来存放字符的地址
>
> 数组指针是用来存放数组的地址

```c
int arr[10] = { 0 };
int* p = arr;
int (*p2)[10] = &arr;	// 数组指针

char* arr[5] = { 0 };
char* (*pc)[5] = &arr;
```

```c
// int (*p)[5]
// p的类型是：int(*)[5];
// p是指向一个整型数组的，数组5个元素int[5]：p + 1 -> 跳过一个5个int元素的数组
void test(int (*p)[5], int r, int c)
{
    int i = 0;
    for(i = 0; i < r; i++)
    {
        // printf("%d ", *(*(p + i) + j));
        printf("%d ", p[i][j]);
    }
}
int main()
{
    int arr[3][5] = {1,2,3,4,5,23,4,5,6,3,4,5,6,7};
    test(arr,3,5);
    
    return 0;
}
```

## 数组参数、指针参数

一维数组传参

```c
#include <stdio.h>
void test(int arr[])
{}
void test(int arr[10])
{}
void test(int *arr)
{}
void test2(int *arr[20])	// 20也可以省略
{}
void test2(int **arr)
{}
int main()
{
    int arr[10] = {0};
    int *arr2[20] = {0};
    test(arr);
    test2(arr2);
}
```

二维数组传参

```c
void test(int arr[3][5])
{}
void test(int arr[][5])
{}
// 二维数组传参，函数形参的设计只能省略第一个[]的数字
// 因为对一个二维数组，可以不知道有多少行，但是必须知道一行多少元素
void test(int (*arr)[5])
{}
int main()
{
    int arr[3][5] = {0};
    test(arr);
}
```

一级指针传参

```c
#include <stdio.h>
void test(int *p, int sz)
{
    int i = 0;
    for(i=0;i<sz;i++)
    {
        printf("%d\n", *(p+i));
    }
}
int main()
{
    int arr[10] = {1,2,3,4,5,6,7,8,9};
    int *p = arr;
    int sz = sizeof(arr)/sizeof(arr[0]);
    // 一级指针p，传给函数
    test(p,sz);
    
    return 0;
}
```

二级指针传参

```c
#include <stdio.h>
// 函数的形式参数是二级指针
void test(int** ptr)
{
    printf("num = %d\n", **ptr);
}
int main()
{
    int n = 10;
    int*p = &n;
    int **pp = &p;
    
    test(pp);
    test(&p);
    
    int* arr[10];	// 指针数组
    test(arr);
    return 0;
}
```

## 函数指针

> &函数名 - 取出的就是函数的地址
>
> 对于函数来说，&函数名和函数名都是函数的地址

```c
int Add(int x, int y)
{
    return x + y;
}
int main()
{
    printf("%p\n", &Add);
    printf("%p\n",Add);
    // 函数指针
    int (*pf)(int, int) = &Add;
    // int (*pf)(int, int) = Add;
    
    // int ret = (*pf)(2,3);	// 加括号防止*对返回的5解引用
    int ret = pf(2,3);
    printf("%d\n", ret);	// 5
    return 0;
}
```

```c
int Add(int x, int y)
{
    return x + y;
}
void calc(int(*pf)(int, int))
{
    int a = 3;
    int b = 5;
    int ret = pf(a, b);
    printf("%d\n", ret);
}
int main()
{
    calc(Add);
    
    return 0;
}
```

## 函数指针数组

把函数的地址存到一个数组中，那这个数组就叫函数指针数组

```c
int (*parr1[10])();
int *parr2[10]();
int (*)() parr3[10];
```

```c
int a(int x, int y){}
int b(int x, int y){}
int c(int x, int y){}
int d(int x, int y){}

int main()
{
    int (*pf)(int, int) = a; // pf是函数指针
    
    int (*arr[4])(int, int) = {a,b,c,d};	// arr就是函数指针的数组
    
    return 0;
}
```

例子

```c
#include <stdio.h>
int Add(int x, int y)
{
	return x + y;
}
int Sub(int x, int y)
{
	return x - y;
}
int Mul(int x, int y)
{
	return x * y;
}
int Div(int x, int y)
{
	return x / y;
}

int main()
{
	int (*arr[])(int, int) = {Add, Sub, Mul, Div};
	int i = 0;
	for (i = 0; i < 4; i++)
	{
		printf("%d\n", arr[i](8, 4));
	}
	return 0;
}
```

## 指向函数指针数组的指针

指向函数指针数组的指针是一个**指针**

指针指向一个**数组**，数组的元素都是**函数指针**

定义

```c
int main()
{
    // 函数指针数组
    int (*pfArr[4])(int, int) = {Add, Sub, Mul, Div};
    
    // 指向【函数指针数组】的指针
    int (*(*ppfArr)[4])(int, int) = &pfArr;
    
    return 0;
}
```

## 回调函数

> 回调函数就是一个通过函数指针调用的函数。
>
> 如果把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数。
>
> 回调函数不是由该函数的实现方式直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应

使用qsort库函数进行冒泡排序

> 语法
>
> ```c
> void qsort(void* base,	// 要排序的数据的起始位置
> 		   size_t num,	// 带排序的数据元素的个数
>      size_t width, // 待排序的数据元素的大小（单位是字节）
>      int(*cmp)(const void* e1, const void* e2)	// 函数指针-比较函数
>     );
> ```
>
> 

```c
int cmp_int(const void* e1, const void* e2)
{
    return (*(int*)e1 - *(int*)e2);	// 升序
    // return (*(int*)e2 - *(int*)e1); // 降序
}

int main()
{
    int arr[] = {0,1,2,3,4,5,6,7,8,9};
    // 把数组排成升序
    int sz = sizeof(arr) / sizeof(arr[0]);
    qsort(arr, sz, sizeof(arr[0], cmp_int));
    int i = 0;
    for(i = 0; i < sz; i++)
    {
        printf("%d ", arr[i]);
    }
    return 0;
}
```

结构体排序

```c
#include <stdlib.h>
#include <stdio.h>
struct Stu
{
	char name[20];
	int age;
};
int cmp_stu_by_name(const void* e1, const void* e2)	// 根据名字排序
{
    // 比较原理：对应的字符一个个比较
    return strcmp(((struct Stu*)e1)->name,((struct Stu*)e2)->name);
}
int cmp_stu_by_age(const void* e1, const void* e2)	// 根据名字排序
{
    return strcmp(((struct Stu*)e1)->age,((struct Stu*)e2)->age);
}
void test()
{
    // 使用qsort来排序结构数据
    struct Stu s[] = { {"zhangsan", 15}, {"lisi",30}, {"wangwu",25} };
    int sz = sizeof(s) / sizeof(s[0]);
    // qsort(s, sz, sizeof(s[0]), cmp_stu_by_name);
    qsort(s, sz, sizeof(s[0]), cmp_stu_by_age);
}
int main()
{
    test();
    return 0;
}
```

# 结构体

> 结构是一些值的集合，这些值称为成员变量。结构的每个成员可以是不同类型的变量

自定义类型：结构体struct

```c
struct Stu
{
    char name[20];
    int age;
    char sex[5];
    char tele[15];
};

void print(struct Stu* ps)
{
    printf("%s %d %s %s\n", (*ps).name, (*ps).age, (*ps).sex, (*ps).tele);
    // -> 结构体指针变量->成员名
    printf("%s %d %s %s\n", ps->name, ps->age, ps->sex, ps->tele);
}


int main()
{
    // 结构体变量的创建
    struct Stu s = { "zhangsan", 20, "nan", "1234567890" };
    // 结构体对象.成员名
    printf("%s %d %s %s\n", s.name, s.age, s.sex, s.tele);
    print(&s);	// 地址传给定义的print函数

    return 0;
}
```

## 结构体的声明

```c
struct tag
{
    meber-list;
}variable-list;
```

```c
// 第一种形式
struct Peo
{
    char name[20];
    char tele[12];
    char gender[5];
};
// 第二种形式
struct Peo
{
    char name[20];
    char tele[12];
    char gender[5];
}p1, p2;	// p1和p2是使用struct Peo结构类型创建的2个变量
// p1和p2是两个全局的结构体变量
```

结构成员的类型：结构的成员可以是标量、数组、指针、甚至是其他结构体

```c
struct Peo
{
    char name[20];
    char tele[12];
    char gender[5];
};

struct St
{
	struct Peo p;
    int num;
    float f;
};

int main()
{
    struct Peo p1 = {"张三","123456789","男"};	// 结构体变量的窗口
    struct St s = { {"李四","987654321","男"}, 100, 3.14 }
}
```

结构体变量的定义和初始化

```c
struct Point
{
    int x;
    int y;
}p1;		// 声明类型的同时定义变量p1
struct Point p2,p3;	// 定义结构体变量p2、p3

// 初始化：定义变量的同时赋初值
struct Point p3 = {x, y};

struct Node
{
    int data;
    struct Point p;
    struct Node* next;
}n1 = {10, {4, 5}, NULL};	// 结构体嵌套初始化

struct Node n2 = {20, {5, 6}, NULL};	// 结构体嵌套初始化
```

## 特殊的声明

> 在声明结构的时候，可以不完全的声明

```c
// 匿名结构体类型
// 只能使用一次
struct
{
    int a;
    char b;
    float c;
}x;
struct
{
    int a;
    char b;
    float c;
}a[20], *p;
```

## 结构的自引用

```c
struct Node
{
    int data;
    struct Node* next;
};
```

## 结构体成员的访问

结构体变量访问成员

> 结构变量的成员是通过点操作符（.）访问的。点操作符接受两个操作数

```c
struct Stu
{
    char name[20];
    int age;
};
struct Stu s;
```

结构体指针访问指向变量的成员

有时候我们得到的不是一个结构体变量，而是指向一个结构体的指针

```c
struct Peo
{
    char name[20];
    char tele[12];
    char gender[5];
};

struct St
{
	struct Peo p;
    int num;
    float f;
};
void test1(struct Peo* sp)
{
    printf("%s %s %s %d\n", sp->name, sp->tele, sp->gender);	// 结构体指针->成员变量
};
void test2(struct Peo p)
{
    printf("%s %s %s %d\n", p.name, p.tele, p.gender);	// 结构体变量.成员变量
}
int main()
{
    struct Peo p1 = {"张三","123456789","男"};	// 结构体变量的窗口
    struct St s = { {"李四","987654321","男"}, 100, 3.14 };
    
    test1(p1);
    test2(&p1);
    
    return 0;
}
```

## 结构体内存对齐

> 计算结构体的大小

结构体的对齐规则：

> 1. 第一个成员在与结构体变量偏移量为0的地址处
>
> 2. 其他成员变量要对齐到某个数字（对齐数）的整数倍的地址处
>
>    **对齐数** =  编译器默认的一个对齐数 与 该成员大小的**较小值**
>
>    - `vs中默认的值为8`
>
> 3. 结构体总大小为最大对齐数（每个成员变量都有一个对齐数）的整数倍
>
> 4. 如果嵌套了结构体的情况，嵌套的结构体对齐到自己的最大对齐数的整数倍处，结构体的整体大小就是所有最大对齐数（含嵌套结构体的对齐数）的整数倍

```c
struct S1
{
    char c1;
    int i;
    char c2;
};
struct S2
{
    char c1;
    char c2;
    int i;
};
int main()
{
    struct S1 s1;
    struct S2 s2;
    
    printf("%d\n",sizeof(s1));	// 12
    printf("%d\n",sizeof(s1));	// 8
    
    printf("%d\n",offsetof(struct S1, c1));	//	0
    printf("%d\n",offsetof(struct S1, i));	//	4
    printf("%d\n",offsetof(struct S1, c2));	//	8
}
```

在设计结构体的时候，既要满足对齐，又要节省空间，最好让占有空间小的成员尽量集中在一起

## 修改默认对齐数

> #pragma预处理指令可以改变默认对齐数

```c
#pragma pack(4)	// 修改对齐数为 4
struct S
{
    int i; // 4 4 4 0~3
    double d;	// 8 4 4 4~11
};
#pragma pack()	// 恢复到默认对齐数

int main()
{
    printf("%d\n", sizeof(struct S));	// 12
    
    return 0;
}
```



## 结构体传参

```c
struct s
{
    int data[1000];
    int num;
};

struct S s = {{1,2,3,4}, 1000};
// 结构体传参
void print1(struct S s)
{
	printf("%d\n", s.num);
};
// 结构地址传参（推荐此写法）传递地址比较省内存
void print2(struct S* ps)
{
    printf("%d\n", ps->num);
};
int main
{
    print1(s);	// 传结构体（传值调用）
    print2(&s);	//传地址（传址调用）
    return 0;
}
```

> 函数传参的时候，参数是需要压栈的，会有时间和空间上的系统开销
>
> 如果传递一个结构体对象的时候，结构体过大，参数压栈的系统开销比较大，所以会导致性能的下降
>
> 结论：结构体传参的时候，要传结构体的地址

# 位段

> 结构体实现位段的能力

## 什么是位段

位段的声明和结构是类似的，有两个不同：

> 1. 位段的成员必须是`int、unsigned int`
> 2. 位段的成员名后边有一个冒号和一个数字

```c
struct A	// 位段类型
{
    // 成员是int类型，一次开辟4个字节
    // 先开辟4个byte-32bit
    int _a:2;	// 分配2个bit
    int _b:5;	// 分配5个bit
    int _c:10;	// 分配10个bit
    // 剩下15bit装不下30bit
    // 再开辟4个byte装下_d
    int _d:30;	// 分配30个bit
};
int main()
{
    printf("%d\n",sizeof(struct A));	// 8
    
    return 0;
}
```

## 位段的内存分配

> 1. 位段的成员可以是`int`，`unsigned int`，`signed int`或者是`char`（属于整型家族）类型
> 2. 位段的空间上是按照需要以4个字节（`int`）或者1个字节（`char`）的方式来开辟的
> 3. 位段涉及很多不确定因素，位段是不跨平台的，注重可移植的程序应该避免使用位段

```c
struct A	// 位段类型
{
    // 成员是int类型，一次开辟4个字节
    // 先开辟4个byte-32bit存放
    int _a:2;	// 分配2个bit
    int _b:5;	// 分配5个bit
    int _c:10;	// 分配10个bit
    // 剩下15bit装不下30bit
    // 再开辟4个byte装下_d
    int _d:30;	// 分配30个bit
};
```

## 位段的跨平台问题

> 1. int 位段被当成有符号数还是无符号数是不确定的
> 2. 位段中最大位的数目不能确定（16位机器最大16，32位机器最大32，写成27，在16位机器会出问题）
> 3. 位段中的成员在内存中从左向右分配，还是从右向左分配标准尚未定义
> 4. 当一个结构包含两个位段，第二个位段成员比较大，无法容纳于第一个位段剩余的位时，是舍弃剩余的位还是利用，这是不确定的
>
> 总结：跟结构相比，位段可以达到同样的效果，但是可以很好的节省空间，但是有跨平台的问题存在

# 表达式求值

表示式求值的顺序一部分是由操作符的优先级和结合性决定

同样，有些表达式的操作数在求值的过程中可能需要转换为其他类型

## 隐式类型转换

C的整型算术运算总是至少以缺省整型类型的精度来进行的

为了获得这个精度，表达式中的字符和短整型操作数在使用之前被转化为普通整型，这种转换称为**整型提升**

**整型提升的意义：**

> 表达式的整型运算要在CPU的相应运算器件内执行，CPU内整型运算器(ALU)的操作数的字节长度一般就是int的字节长度，同时也是CPU的通用寄存器的长度
>
> 因此，即使两个char类型的相加，在CPU执行时实际上也要先转换为CPU内整型操作数的标准长度
>
> 通用CPU（general-purpose CPU）是难以直接实现两个8比特字节直接相加运算（虽然机器指令中可能有这种字节相加指令）。所以，表达式中各种长度可能小于int长度的整型值，都必须先转换为 int 或 unsigned int ，然后才能送入CPU去执行运算

```c
char a = 1;
char b = 2;
char c = a + b;
// a 和 b的值被提升为普通整型，然后再执行加法运算
// 加法运算完成之后，结果将被截断，然后再存储于 c 中
```

如何进行整体提升呢

> 整型提升是按照变量的数据类型的符号位来提升的

```
// 负数的整型提升
char c1 = -1;
变量c1的二进制位（补码）中只有8个比特位：
11111111
因为 char 为有符号的 char
所以整形提升的时候，高位补充符号位，即为1
提升之后的结果是：
11111111111111111111111111111111

// 整数的整型提升
char c2 = 1;
变量c2的二进制位（补码）中只有8个比特位：
00000001
因为 char 为有符号的 char
所以整形提升的时候，高位补充符号位，即为0
提升之后的结果是：
00000000000000000000000000000001

// 无符号整形提升，高位补0
```

```c
char a = 5;
char b = 126;
char c = a + b;
printf("%d\n", c);	// -125
```

## 算术转换

如果某个操作符的各个操作属于不同的类型，那么除非其中一个操作数的转换为另一个操作数的类型，否则操作就无法进行。下面的层次体系称为**寻常算术转换**

```
long double
double
float
unsigned long int
long int
unsigned int
int
```

如果某个操作数的类型在上面这个列表中排位较低，那么首先要转换为另一个操作数的类型后执行运算

警告：

但是算术转换要合理，要不然会有一些潜在的问题

```c
float f = 3.14;
int num = f;	// 隐式转换，会有精度丢失
```

## 操作符的属性

复杂表达式的求值有三个影响的因素

1. 操作符的优先级
2. 操作符的结合性
3. 是否控制求值顺序

**两个相邻的操作符先执行哪个？取决于他们的优先级。如果两者的优先级相同，取决于它们的结合性**

```c
// 优先级相等，加法结合性L-R（左-右）
a + b + c
```

操作符优先级

| 操作符 | 描述                 | 用法示例                 | 结果类型   | 结合性 | 是否控制求值顺序 |
| :----: | -------------------- | ------------------------ | ---------- | :----: | :--------------: |
|  ( )   | 聚组                 | (表达式)                 | 与表达式同 |  N/A   |        否        |
|  ( )   | 函数调用             | rexp(rexp, ...,rexp)     | rexp       |  L-R   |        否        |
|  [ ]   | 下标引用             | rexp[rexp]               | lexp       |  L-R   |        否        |
|   .    | 访问结构成员         | lexp.member_name         | lexp       |  L-R   |        否        |
|   ->   | 访问结构指针成员     | rexp->member_name        | rexp       |  L-R   |        否        |
|   ++   | 后缀自增             | lexp ++                  | rexp       |  L-R   |        否        |
|   --   | 后缀自减             | lexp --                  | rexp       |  L-R   |        否        |
|   !    | 逻辑反               | ! rexp                   | rexp       |  R-L   |        否        |
|   ~    | 按位取反             | ~ rexp                   | rexp       |  R-L   |        否        |
|   +    | 单目，表示正值       | + rexp                   | rexp       |  R-L   |        否        |
|   -    | 单目，表示负值       | - rexp                   | rexp       |  R-L   |        否        |
|   ++   | 前缀自增             | ++ rexp                  | rexp       |  R-L   |        否        |
|   --   | 前缀自减             | -- lexp                  | rexp       |  R-L   |        否        |
|   *    | 间接访问             | * rexp                   | lexp       |  R-L   |        否        |
|   &    | 取地址               | & lexp                   | rexp       |  R-L   |        否        |
| sizeof | 取其长度，以字节表示 | sizeof rexp sizeof(类型) | rexp       |  R-L   |        否        |
| (类型) | 类型转换             | (类型) rexp              | rexp       |  R-L   |        否        |
|   *    | 乘法                 | rexp * rexp              | rexp       |  L-R   |        否        |
|   /    | 除法                 | rexp / rexp              | rexp       |  L-R   |        否        |
|   %    | 整数取余             | rexp % rexp              | rexp       |  L-R   |        否        |
|   +    | 加法                 | rexp + rexp              | rexp       |  L-R   |        否        |
|   -    | 减法                 | rexp - rexp              | rexp       |  L-R   |        否        |
|   <<   | 左移位               | rexp << rexp             | rexp       |  L-R   |        否        |
|   >>   | 右移位               | rexp >> rexp             | rexp       |  L-R   |        否        |
|   >    | 大于                 | rexp > rexp              | rexp       |  L-R   |        否        |
|   >=   | 大于等于             | rexp >= rexp             | rexp       |  L-R   |        否        |
|   <    | 小于                 | rexp < rexp              | rexp       |  L-R   |        否        |
|   <=   | 小于等于             | rexp <= rexp             | rexp       |  L-R   |        否        |
|   ==   | 等于                 | rexp == rexp             | rexp       |  L-R   |        否        |
|   !=   | 不等于               | rexp != rexp             | rexp       |  L-R   |        否        |
|   &    | 位与                 | rexp & rexp              | rexp       |  L-R   |        否        |
|   ^    | 位异或               | rexp ^ rexp              | rexp       |  L-R   |        否        |
|   \|   | 位或                 | rexp \| rexp             | rexp       |  L-R   |        否        |
|   &&   | 逻辑与               | rexp && rexp             | rexp       |  L-R   |        是        |
|  \|\|  | 逻辑或               | rexp \|\| rexp           | rexp       |  L-R   |        是        |
|  ? :   | 条件操作符           | rexp ? rexp : rexp       | rexp       |  N/A   |        是        |
|   =    | 赋值                 | lexp = rexp              | rexp       |  R-L   |        否        |
|   +=   | 以...加              | lexp += rexp             | rexp       |  R-L   |        否        |
|   -=   | 以...减              | lexp -=rexp              | rexp       |  R-L   |        否        |
|   *=   | 以...乘              | lexp *= rexp             | rexp       |  R-L   |        否        |
|   /=   | 以...除              | lexp /= rexp             | R-L        |  R-L   |        否        |
|   %=   | 以...取模            | lexp %= rexp             | rexp       |  R-L   |        否        |
|  <<=   | 以...左移            | lexp <<= rexp            | rexp       |  R-L   |        否        |
|  >>=   | 以...右移            | lexp >>= rexp            | rexp       |  R-L   |        否        |
|   &=   | 以...与              | lexp &= rexp             | rexp       |  R-L   |        否        |
|   ^=   | 以...异或            | lexp ^= rexp             | rexp       |  R-L   |        否        |
|  \|=   | 以...或              | lexp \|= rexp            | rexp       |  R-L   |        否        |
|   ,    | 逗号                 | rexp, rexp               | rexp       |  L-R   |        是        |

一些**问题表达式**

```c
// 表达式的求值部分由操作符的优先级决定
// 表达式1
a*b + c*d + e*f
```

> 注释：代码1在计算的时候，由于 * 比 + 的优先级高，只能保证 , * 的计算是比 + 早，但是优先级并不能决定第三个 * 比第一个 + 早执行

所以表达式的计算机顺序就可能是：

```c
//a*b + c*d + e*f
// 1  4  2  5  3

//a*b + c*d + e*f
// 1  3  2  5  4
```

# 枚举

## 枚举类型的定义和使用

{}中的内容是枚举类型的可能取值，也叫**枚举常量**

```c
enum gender
{
    MALE,
    FEMALE,
    SECRET
};
enum Color
{
    RED,
    GREEN,
    BLUE
};
int main()
{
    enum Color R = RED;
    
    return 0;
}
```

这些可能取值都是有值的，默认从0开始，一次递增1，当然在定义的时候也可以赋初值

```c
// 默认初始值
enum gender
{
    MALE,	// 0
    FEMALE,	// 1
    SECRET	// 2
};

// 赋初值
enum Color
{
  	RED=1,		// 1
    GREEN=2,	// 2
    BLUE,		// 3
    BLACK=5		// 5	
};
int main()
{
    printf("%d\n", RED);	// 1
    printf("%d\n", GREEN);	// 2
    printf("%d\n", BLUE);	// 3
}
```

## 枚举的优点

1. 增加代码的可读性和可维护性
2. 和#define定义的标识符比较枚举有类型检查，更加严谨
3. 防止了命名污染（封装）
4. 便于调试
5. 使用方便，一次可以定义多个常量

# 联合（共和体）

## 联合类型的定义

联合是一种特殊的自定义类型

这种类型定义的变量包含一系列的成员，**特征是这些成员公用同一块空间**

```c
// 联合类型的声明
union
{
    char a;
    int b;
}u;
union Un
{
    char c;
    int i;
};
int main()
{
    // 联合变量的定义
    union Un un;
    // 计算整个变量的大小
    printf("%d\n", sizeof(un)); // 4
}
```

## 联合的特点

> 联合的成员是共用同一块内存空间的，这样一个联合变量的大小，至少是最大成员的大小（因为联合至少得有能力保存最大的那个成员）

```c
union Un
{
    int i;
    char c;
};
int main()
{
    union Un un;
    printf("%d\n", &(un.i));
    printf("%d\n", &(un.c));
}
```

## 联合大小的计算

联合的大小至少是最大成员的大小

当最大成员大小不是最大对齐数的整数倍的时候，就要对齐到最大对齐数的整数倍

```c
union Un
{
    char arr[5];	// 5
    int i;	// 4
};

int main()
{
    printf("%d\n", sizeof(union Un));	// 8
    
    return 0;
}
```

# 预定义符号

```c
__FILE__	// 进行编译的源文件
__LINE__	// 文件当前的行号
__DATE__	// 文件被编译的日期
__TIME__	// 文件被编译的时间
__STDC__	// 如果编译器遵循ANSI C，其值为1，否则未定义
```

这些预定义符号都是语言内置的

```c
printf("file:%s line:%d\n", __FILE__, __LINE__);
```

# 动态内存管理

## malloc和free

> C语言提供了一个动态内存开辟的函数
>
> malloc和free都声明在`stdlib.h`头文件中

malloc函数

```c
void* malloc (size_t size);
```

这个函数向内存申请一块**连续可用**的空间，并返回指向这块空间的指针

- 如果开辟成功，则返回一个指向开辟好空间的指针
- 如果开辟失败，则返回一个NULL指针，因此malloc的返回值一定要做检查
- 返回值的类型是`void*`，所以malloc函数并不知情开辟空间的类型，具体在使用的时候使用者自己决定
- 如果参数`size`为0，malloc的行为是标准是未定义的，取决于编译器

```c
int main()
{
	int arr[10] = { 0 };
    // 动态内存开辟
    int* p = (int*)malloc(40);
    if(p == NULL)
    {
        printf("%s\n", strerror(errno));
        return 1;
    }
    // 使用
    int i = 0;
    for(i = 0; i < 10; i++)
    {
        *(p + i) = i;
    }
    for( i = 0; i < 10; i++)
    {
        printf("%d ", *(p + i));
    }
    free(p);
    p = NULL;
    
    return 0;
}
```

C语言提供了另外一个函数free，专门是用来做动态内存的释放和回收的，函数原型如下

```c
void free (void* ptr)
```

free函数用来释放动态开辟的内存

- 如果参数 ptr 指向的空间不是动态开辟的，那free函数的行为是未定义的
- 如果参数 ptr 是NULL指针，则函数什么事都不做

## calloc

`calloc`函数用来动态内存分配。原型如下

```c
void* calloc (size_t num, size_t size);
```

- 函数的功能是为`num`个大小为`size`的元素开辟一块空间，并且把空间的每个字节初始化为0
- 与函数`malloc`的区别只在于`calloc`会在返回地址之前把申请的空间的每个字节初始化为全0

```c
// 开辟10个整型的空间
int main()
{
    int*p = (int*)calloc(10, sizeof(int));
    if(p == NULL)
    {
        printf("%s\n", strerror(errno));
        return 1;
    }
    // 打印
    int i = 0;
    for(i = 0; i < 10; i++)
    {
        printf("%d ", *(p + i));	// 0 0 0 0 0 0 0 0 0 0
    }
    // 释放
    free(p);
    p = NULL;
    
    return 0;
}
```

## realloc

realloc函数对动态开辟内存大小的调整。函数原型：

```c
void* realloc (void* ptr, size_t size);
```

- `ptr`是要调整的内存地址
- `size`调整之后新大小
- 返回值为调整之后的内存起始位置
- 这个函数调整原内存大小的基础上，还会将原来内存中的数据移动到**新的空间**
- realloc在调整内存空间的是存在两种情况：
  - 情况1：原有空间有足够大的空间
  - 情况2：原有空间之后没有足够大的空间

```c
int main()
{
    int*p = (int*)malloc(40);
    if(NULL == p)
    {
        printf("%s\n", strerror(errno));
        return 1;
    }
    // 使用
    // 1 2 3 4 5 6 7 8 9 10
    int i = 0;
    for(i = 0; i < 10; i++)
    {
        *(p + i) = i + 1;
    }
    // 扩容
    int* ptr = (int*)realloc(p ,80);
    if(ptr != NULL)
    {
        p = ptr;
    }
    // 使用
    for(i = 0; i < 10; i++)
    {
        printf("%d ",*(p + i));
    }
    free(p);
    p = NULL;
    
    return 0;
}
```

# 柔性数组

> C99中，结构体中的最后一个元素允许是未知大小的数组，这就叫做【柔性数组】成员

```c
typedef struct st_type
{
    int i;
    int a[0];	// 柔性数组成员
}type_a;

// 有些编译器会报错无法编译可以改成
typedef struct st_type
{
    int i;
    int a[];	// 柔性数组成员
}type_a;
```

## 柔性数组的特点

- 结构中的柔性数组前面必须至少一个其他成员
- sizeof返回的这种结构大小不包括柔性数组的内存
- 包含柔性数组成员的结构用malloc()函数进行内存的动态分配，并且分配的内存应该大于结构的大小，以适应柔性数组的预期大小

```c
typedef struct st_type
{
    int i;
    int a[0];	// 柔性数组成员
}type_a;
printf("%d\n", sizeof(type_a));	// 4

// 有些编译器会报错无法编译可以改成
typedef struct st_type
{
    int i;
    int a[];	// 柔性数组成员
}type_a;
```

## 柔性数组的使用

```c
struct S
{
    int n;
    int arr[];	// 柔性数组成员
}
int main()
{
    struct S* ps = (struct S*)malloc(sizeof(struct S) + 40);
    if(ps == NULL)
    {
        return 1;
    }
    ps->n = 100;
    int i = 0;
    for(i = 0; i < 10; i++)
    {
        ps->arr[i] = i;
    }
    for(i = 0; i < 10; i++)
    {
        printf("%d ",ps->arr[i]);
    }
    struct S* ptr = (struct S*)realloc(ps, sizeof(struct S) + 80);
    if(ptr != NULL)
    {
        ps = ptr;
        ptr = NULL;
    }
    free(ps);
    
    return 0;
}
```

# 文件操作

## 文件指针

缓冲文件系统中，关键的概念是“**文件类型指针**”，简称“**文件指针**”

每个被使用的文件都在内存中开辟了一个相应的文件信息区，用来存放文件的相关信息（如文件的名字，文件状态及文件当前的位置等）。这些信息是保存在一个结构体变量中。该结构体类型是由系统声明的，取名**FILE**

每当打开一个文件的时候，系统会根据文件的情况自动创建一个FILE结构的变量，并填充其中的信息

创建一个FILE*的指针变量

```c
FILE* pf;	// 文件指针变量
```

定义pf是一个指向FILE类型数据的指针变量。可以使pf指向某个文件的文件信息区（是一个结构体变量）。通过该文件信息区中的信息就能访问该文件。也就是说，**通过文件指针变量能够找到与它关联的文件**

## 文件的打开和关闭

文件在读写之前应该先**打开文件**，在使用结束之后应该**关闭文件**

在编写程序的时候，在打开文件的同时，都会返回一个FILE*的指针变量指向该文件，也相当于建立指针和文件的关系

ANSIC规定使用f fopen 函数来打开文件，fclose 来关闭文件

```c
// 打开文件
FILE * fopen ( const char * filename, const char * mode);
// 关闭文件
int fclose( FILE * stream );
```

文件打开模式

| 模式 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| "r"  | 打开一个用于读取的文件。该文件必须存在                       |
| "w"  | 创建一个用于写入的空文件。如果文件名称与已存在的文件相同，则会删除已有文件的内容，文件被视为一个新的空文件 |
| "a"  | 追加到一个文件。写操作向文件末尾追加数据。如果文件不存在，则创建文件 |
| "r+" | 打开一个用于更新的文件，可读取也可写入。该文件必须存在       |
| "w+" | 创建一个用于读写的空文件                                     |
| "a+" | 打开一个用于读取和追加的文件                                 |

```c
#include <string.h>
#include <errno.h>
int main()
{
    FILE* pf = fopen("test.txt","r");
    if(pf == NULL)
    {
        printf("%s\n", strerror(errno));
        return 1;
    }
    // 读文件
    
    // 关闭文件
    fclose(pf);
    pf = NULL;
    
    return 0;
}
```

## 文件的顺序读写

| 功能           | 函数名  | 函数原型                                                     | 适用于     |
| -------------- | ------- | ------------------------------------------------------------ | ---------- |
| 字符输入函数   | fgetc   | int fgetc (FILE * stream);                                   | 所有输入流 |
| 字符输出函数   | fputc   | int fputc (int character, FILE * stream);                    | 所有输出流 |
| 文本行输入函数 | fgets   | char * fgets (char * str, int num,  FILE * stream);          | 所有输入流 |
| 文本行输出函数 | fputs   | int fputs (const char * str, FILE * stream);                 | 所有输出流 |
| 格式化输入函数 | fscanf  | int fscanf (FILE * stream, const char * format, ...)         | 所有输入流 |
| 格式化输出函数 | fprintf | int fprintf (FILE * stream, const char * fornat, ...)        | 所有输出流 |
| 二进制输入     | fread   | size_t fread (void * ptr, size_t size, size_t count, FILE * stream); | 文件       |
| 二进制输出     | fwrite  | size_t fwrite (const void * ptr, size_t size, size_t count, FILE * stream); | 文件       |

流

> 任何一个C程序，只要运行起来就会默认打开3个流：
>
> FILE * stdin - 标准输入流（键盘）
>
> FILE * stdout - 标准输出流（屏幕）
>
> FILE * stderr - 标准错误流（屏幕）

```c
fprintf(stdout,"%d\n",18);
```

## 文件的随机读写

### fseek

> 根据文件指针的位置和偏移量来定位文件指针

```c
int fseek( FILE * stream, long int offset, int origin);
```



### ftell

> 返回文件指针相对于起始位置的偏移量

```c
long int ftell (FILE * stream);
```



### rewind

> 让文件指针的位置回到文件的起始位置

```c
void rewind (FILE * stream);
```



## 文本文件和二进制文件

根据数据的组织形式，数据文件被称为**文本文件**或者**二进制文件**

数据在内存中以二进制的形式存储，如果不加转换的输出到外存，就是**二进制文件**

如果要求在外存上以ASCII码的形式存储，则需要在存储前转换。以ASCII字符的形式存储的文件就是**文本文件**

一个数据在内存中的存储

> 字符一律以ASCII形式存储，数值型数据既可以用ASCII形式存储，也可以使用二进制形式存储
>
> 如有整数10000，如果以ASCII码的形式输出到磁盘，则磁盘中占用5个字节（每个字符一个字节），而二进制形式输出，则在磁盘上只占4个字节（VS2013测试）

## 文件读取结束的判定

### 被错误使用的`feof`

在文件读取过程中，不能用 feof函数的返回值直接用来判断文件的是否结束

而是**应用于当文件读取结束的时候，判断是读取失败结束，还是遇到文件尾结束**

1. 文本文件读取是否结束，判断返回值是否为`EOF`（fgetc），或者`NULL`（fgets）

   例如：

   - `fgets`判断是否为`EOF`
   - `fgets`判断返回值是否为`NULL`

2. 二进制文件的读取结束判断，判断返回值是否小于实际要读的个数

   例如：

   - fread判断返回值是否小于实际要读的个数

## 文件缓冲区

ANSIC标准采用”**缓冲文件系统**“处理的数据文件的，所谓缓冲文件系统是指系统自动地在内存为程序中每一个正在使用的文件开辟一块”**文件缓冲区**“。从内存向磁盘输出数据会先送到内存中的缓冲区，装满缓冲区后才一起送到磁盘上。如果从磁盘向计算机读入数据，则从磁盘文件中读取数据输入到内存缓冲区（充满缓冲区），然后再从缓冲区逐个地将数据送到程序数据区（程序变量等）。缓冲区的大小根据C编译系统决定的



因为有缓冲区的存在，C语言再操作文件的时候，需要做刷新缓冲区或者在文件操作结束的结束的时候关闭文件

如果不做，可能导致读写文件的问题

# 对比一组函数

> scanf 是针对标准输入的格式化输入语句
>
> printf 是针对标准输出的格式化输出语句

> fscanf 是针对所有输入流的格式化输入语句
>
> fprintf 是针对所有输出流的格式化输出语句

> sscanf 从一个字符串中转化处一个格式化的数据
>
> sprintf 是把一个格式化的数据转化成字符串

# 条件编译

在编译一个程序的时候如果要将一条语句（一组语句）编译或者放弃是很方便的。因为我们有条件编译指令

常见的条件编译指令

```c
1、
    #if 常量表达式
    // ...
    #endif
    
    // 常量表达式由预处理求值
    如：
    #define __DEBUG__ 1
    #if __DEBUG__
    	// ...
    #endif
2、多个分支的条件编译
    #if 常量表达式
    	// ...
    #elif 常量表达式
    	// ...
    #else
    	// ...
    #endif
3、判断是否被定义
    #if defined(symbol)
    #ifdef symbol
    
    #if !defined(symbol)
    #ifndef symbol
4、嵌套指令
    #if defined(OS_UNIX)
    	#ifdef OPTION1
    		unix_version_option1();
		#endif
		#ifdef OPTION2
			unix_version_oprion2();
		#endif
	#elif defined(OS_MSDOS)
		#ifdef OPTION2
			msdos_version_option2();
		#endif
	#endif
```

# 文件包含

`#include`指令可以使另外一个文件被编译。就像它实际出现于`#include`指令的地方一样

这种替换的方式很简单：

> 预处理器先删除这条指令，并用包含文件的内容替换
>
> 如果这样一个源文件被包含10次，那就实际被编译10次

## 头文件被包含的方式

`<>`和`""`的区别是查找的策略不同

> `<>` 查找策略：直接去库目录下查找
>
> `""`查找策略：
>
> 1. 先去代码所在的路径下查找
> 2. 如果上面找不到，再去库目录下查找