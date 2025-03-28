---
title: 教妹学Java：聊聊基本数据类型的转换
shortTitle: Java数据类型转换
category:
  - Java核心
tag:
  - Java语法基础
description: 本文详细讲解了Java数据类型转换，包括强制类型转换和自动类型转换。通过学习本文，您将深入理解Java数据类型转换的原理和使用场景，掌握各种类型转换的技巧，避免常见的类型转换错误，提高Java编程效率。
head:
  - - meta
    - name: keywords
      content: Java, 数据类型转换, 强制类型转换, 自动类型转换
---


“三妹，今天我们来聊聊 Java 中的数据类型转换。”我开门见山地对三妹说。

“好啊。”三妹愉快地说。

数据类型转换的目的是确保在表达式求值时，不同类型的数据能够相互兼容。

### 01、自动类型转换

自动类型转换（自动类型提升）是 Java 编译器在不需要显式转换的情况下，将一种基本数据类型自动转换为另一种基本数据类型的过程。这种转换通常发生在表达式求值期间，当不同类型的数据需要相互兼容时。自动类型转换遵循以下规则：

- 如果任一操作数是 double 类型，其他操作数将被转换为 double 类型。
- 否则，如果任一操作数是 float 类型，其他操作数将被转换为 float 类型。
- 否则，如果任一操作数是 long 类型，其他操作数将被转换为 long 类型。
- 否则，所有操作数将被转换为 int 类型。

需要注意的是，自动类型转换只发生在兼容类型之间。例如，从较小的数据类型（如 int）到较大的数据类型（如 long 或 double）的转换是安全的，因为较大的数据类型可以容纳较小数据类型的所有可能值。

```
byte -> short -> int -> long -> float -> double
char -> int -> long -> float -> double
```

下面是一个简单的示例，演示了自动类型转换：

```java
int intValue = 5;
double doubleValue = 2.5;

// 自动类型转换：intValue 被转换为 double 类型
double result = intValue * doubleValue;
System.out.println("结果: " + result); // 输出：结果: 12.5
```

在这个示例中，我们有一个 int 类型的变量 intValue 和一个 double 类型的变量 doubleValue。当我们将它们相乘时，根据自动类型转换的规则，intValue 将被转换为 double 类型，以便将两个 double 类型的操作数相乘。最终结果将是一个 double 类型的值：12.5。

再来举个例子，顾客到超市购物，购买牙膏 2 盒，面巾纸 4 盒。其中牙膏的价格是 10.9 元，面巾纸的价格是 5.8 元，求商品总价格。实现代码如下：

```java
float price1 = 10.9f; // 定义牙膏的价格，单精度浮点型float
double price2 = 5.8; // 定义面巾纸的价格，双精度浮点型double
int num1 = 2; // 定义牙膏的数量，整型 int
int num2 = 4; // 定义面巾纸的数量
double res = price1 * num1 + price2 * num2; // 计算总价
System.out.println("一共付给收银员" + res + "元"); // 输出总价
```

上述代码中首先定义了一个 float 类型的变量存储牙膏的价格，然后定义了一个 double 类型的变量存储面巾纸的价格，再定义两个 int 类型的变量存储物品的数量，最后进行了乘运算以及和运算之后，将结果储存在一个 double 类型的变量中进行输出。

```
一共付给收银员44.99999923706055元
```

从执行结果看出，float、int 和 double 三种数据类型参与运算，最后输出的结果为 double 类型的数据。这种转换一般称为“表达式中类型的自动提升”。

自动类型提升有好处，但它也会引起令人疑惑的编译错误。例如，下面看起来正确的程序却会引起问题：

```java
byte b = 50;

b = b * 2; // Type mismatch: cannot convert from int to byte
```

如上所示，第二行会报“类型不匹配：无法从int转换为byte”错误。

该程序试图将一个完全合法的 byte 型的值 `50*2` 存储给一个 byte 型的变量。但是当表达式求值的时候，操作数被自动的提升为 int 型，计算结果也被提升为 int 型。这样表达式的结果现在是 int 型，不强制转换它就不能被赋为 byte 型。

所以应该使用一个显示的强制类型转换，例如：

```java
byte b = 50;
b = (byte)(b*2);
```

这样就能产生正确的值 100。

但如果是下面这样的代码，就不会报错：

```java
byte b = 50;
b *= 2;
```

这是因为 `b *= 2` 等价于 b = `(byte)(b*2)`，编译器会自动进行强制类型转换。

另外，之前有[球友](https://javabetter.cn/zhishixingqiu/)不太理解这样的代码：

```java
byte b = 50;
```

球友认为 50 默认的是 int 类型，大范围类型转换为小范围类型不应该是需要强制转化吗？

我的回答是编译器帮我们自动做了隐式转换，一个值是 int 类型，但是它的值在 byte 类型的取值范围（-127到 128）内，那么编译器会自动将 int 类型转换为 byte 类型。

![](https://cdn.tobebetterjavaer.com/stutymore/type-cast-20240110162913.png)

希望大家能顾理解，自动类型转换并不局限于小类型到大类型，也可以是大类型到小类型，只要值在小类型的取值范围内，编译器就会自动帮我们做隐式转换。

注意：char 类型比较特殊，char 自动转换成 int、long、float 和 double，但 byte 和 short 不能自动转换为 char，而且 char 也不能自动转换为 byte 或 short。

### 02、强制类型转换

强制类型转换是 Java 中将一种数据类型显式转换为另一种数据类型的过程。与自动类型转换不同，强制类型转换需要程序员显式地指定要执行的转换。强制类型转换在以下情况中可能需要：

- 将较大的数据类型转换为较小的数据类型。
- 将浮点数转换为整数。
- 将字符类型转换为数值类型。

需要注意的是，强制类型转换可能会导致数据丢失或精度降低，因为目标类型可能无法容纳原始类型的所有可能值。因此，在进行强制类型转换时，需要确保转换后的值仍然在目标类型的范围内。

```
double -> float -> long -> int -> char -> short -> byte
```

以下是一个简单的示例，演示了强制类型转换：

```java
double doubleValue = 42.8;

// 强制类型转换：将 double 类型转换为 int 类型
int intValue = (int) doubleValue;
System.out.println("整数值: " + intValue); // 输出：整数值: 42
```

在这个示例中，我们有一个 double 类型的变量 doubleValue。我们希望将其转换为 int 类型的变量 intValue。为此，我们使用强制类型转换语法，即在要转换的变量之前加上目标类型的括号（如 (int)）。

需要注意的是，将 doubleValue 转换为 int 类型时，小数部分将被截断。因此，输出结果将是：Integer value: 42。在这种情况下，精度丢失是可以接受的，但在其他情况下，我们可能需要更加小心地处理类型转换以避免数据丢失。

顾客到超市购物，购买牙膏 2 盒，面巾纸 4 盒。其中牙膏的价格是 10.9 元，面巾纸的价格是 5.8 元，求商品总价格，在计算总价时采用 int 类型的数据进行存储。实现代码如下：

```java
float price1 = 10.9f;
double price2 = 5.8;
int num1 = 2;
int num2 = 4;
int res2 = (int) (price1 * num1 + price2 * num2);
System.out.println("一共付给收银员" + res2 + "元");
```

在上述实例中，有 double 类型、float 类型和 int 类型的数据参与运算，其运算结果默认为 double 类型，题目要求的结果为 int 类型，因为 int 类型的取值范围要小于 double 类型的取值范围，所以需要进行强制类型转换。

```
一共付给收银员44元
```

### 小结

考虑下面这几个算式的结果：

```java
int a = 1500000000, b = 1500000000;
int sum = a + b;
long sum1 = a + b;
long sum2 = (long)a + b;
long sum3 = (long)(a + b);
```

“这几个结果应该一样吧，都是求 a+b 的结果嘛。”三妹想当然地得出了她的结论。

“不是的，三妹，你看看这几个结果的值。”我说。

```java
-1294967296
-1294967296
3000000000
-1294967296
```

“这是怎么回事？为什么结果不一样？”三妹有点懵。

“这是因为int 类型的取值范围是 `-2147483648~2147483647`，而 a 和 b 的值都是 1500000000，和超出了 int 类型的取值范围，所以会出现溢出的情况。”我解释道。

“那为什么 sum2 的结果是 3000000000 呢？”三妹又问。

“这是因为 sum2 的计算过程是先将 a 转换为 long 类型（强制类型转换），然后再和 b 相加（隐式类型转换），此时的结果也是 long 型，而 3000000000 并没有超出 long 型的取值范围。”我说。

- `int sum = a + b`，a 和 b 都是 int 类型，所以 a+b 的结果也是 int 类型，但是 a+b 的结果超出了 int 类型的取值范围，所以会出现溢出的情况。
- `long sum1 = a + b`，a 和 b 都是 int 类型，于是 a+b 的和也是 int 类型，但超出了 int 的取值范围，所以会出现溢出的情况；如果 a+b 的和没有超出 int 取值范围，其实会将 a+b 的结果隐式转换为 long 类型。
- `long sum2 = (long)a + b`，a 是 int 类型，但是 `(long)a` 将 a 强转为了 long 类型，然后再和 b 相加，此时 b 将隐式提升为 long 型，于是等式右边的结果也是 long 型，而 3000000000 并没有超出 long 型的取值范围。
- `long sum3 = (long)(a + b)`，a 和 b 都是 int 类型，a+b 的结果也是 int 类型，但超出了 int 的取值范围，所以会出现溢出的情况；即便是外面有一层 long 的强转，但还没有来得及强转，a+b 的结果已经溢出了，所以强转也没用。

那以上，基本上就把 Java 中的基本类型转化讲清楚了。


---

GitHub 上标星 10000+ 的开源知识库《[二哥的 Java 进阶之路](https://github.com/itwanger/toBeBetterJavaer)》第一版 PDF 终于来了！包括Java基础语法、数组&字符串、OOP、集合框架、Java IO、异常处理、Java 新特性、网络编程、NIO、并发编程、JVM等等，共计 32 万余字，500+张手绘图，可以说是通俗易懂、风趣幽默……详情戳：[太赞了，GitHub 上标星 10000+ 的 Java 教程](https://javabetter.cn/overview/)


微信搜 **沉默王二** 或扫描下方二维码关注二哥的原创公众号沉默王二，回复 **222** 即可免费领取。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/gongzhonghao.png)