---
title: "字符之间的大小写转换"
date: 2019-05-04T22:21:18+08:00
categories:
- 笔记
---

提到字符的大小写转换，思路有以下几种：

1. 小写字母在 `ASCII` 表中的数值更高，且与大写字母之间的差值： `a - A = 32`
2. 任意小写字母相对于 `a` 的差值，与相应大写字母相对于 `A` 的差值是相等的，即 `e - a = E - A`

转小写：

```java
// 方案 1
char upper = 'A';
char lower = (char) (upper - 32);

// 方案 2
char upper = 'A';
char lower = upper - 'A' + 'a';
```

转大写：

```java
// 方案 1
char lower = 'a';
char upper = (char) (lower + 32);

// 方案 2
char lower = 'a';
char upper = lower - 'a' + 'A'；
```

上述的方案都是在**已知大小写**的情况下，进行相应的转换。

但是，倘若要对字符进行**大小写反转**，即大写变成小写，小写变成大写，代码会变成这个样子：

```java
char ch;

// 转大写的操作
if (Character.isLowerCase(ch)) {
  ch = ch - 'a' + 'A';
}
// 转小写的操作
if (Character.isUpperCase(ch)) {
  ch = ch - 'A' + 'a';
}
```

这种方法，每次在转换前都要进行一次判断，根据判断的不同，执行的操作也不同。



为了更加简便，这里引入位运算：

```java
// 异或的运算规则
a ^ b = c
c ^ b = a

// 稍微更改一下等式
char a = 'a';
char A = 'A';
a ^ ? = A
A ^ ? = a
? = A ^ a = 97 ^ 65 = 32
```

使用位运算的代码如下：

```java
char ch;
ch ^= 32;
```

所以，任何字母与 32 进行异或运算后，都会得到大小写反转的结果，这样不仅省去了多余的判断，而且不论源字符如何，执行的操作都一样。