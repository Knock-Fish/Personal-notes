# C++语句

## cout输出语句和endl控制符

`cout`语句：`cout`的对象属性包括一个插入运算符 `<<` 来输出

`cout`语句会将输出的字符串自动拼接

```c++
#include <iostream>
int main()
{
	using namesoace std;
	cout << "Hello";		
    cout << "World";	// HelloWorld
    
    int num = 10;
    cout << num << endl;	// 10
    
    // 拼接
    cout << "Hello," <<	"C++";		// Hello,C++
	
    cin.get();
	return 0;
}
```

> 程序结束后会自动关闭窗口，在 `return` 前添加 `cin.get()` 语句可以保持窗口运行

`endl` 控制符：在打印输入时插入 `endl`控制符 换行

## cin 输入语句

键盘输入的信息从 `cin` 流向运算符指向的变量（符号 `<<` 和 `>>` 选择指示信息流的方向）

```c++
#include <iostream>

int main()
{
    using namesoace std;
    int num;
    cin >> num;		// 接收输入的值保存到 num 变量
    cout << num << endl;
    return 0;

}
```

# 变量和数据类型

## 声明变量和赋值

```c++
int n;	//  声明变量
 n = 5;	// 赋值

int m = 1;	// 声明变量同时赋值

int a;
int b;
int c;
a = b = c = 2	// 连续赋值
```

## 数据类型

整型

| 整型      |
| --------- |
| int       |
| short     |
| char      |
| long      |
| long long |
| bool      |

无符号类型

| 类型               | 存储范围 |
| ------------------ | -------- |
| unsigned int       |          |
| unsigned short     |          |
| unsigned char      |          |
| unsigned long      |          |
| unsigned long long |          |



浮点类型

| 浮点类型    |
| ----------- |
| float       |
| double      |
| long double |

