---
title: "C笔记"
date: 2019-05-22T10:47:56+08:00
categories:
- 笔记
tags:
- c
draft: true
---

## 字符数组与字符串

## 字节对齐

### 结构体字节对齐
```c
struct employee {
	int number;
	double salary;
	char name[20];
} emp;
```
首先，我们知道基本数据类型的大小：

+ `char` => `1 Byte`
+ `int` => `4 Byte`
+ `double` => `8 Byte`

`employee` 结构体内共有 3 个成员变量。下面开始模拟字节对齐过程：

1. 读取到 `int` ，当前最大空间为 `4 Byte` 。
2. 读取到 `double` ，更新当前最大空间为 `8 Byte` ，需要对前面的 `int` 进行字节填充，填充大小为 `8 Byte - 4 Byte = 4 Byte` ，此时结构体大小为 `8 Byte + 8 Byte = 16 Byte` 。
3. 读取到 `char` 类型，且此时为最后一个类型，先不用立刻进行字节对齐。 `char` 数组大小为 `20 Byte` ，首先计算总大小 `16 Byte + 20 Byte = 36 Byte` ，此前最大空间为 `8 Byte` ，此时 `36 Byte % 8 Byte = 0` ，满足要求，结束空间分配。倘若 `char` 数组大小为 `7` ，则总大小 `16 Byte + 7 Byte = 23 Byte` ， 由于 `23 Byte % 8 Byte != 0` ，此时需要进行字节填充，取 `8 Byte` 的最小倍数且大于当前总大小（`23 Byte`）， 即结构体的最终大小为 `24 Byte` 。 

Q：为什么要进行字节填充？

> 我还不甚了解，等有机会好好看一下。

### 共用体字节对齐

```c
union U {
	int a;
	double b;
	char c[10];
}
```
共用体内所有的变量都使用相同的一段地址空间，它的空间大小由最长的变量（此处为 `char c[10]`）决定。也就是说，`union U` 的大小为 `10 Byte` 。

## 科学记数法

```c
int i = 3E2;
long l = 3E5L;
float f = 3.14E5F;
float f2 = 3.14E-2F;
```

## 指针
### 指针数组与数组指针

### 指针常量与常量指针

常量指针，比如：`const int * a`
`int * const a` 的区别

`const int` 定义一个常量型 `int` ，即 `int` 内存放的值不可变。而 `* a` 则表示该变量是一个指针。所以，合并起来就是：`a` 是指向**常量型** `int` 的指针，即 `a` 指向地址内的存储的值不可变。

`int *` 定义了一个 `int` 型的指针，`const a` 表示 `a` 是不可变的常量。所以，合并起来就是：`a` 是指向**变量型** `int` 的指针，即 `a` 指向地址内存储的值可变，但是 `a` 本身不可变，即 `a` 所指向的地址不可变。

### 优先级

#### 与自增运算符比较

`right ++ > * > left ++`

```c
int a = 1;
int *p = &a;
++*p;    // *(++p)
*p++;    // (*p)++
```

#### 与算术运算符比较

`* > +`

```c
int a = 3;
int * ptr = &a;
*a+1;    // (*a)+1 => 4
```

### 二维数组指针引用

```c
int a[2][2];
*(a)+1;    // &a[0][1]
*(*(a)+1);    // a[0][1]
*(a+1)+1;    // &a[1][1]
*(*(a+1)+1);    // a[1][1]
```
### 示例

```c
char * str = "abcdefg";
printf("%d\n", &str);
int * ptr = (int *) str;
printf("%d\n", ptr);
printf("%c\n", *(ptr + 1));


int a[2] = {1,2};
printf("%d", &a);    // -9880
printf("%d", &a+1);    // -9872

/*
 * a 指向 int 数组，长度为 2，大小为 4 Byte
 * &a+1 向上移动一个变量的大小，即 4 Byte
 */

int *ptr = (int *) (&a + 1);    // 指向最后一个元素的后一个元素
printf("%d", *(ptr - 1));    // 指向最后一个元素，即 2

int *(*p(int))[3]
```

## sizeof

### 介绍

老师教学的时候，只会告诉你 `sizeof()` 这种用法，以及它是一个运算符。这不禁让人困惑。

我们都知道，函数调用的格式是 `fun()` 。所以，第一眼看到 `sizeof()` 的时候，会本能的将它判定为一个函数。从运算符的角度出发，它的用法应当是这样的：`sizeof int` 。

如果要得到 `a+b` 的运算结果的 `size` ，考虑这种用法：`sizeof a+b` ， 可是这种用法是错误的。因为 `sizeof` 的优先级比 `+` 高，所以 `sizeof a+b` 会得到 `(sizeof a) + b` 的值。

为了确保得到我们预期的结果，我们需要使用 `()` 来干预运算顺序，比如：`sizeof (a+b)` ， 这样它就会先计算 `a+b` 的值 `sum`，然后计算 `sizeof sum` 。 

### 计算变量大小

```c
char c1[] = {'a','b'};
char c2[] = "ab";
char *ptr;
sizeof c1;    // 2 Byte
sizeof c2;    // 包含字符串结束符，3 Byte
sizeof ptr;    // 指针大小，8 Byte，具体取决于操作系统位数
```

### 求数组长度

```c
int nums[10] = {0};
int len = sizeof(nums) / sizeof(int);    // 正确
int getLength(int nums[]) {
    int len = sizeof(nums) / sizeof(int);    // 错误
    return len;
}
```

**参考：**

+ https://stackoverflow.com/questions/1393582/why-is-sizeof-considered-an-operator

## strlen

```c
char c1[] = {'a', 'b'};
char c2[] = "ab";
char c3[3] = {'a', 'b'};
strlen(c1);    // 2
strlen(c2);    // 
```

```c
char str[] = "hello";
str[0] = 'X';    // 正确，此处默认应用了 strcpy 将常量的值复制给了 str
char *ptr = (char *) "hello";
ptr[0] = 'X';    // 错误，不能更改常量
```

函数返回的地址是否存在

```c
char * getChars() {
    char chars[10];
    return chars;
}
```



## 三元运算符

```c
// condition ? statement1 : statement2; 
int a = 1;
int b = 2;
int max = a > b ? a : b;
```

## 函数

声明

```c
void fun(int);
```

定义

```c
void fun(int n) {
    // some code
}
```

## 数组

一维数组的长度为**常量表达式**。
```c
int a[5];    // true，长度为常量
int a[5*2];    // true，长度为常量表达式
int a[var];    // false，长度 var 是一个变量
```

使用**宏定义**或者**常量**指定数组长度。
```c
#define N 100
const int M = 100;
int a[N];
int b[M];
```

数组赋初始值，未赋值的元素默认值为 0 。
```c
int a[3] = {0};    // 与 int a[3] = {0,0,0} 相同
int a[3] = {1};    // 与 int a[3] = {1,0,0} 相同
```

常见数组排序的方法有：
1. 快速排序
2. 选择排序
3. 冒泡排序
4. 希尔排序


数组名为常量，对常量进行自增自减操作是错误的。
```c
int a[2];
a++;    // false
a--;    // false
a+1;    // true
```

二维数组引用
```c
int nums[3][2];
int i, j;
int e;
// 取第 i 行 的 第 j 列元素的值
e = *(nums[i] + j);
e = *(*(nums + i)+j);
```

字符数组中，`strlen()` 函数返回的是有效字符数，其中不包括结尾的 `\0` 字符。



## 动态内存分配

### malloc

数组的长度只能为常量，如果需要根据输入生成对应大小的数组，就必须使用 `malloc` 。

```c
int main() {
    int length;
    scanf("%d", &length);
    int a[length];    // 错误
    int *a = (int *) malloc (sizeof(int) * length);    // 正确
    return 0;
}
```

**案例**：

```c
// 错误：在主函数声明了变量，在子函数对其动态分配大小，并进行初始化赋值操作
void init(int **nums) {
    nums = (int **) malloc (sizeof(int *));
    // 赋值代码
}
int main() {
	int **nums;
    init(nums);    // 调用了子函数后，nums仍然为空，给形参分配的大小与实参无关
}

// 正确：在主函数完成动态大小分配，在子函数进行赋值操作
void init(int **nums) {
    // 赋值代码
}
int main() {
    int **nums = (int **) malloc (sizeof(int *));
    init(nums);
}
```

`malloc` 分配的内存是在堆中，函数中的变量是存储在栈中，退出函数后，在其内部动态分配的空间依然得到了保留。

### realloc

### free