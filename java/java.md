# java程序编写和执行的过程

java程序开发三步骤

1. 编写。将 Java代码编写写在.java结尾的源文件中
2. 编译。针对于.java结尾的源文件继续编译操作。格式：javac 源文件名.java
   - 编译以后，会生成1个或多个字节码文件。每一个字节码文件对应一个Java类，并且字节码文件名与类名相同
3. 运行。针对于编译后生成的字节码文件，进行解释运行。格式：java字节码文件名
   - 一个源文件中可以声明多个类。但是最多只能有一个类使用`public`进行声明且要求声明为`public`的类的类名与源文件名相同

```java
public class HelloJava{
	public static void main(String[] args){
		System.out.println("hello,world!!");
	}
}
class Hi{
    
}


其中：
    1、class：关键字，表示“类”，后面跟着类名
    2、main()方法的格式是固定的，表示程序的入口
       public static void main(String[] args)
    3、Java程序是严格区分大小写的
    4、从控制台输出数据的操作：
    	System.out.println()：输出数据之后，会换行
    	System.out.print()：输出数据之后，不会换行
    5、每一行执行语句必须以;结束
```



# 变量和数据类型

## 数据类型

基本数据类型

| 类型    | 说明         | 占用存储空间                                  |
| ------- | ------------ | --------------------------------------------- |
| byte    | 字节型       | <span style="color:red">1字节 = 8bit位</span> |
| short   | 短整型       | 2字节                                         |
| int     | 整型         | 4字节                                         |
| long    | 长整型       | 8字节                                         |
| float   | 单精度浮点型 | 4字节                                         |
| double  | 双精度浮点型 | 8字节                                         |
| char    | 字符型       | 2字节                                         |
| boolean | 布尔型       |                                               |

引用数据类型

| 类型       | 说明 |
| ---------- | ---- |
| class      | 类   |
| interface  | 接口 |
| [ ]        | 数组 |
| enum       | 枚举 |
| @interface | 注释 |
| record     | 记录 |

## 变量的声明和赋值

变量的数据类型可以是基本数据类型，也可以是引用数据类型

给变量赋值，就是把“值”存到该变量代表的内存空间中。同时，给变量赋的值类型必须与变量声明的类型一致或兼容

```java
public static void main(String[] args){
    /* 变量的声明 */
    int age;	 	// 存储一个整数类型的年龄
    double weight;	 // 存储一个小数类型的体重
    char gerner;	// 存储一个单字符类型的性别
    boolean marry;	// 存储一个布尔类型的婚姻状态
    String name;	// 存储一个字符串类型的姓名
    
    // 声明多个同类型的变量
    int a,b,c;	// 表示a,b,c三个变量都是int类型
    
    /*变量的赋值*/
    age = 18;	// 使用已经声明的变量赋值
    int num = 1;	// 变量声明的同时赋值
    
    /* 变量反复赋值 */
    int a = 1;
    a = 2; 	// 修改 a 变量的值
}
```

# 基本数据类型变量间运算规则

## 自动类型提升

将取值范围小（或容量小）的类型自动提升为取值范围大（或容量大）的类型

基本类型的转换规则

```java
char
byte	->	int	->	long	->	float	->	double
short
```



当把存储范围小的值（常量值、变量的值、表达式计算的结果值）赋值给了存储范围大的变量时

```java
int i = 'A';//char 自动升级为 int，其实就是把字符的编码值赋值给 i 变量了
double d = 10;//int 自动升级为 double
long num = 1234567; //右边的整数常量值如果在 int 范围呢，编译和运行都可以通过，这里涉及到数据类型转换

//byte bigB = 130;//错误，右边的整数常量值超过 byte 范围
long bigNum = 12345678912L;//右边的整数常量值如果超过 int 范围，必须加 L，显式表示 long 类型。否则编译不通过
```



当存储范围小的数据类型与存储范围大的数据类型变量一起混合运算时，会按照其中最大的类型运算

```java
int i = 1;
byte b = 1;
double d = 1.0;
double sum = i + b + d;//混合运算，升级为 double
```



当 byte，short，char 数据类型的变量进行算术运算时，按照 int 类型处理

```java
byte b1 = 1;
byte b2 = 2;
byte b3 = b1 + b2;//编译报错，b1 + b2 自动升级为 int
char c1 = '0';
char c2 = 'A';
int i = c1 + c2;//至少需要使用 int 类型来接收
System.out.println(c1 + c2);//113
```



## 强制类型转换

转换格式

```
数据类型 变量名 = (数据类型)被强转数据值;	// ()钟的数据类型必须 <= 变量值的数据类型
```

当把存储范围大的值（常量值、变量的值、表达式计算的结果值）强制转换为存储范围小的变量时，可能会损失精度溢出

```java
int i = (int)3.14;	// 损失精度

doble d = 1.2;
int num = (int)d;	// 损失精度

int i = 200;
byte b = (byte)i;	// 溢出
```

某个值想要提升数据类型时，也可以使用强制类型转换。这种情况的强制类型转换是*没有风险*的，通常省略

```java
int i = 1;
int j = 2;
double bigger = (double)(i/j);
```

声明 long 类型变量时，可以出现省略后缀的情况。float 则不同

```java
long l1 = 123L;
long l2 = 123;	// int类型的 123 自动提升为long 类型

// long l3 = 123123123123;	// 报错，因为 123123123123 超出了int的范围
long l4 = 123123123123L;


// float f1 = 12.3;	// 报错，因为 12.3 看做是double，不能自动转换为 float 类型
float f2 = 12.3F;
float f3 = (float)12.3;
```



## 基本数据类型与 String 的运算

任意基本数据类型的数据与 String 类型只能进行连接 "+" 运算，且结果一定也是 String 类型

```java
System.out.println("" + 1 + 2);		// 12

int num = 10;
boolean b = true;
String s1 = "abc";

String s2 = s1 + num + b;	// 注意顺序，int类型不能与boolean运算
System.out.println(s2);		// abc10true
```

String 类型不能通过强制类型 () 转换，转为其他的类型

```java
String str = "123";
int num = (int)str;		// 报错

int num = Ihteger.parseInt(str);	// 借助包装类的方法转换
```



# 运算符

## 算术运算符

| 运算符 | 运算                                                         |
| ------ | ------------------------------------------------------------ |
| +      | 正号，加法，字符串拼接                                       |
| -      | 符号，减法                                                   |
| *      | 乘法                                                         |
| /      | 除法                                                         |
| %      | 取模（取余）                                                 |
| ++     | 前置（后置）递增：<br>（前）：先自增，后取值<br>（后）：先取值，后自增 |
| --     | 前置（后置）递减：<br>（前）：先自减，后取值<br>（后）：先取值，后自减 |



## 赋值运算符

| 运算符 | 说明                                   |
| ------ | -------------------------------------- |
| +=     | 两边的值进行相加，结果赋值给左边的变量 |
| -=     | 两边的值进行相减，结果赋值给左边的变量 |
| *=     | 两边的值进行相乘，结果赋值给左边的变量 |
| /=     | 两边的值进行相除，结果赋值给左边的变量 |
| %=     | 两边的值进行取余，结果赋值给左边的变量 |



## 比较运算符

| 运算符     | 说明               |
| ---------- | ------------------ |
| ==         | 相等于             |
| !=         | 不等于             |
| <          | 小于               |
| >          | 大于               |
| <=         | 小于等于           |
| >=         | 大于等于           |
| instanceof | 检查是否是类的对象 |



## 三目运算符（条件运算符）

条件表达式为 true 时返回表达式1，为 false 时返回表达式2

```java
/* 条件表达式 ? 表达式1 : 表达式2 */

int a = 10;
int b = 20;
int n = a > b ? "yes" : "no";
System.out.println(n);	// no
```



## 逻辑运算符

| 运算符      | 说明                                                    |
| ----------- | ------------------------------------------------------- |
| & 和 &&     | 表示 “且” ，全为 true 则为 true，否则为 false           |
| \| 和  \|\| | 表示 “或”，有一个 true 则为 true，全为 false 则为 false |
| !           | 表示 “非”，取反，true 变 false，false 变 true           |
| ^           | 当两边布尔值不同时，结果为 true，相同时为 false         |

 `&` 和 `&&`的区别

- `&`：如果符号左边为 false ，则**继续**执行符号右边的操作
- `&&` ：如果符号左边为 false ，则**不再继续**执行符号右边的操作

`|` 和 `||` 的区别

- `|` ：如果符号左边为 true，则**继续**执行符号右边的操作
- `||` ：如果符号左边为 true ，则**不再继续**执行符号右边的操作

```java

/* | */
int x1 = 1, y1 = 1;
System.out.println(++x1 == 2 | ++y1 == 2);  // true
System.out.println(x1);  // 2
System.out.println(y1);  // 2

/* || */
int x2 = 1, y2 = 1;
System.out.println(++x2 == 2 || ++y2 == 2);	//true
System.out.println(x2);  // 2
System.out.println(y2);  // 1

/* & */
int a1 = 1, b1 = 1;
System.out.println(a1 == 2 & ++b1 == 2);	// false
System.out.println(a1);	// 1
System.out.println(b1);	// 2

/* && */
int a2 = 1, b2 = 1;
System.out.println(a2 == 2 && ++b2 == 2);	// false
System.out.println(a2);	// 1
System.out.println(b2);	// 1

/* ! */
System.out.println(!true);	// false

/* ^ */
int n1 = 10, n2 = 10, n3 = 20;
System.out.println((n1 == n2) ^ (n1 > n3));	// true
System.out.println((n1 == n2) ^ (n1 < n3));	// false
System.out.println((n1 > n2) ^ (n1 > n3));	// false
```



## 位运算符

位运算符的运算过程基于二进制的补码运算

| 运算符 | 作用       | 说明                                                         |
| ------ | ---------- | ------------------------------------------------------------ |
| <<     | 左移       | 左边被移除的高位丢弃，右边空缺位补 0<br/>在一定范围内，数据每向左移动一位，相当于原数据 `*2`（正负数都适用）<br/>如 `3<<4`，那么 3\*2 的 4次幂 ，3 \* 16 = 48 |
| >>     | 右移       | 正数右移后，空缺位补0。负数右移后，空缺位补1<br/>在一定范围内，数据每右移动一位，相当于原数据 `/ 2`（正负数都适用）<br/><span style="color:red">如果不能整除，向下取整</span><br/>如 `69>>4`，那么 69 / 2 的 4次，69 / 16 = 4 |
| >>>    | 无符号右移 | 右移后，左边被移位二进制最高无论是 0 或者是 1，空缺位都补 0（正负数都适用） |
| &      | 与         | 对应位只有 `1 & 1` 时结果是 1， 否则是 0<br/>如 `0000 0001 & 0000 0011`，结果为· `0000 0001` |
| \|     | 或         | 对应位只有 `0 | 0` 时结果时 0， 否则是 1<br/>如 `0000 0001 | 0000 0011`，结果为 `0000 0011` |
| ^      | 异或       | 对应位只有`1 ^ 0 ` 时结果为 1，否则为 0<br/>如 `0000 0001 ^ 0000 0011`，结果为 `0000 0010` |
| ~      | 取反       | 对应位为 1，则结果为 0；对应位为 0，则结果为 1。<br/>正数取反，二进制码按补码各位取反<br/>负数取反，二进制码按补码各位取反 |



## 运算符优先级

上一行中的运算符总是优先于下一行

| 说明             | 运算符                     |
| ---------------- | -------------------------- |
| 括号             | `()、[]、{}`               |
| 正负号           | `+、-`                     |
| 单元运算符       | `++、--、~、!`             |
| 乘法、除法、求余 | `*、/、%`                  |
| 加法、减法       | `+、-`                     |
| 移位运算符       | `<<、>>、>>>`              |
| 关系运算符       | `<、<=、>=、>、instanceof` |
| 等价运算符       | `==、!=`                   |
| 按位与           | `&`                        |
| 按位异或         | `^`                        |
| 按位或           | `/`                        |
| 条件与           | `&&`                       |
| 条件或           | `//`                       |
| 三元运算符       | `? :`                      |
| 赋值运算符       | `=、+=、-=、*=、/=、%=`    |
| 位赋值运算符     | `&=、/=、<<=、>>=、>>>=`   |



# 流程控制语句

## 分支语句

### if-else 条件判断

**单分支条件判断**：条件表达式为 true 就执行语句块，如果为 false 则不执行

```java
if(条件表达式){
    语句块;
}
```



**双分支条件判断**：条件表达式为 true 就执行语句块1，如果为 false 则执行语句块2

```java
if(条件表达式){
    语句块1;
}else{
    语句块2;
}
```



**多分支条件判断**：从上往下依次判断，一旦条件表达式为 true，则执行相应的语句块，后面的条件表达式则不再进行判断，执行完对应的语句块，就跳出当前结构。如果前面条件表达式都为 false，则执行 else 语句

```java
if (条件表达式1) {
    语句块1;
}else if (条件表达式2) {
    语句块2;
}else{
    语句块n;
}
```



### switch-case 选择结构

1. 根据 `switch` 中表达式的值，依次匹配各个 `case` 。如果表达式的值等于某个 `case` 中的常量值，则执行对应 `case` 中的语句块
2. `switch`  中表达式的值的类型必须是其中之一：`byte`，`short`，`char`，`int`，枚举，`String`
3. `break` 语句用于执行完一个 `case` 后使程序跳出 `switch` 语句，如果没有 `break` ，程序会顺序执行到 `switch` 结尾（`case` 穿透性）
4. `default` 语句是可选的。当没有匹配的 `case` 时，执行 `default` 语句

```java
switch(表达式){
    case 常量值1:	// case子句中的值必须是常量，不能是变量或不确定表达式值或范围
        语句块1;
        //break;
    case 常量值2:
        语句块2;
        //break;
    case 常量值3:
        语句块3;
        //break;
    default:		// 如case在有break情况下，如果前面一个都没选，则执行默认语句
        语句块n;
        break;
}
```



## 循环语句

循环语句具有在某些条件满足的情况下，反复执行特定代码的功能

### for 循环

`for` 语句中必须有初始化，循环条件，结束条件

1. 初始化可以声明多个变量，但必须是同一类型，用逗号分隔
2. 循环条件为 `boolean` 类型表达式，当值为 false 时，退出循环
3. 可以有多个变量更新，用逗号分隔

```java
for(初始化; 循环条件; 迭代){
    循环体;
}
```

例如

```java
// 格式是多样的


for(int i = 0; i < 5; i++){
    System.out.println(i);	// 0 1 2 3 4
}


int n = 0;
for(; n < 5; n++){
    System.out.println(n);	// 0 1 2 3 4
}


int n = 1;
for(System.out.println("a");n < 5; System.out.println("b"), n++){
    System.out.println(n);	// a 1 b 2 b 3 b 4 b
}
```



### while 循环

`while`  循环条件必须是 `boolean` 类型，结构中必须要有结束条件，否则变成死循环

```java
while(循环条件){
    循环体;
    迭代;
}
```

例如

```java
int i = 1;
while(i <= 5){
    System.out.println(i);
    i++;
}
```



### do-while 循环

`do-while` 循环中至少会先执行一次循环体，后进入循环

```java
do{
    循环体;
    迭代;
}while(循环条件);
```



## 关键字 break 和 continue

### break

`break` 语句用于结束某个语句块的执行

在多层嵌套的语句块中时，`break` 可以通过标签指明要终止的语句（标签名任意）

```java
// break 跳出结构
for(int n = 0; n < 10; n++){
    if (n == 6){	// 当 n = 6 时跳出循环
        System.out.println(n);
        break;
    }
}

// 通过标签结束指定语句
int sum = 0;
label1:
for (int i = 0; i < 5; i++) {
    label2:
    for (int j = 0; j < 5; j++) {
        label3:{
            sum++;
            if (j == 2) {
                System.out.println("j="+j);
                break label1;    // j = 2时，结束label1标签的语句块
            }
        }
    }
}
System.out.println("内层for循环了"+sum+"次");    
```



### continue

`continue` 语句用于结束当次语句块的执行

在多层嵌套的语句块中时，`continue` 可以通过标签指明当次要跳过的语句（标签名任意）

```java
// continue 跳出当次循环
for(int n = 0; n < 5; n++){
    if(n == 2){
        continue;	// n = 2 时跳过当前循环
    }
    System.out.println(n);	// 0 1 3 4
}


// 通过标签跳出指定语句
int sum = 0;
label1:
for(int i = 0; i < 5; i++){
    label2:
    for(int j = 0; j < 5; j++){
        label3:{
            sum++;
            if (j == 2) {
                System.out.println("j=" + j);
                continue label1;	// 每j = 2时，跳过当前label1标签执行的语句块
            }
        }
    }
}
System.out.println("内层for循环了"+sum+"次");
```



# Scanner 键盘输入

`Scanner ` 类可以从键盘获取不同类型（基本数据类型、`String` 类型）的变量

四个步骤：

1. 导包：`import java.util.Scanner`

2. 创建 `Scanner`  类型的对象

   ```java
   Scanner 变量名 = new Scanner(System.in);
   ```

3. 在读取前一般需要 使用 `hasNext` 与 `hasNextLine` 判断是否还有输入的数据

   ```java
   import java.util.Scanner;
   
   public class PrintTest {
       public static void main(String[] args) {
           Scanner scan = new Scanner(System.in);
           // 判断是否还有输入
           if(scan.hasNext()){
               String str = scan.next();
               System.out.println(str);
           }
           // 释放资源
           scan.close();
       }
   }
   ```

4. 调用 `Scanner` 类的方法，来获取指定类型的变量

   | 方法            | 说明                                                         |
   | --------------- | ------------------------------------------------------------ |
   | `next()`        | 一定要读取到有效字符后才可以结束输入，在读取的字符中<span style="color:red">以空白为结束符</span> |
   | `nextLine() `   | 以 Enter 为结束符，返回输入回车之前的所有字符（包括空白）    |
   | `nextInt()`     | 返回 `int` 类型的值                                          |
   | `nextDouble()`  | 返回 `double` 类型的值                                       |
   | `nextByte()`    | 返回 `byte` 类型的值                                         |
   | `nextShort()`   | 返回 `short` 类型的值                                        |
   | `nextBoolean()` | 返回 `boolean` 类型的值                                      |
   | `nextFloat()`   | 返回 `float` 类型的值                                        |
   | `nextLong()`    | 返回 `long` 类型的值                                         |



# 数组

## 一维数组

### 数组声明

Java语言中声明数组时不能指定其长度（数组中的元素个数）

```java
// 声明一维数组
// 推荐写法
元素的数据类型[] 数组名;

// 第二种写法
元素的数据类型 数组名[];
```

```java
int [] arr1;
int arr2[];
```



### 数组初始化

**静态初始化**：数组变量的初始化和数组元素的赋值操作同时进行

```java
// 格式1
数据类型[] 数组名 = new 数据类型[]{元素1, 元素2, 元素3, ...};	// 初始化同时赋值

// 格式2
数据类型[] 数组名;		// 先声明
数组名 = new 数据类型[]{元素1, 元素2, 元素3, ...};	// 后赋值
```

> 数组本身是引用数据类型，要用 `new` 创建数组

例如

```java
int[] arr1 = {1, 2, 3, 4, 5};
int[] arr2 = new int[]{1, 2, 3, 4, 5};
```



**动态初始化**：数组变量的初始化和数组元素的赋值操作分开进行

```java
// 格式1
数据类型[] 数组名 = new 数据类型[长度];

// 格式2
数据类型[] 数组名;
数组名 = new 数据类型[长度];
```

> 数组有定长特性，长度一旦指定，不可更改

例如

```java
// 格式1
int arr1 = new int[5];

// 格式2
int arr2;
arr2 = new int[5];
```



### 数组的引用

可以通过数组的索引（下标）访问到数组中的元素

数组的下标范围：数组下标从 `[0]` 开始，下标范围是 `[0, 数组名.lenth-1]`

数组元素下标可以是整型常量或整型表达式

```java
// 格式
数组名[索引];

int arr = {1, 2, 3, 4, 5};
System.out.println("arr数组的3个元素：" + arr[2]);		// 3

// 修改第2个元素的值
arr[1] = 20;
System.out.println("arr数组的3个元素：" + arr[1]);		// 20
```



### 数组元素的默认值

数组的引用类型，当数组未赋值时会自动分配默认值

| 数组元素类型 | 元素默认初始值                 |
| :----------- | ------------------------------ |
| byte         | 0                              |
| short        | 0                              |
| int          | 0                              |
| long         | 0L                             |
| float        | 0.0F                           |
| double       | 0.0                            |
| char         | 0 或写为 '\u0000' （表现为空） |
| boolean      | false                          |
| 引用类型     | null                           |



## 二维数组

### 声明和初始化

二维数组声明的语法格式

```java
// 推荐写法
数据类型[][] 数组名;

// 第二种
数据类型 数组名[][];

// 第三种
数据类型[] 数组名[]
```



#### 静态初始化

定义一个名称为 arr 的二维数组，二维数组中有三个一维数组

```java
// 格式
int[][] arr = new int{
    {1, 2, 3},
    {4, 5},
    {6, 7, 8, 9};
}
```

注意特殊写法

```java
int[] x, y[];	// x 是一维数组，y是二维数组
```



#### 动态初始化

规则二维表：每一行的列数是相同的

```java
// 第一：确定行数和列数
数据类型[][] 数组名 = new 数据类型[m][n];	// 此时创建完数组，行数、列数确定，而且元素也都有默认值
// m：表示这个二维数组有多少个一维数组
// n：表示每一个一维数组有多少元素

// 第二：为元素赋值
数组名[行小标][列下标] = 值;
```



不规则：每一行的列数不一样

```java
// 第一：确定总行数
数据类型[][] 数组名 = new 数据类型[总行数][];	// 此时确定了总行数，每一行里面现在是 null

// 第二：再确定每一行的列数，创建每一行的一维数组
数组名[行下标] = new 数据类型[该行的总列数];	// 此时已经new完的元素就有默认值了（没有new的行还是 null）

// 第三：为元素赋值
数组名[行下标][列下标] = 值;
```



### 数组的长度和角标

二维数组的长度/行数

```java
二维数组名.length
```

获取二维数组的某一行

```java
二维数组名[行下标]
```

获取某一行的列数

```java
二维数组名[行下标].length
```

获取某一个元素

```java
二维数组名[行下标][列下标]		// 先确定行，再列
```



# 面向对象

## 类的定义

使用`class`关键字定义类

```java
[修饰符] class 类名{
    属性声明;
    方法声明;
}
```

例如

```java
public class Persons{
    // 声明属性name
    String name;
    
    // 声明方法
    public void say(){
        System.out.println("你好");
    }
}
```



## 对象的创建和调用

使用 `new`关键字创建对象，对象是类的一个实例，具备该类的属性和方法

```java
// 实例化对象
// 方式1：给创建的对象命名
类名 对象名 = new 类名();

// 方式2：匿名
new 类名();


// 访问对象成员
对象名.属性		// 访问属性
对象名.方法		// 访问方法
```

例如

```java
public class Persons{
    String name;
        int age;
    public void say(){
        System.out.println("你好");
    }
}
class Person{
    public static void main(String[] args){
        // 创建 Person 类的对象
        Persons p1 = new Persons();
        // 访问属性
        p1.age = 18;
        p1.name = "张三";
        // 访问方法
        p1.say();
    }
}
```



## 匿名对象（anonymous object）

匿名对象：可以不定义对象的句柄，直接调用这个对象的方法

- 如果一个对象只需要进行一次方法调用，那么就可以使用匿名对象
- 常用将匿名对象作为实参传递给一个方法调用

```java
public class Persons{
    String name;
        int age;
    public void say(){
        System.out.println("你好");
    }
}
class Person{
    public static void main(String[] args){
       new Persons().say();		// 匿名对象
    }
}
```



## 声明成员变量

```java
[修饰符1] class 类名{
    [修饰符2] 数据类型 成员变量名 [=初始化值];
}
```



## 声明方法

```java
[修饰符] 返回值类型 方法名 ([形参列表])[throws 异常列表]{
    方法体的功能代码
}
```



# return 关键字

`return` 在方法的作用：

1.  结束一个方法
2. 结束一个方法的同时，返回数据给方法的调用者



# 方法的重载（overload）

在同一个类中，允许存在一个以上的同名方法，只要它们的参数列表不同即可（参数个数或参数类型的不同）

**重载的特点**：与修饰符、返回值类型无关，只看参数列表，且参数列表必须不同。调用时，根据方法参数列表的不同区别

**重载方法调用**：JVM通过方法的参数列表，调用匹配的方法

- 先找个数，类型最匹配的
- 再找个数和类型可以兼容的，如果同时多个方法可以兼容将会报错

```java
class Add {
    // 返回两个整数的和
    public static int add(int x, int y) {
        return x + y;
    }

    // 返回三个整数的和
    public static int add(int x, int y, int z) {
        return x + y + z;
    }

    public static void main(String[] args) {
        System.out.println(add(1, 2, 3));
    }
}
```



# 可变个数的形参

可变参数：方法参数部分指定类型的参数个数是可变多个

可变个数形参的方法与同名的方法之间，彼此构成重载

可变参数方法的使用与方法参数部分使用数组是一致的，二者不能同时声明，否则报错

可变形参需要放在形参声明的最后

一个方法的形参最多只能声明一个可变个数的形参

```java
方法名(参数的类型名 ...参数名)
```

例如

```java
class Add {
    // 求 n 个整数的和
    public static int add(int ...num){
        int sum = 0;
        for(int i = 0; i < num.length; i++){
            sum += num[i];
        }
        return sum;
    }
    public static void main(String[] args) {
        System.out.println(add(1,2,3));
    }
}
```



# 方法的参数传递机制

## 形参和实参

**形参（formal parameter）**：在定义方法时，方法名后面括号 `()` 中声明的变量称为形式参数，简称形参

**实参（actual parameter）**：在调用方法时，方法名后面括号 `()` 中的使用的值 / 变量 / 表达式称为实际参数，简称实参

## 参数传递机制：值传递

Java里方法的参数传递方式只有一种：值传递。即将实际参数值的副本（复制品）传入方法内，而参数本身不受影响

- 形参是基本数据类型：将实参基本数据类型变量的 “数据值” 传递给形参
- 形参是引用数据类型：将实参引用数据类型变量的 “地址值” 传递给形参



# package、import 关键字

## package

package（包）：用于指明该文件定义的类、接口等结构所在的包

- 一个源文件只能有一个声明包的 `package` 语句
- 包声明应该在源文件的第一行，每个源文件只能有一个包声明，这个文件中的每个类型都应用于它
- 如果一个源文件中没有使用包声明，那么其中的类，函数，枚举，注释等将被放在一个无名的包（unnamed package）中

```java
package 顶层包名.子包名;
```



例如：pack1\pack2\PackageTest.java

```java
package pack1.pack2;	// 指定类PackageTest 属于包 pack1.pack2
public class PackageTest{
    public void display(){
        System.out.println("in method display()");
    }
}
```



包的作用：

- 把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用
- 解决类命名冲突
- 包限定了访问权限，拥有包访问权限的类才能访问某个包中的类



## import

使用 `import` 语句导入定义在其它包中的 Java 类

- 在 java 源文件中 `import` 语句必须位于 Java 源文件的头部
-  用于引入其他包中的类、接口或静态成员，它允许你在代码中直接使用其他包中的类，而不需要完整地指定类的包名

```java
import 包名.类名;
import 包名.*;	// 引入包中的所有结构
```



例如：

PackageTest.java

```java
package pack;
public class PackageTest{
    public void display(){
        System.out.println("in method display()");
    }
}
```

PrintTest.java

```java
import pack.PackageTest;	// 导入包中的PackageTest类

public class PrintTest {
    public static void main(String[] args) {
        PackageTest t = new PackageTest();
        t.display();
    }
}
```



# 封装性

实现数据封装：控制类或成员的可见性范围，需要依赖访问控制修饰符，也称为权限修饰符来控制

权限修饰符及具体访问范围：

| 修饰符      | 本类内部 | 本包内 | 其他包的子类 | 其他包非子类 |
| ----------- | -------- | ------ | ------------ | ------------ |
| `private`   | √        | x      | x            | x            |
| 缺省        | √        | √      | x            | x            |
| `protected` | √        | √      | √            | x            |
| `public`    | √        | √      | √            | √            |

具体修饰的结构

- 外部类：`public`、缺省
- 成员变量、成员方法、构造器、成员内部类：`public`、`protected`、缺省、`private`



## 成员变量 / 属性私有化

私有化类的成员变量，提供公共的 `get` 和 `set` 方法，对外暴露获取和修改属性的功能

使用 `private` 修饰成员变量

```java
public class Person{
    // 一、private 数据类型 变量名;
    private String name;
    private int age;
    private boolean marry;
    
    // 二、提供 getXxx 方法 / setXxx 方法，可以访问成员变量
    public void setName(String n){
        name = n;
    }
    
    public String getName(){
        return name;
    }
    
    public void setAge(int a){
        age = a;
    }
    
    public int getAge(){
        return age;
    }
    
    public void setMarry(boolean m){
        marry = m;
    }
    
    public boolean isMarry(){
        return marry;
    }
    
}
```

实例变量私有化，跨类无法直接使用

```java
public class PersonTest {
    public static void main(String[] args) {
        Person p = new Person();
        // 实例变量私有化，跨类无法直接使用
        /* p.name = "张三";
        p.age = 18;
        p.marry = true; */
        p.setName("张三");
        System.out.println(p.getName());

        p.setAge(18);
        System.out.println(p.getAge());

        p.setMarry(true);
        System.out.println(p.isMarry());
    }
}
```



## 方法私有化



# 构造器（Constructor）

在 `new` 对象的时候为实例变量赋值

- 构造器名必须与它所在的类名相同
- 没有返回值，不需要返回值类型，也不需要 `void`
- 构造器的修饰符只能时权限修饰符，不能被其他任何修饰
- 不能有 `return` 语句返回值
- 如没有显式的声明类中的构造器时，会默认提供一个无参构造器并且修饰符默认与类的修饰符相同（在类中至少存在一个构造器）
- 构造器是可以重载的

```java
// 构造器的语法格式
[修饰符] class 类名{
    [修饰符] 构造器(){
        // 实例初始化代码
    }
    [修饰符] 构造器(参数列表){
        // 实例初始化代码
    }
}
```

例如

Person.java

```java
public class Person {
    private String name;
    private int age;
    // 无参构造
    public Person(){}
    // 有参构造
    public Person(String n, int a){
        name = n;
        age = a;
    }
    public String getInfo(){
        return "姓名：" + name + "，年龄：" + age;
    }
}

```

PersonTest.java

```java
public class PersonTest {
    public static void main(String[] args) {
        // 调用无参构造器创建对象
        Person p1 = new Person();

        // 调用有参构造创建对象
        Person p2 = new Person("张三", 18);

        System.out.println(p1.getInfo());
        System.out.println(p2.getInfo());
    }
}

```



