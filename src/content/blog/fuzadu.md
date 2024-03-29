---
title: 衡量算法和数据结构的尺子：复杂度计算
date: 2020-05-27 23:22:48
description: 凌晨时分，你写出一段代码，实现了某个算法，优雅如诗。默默在心底说一声：NB！但你知道这注定无人知晓，因为身边早已寂静无声。但你写的代码是否真的 NB？复杂度或许能够解决这个问题。
image: "https://xrtech.oss-cn-shanghai.aliyuncs.com/uPic/2023-06/posts/1590598066901.jpg"
author: "乌日其浪"
categories: [算法]
tags: [算法, 复杂度]
---

凌晨时分，你写出一段代码，实现了某个算法，优雅如诗。默默在心底说一声：“NB！” 但你知道这注定无人知晓，因为身边早已寂静无声。
但你写的代码是否真的 NB？复杂度或许能够解决这个问题。

> 学习数据结构与算法，就注定无法绕开复杂度分析。

## 1.为什么需要复杂度分析？

试想，如果不采用复杂度分析，判断一个算法的好最好的方法就是直接将代码跑一遍。但直接运行代码来观察其效率有几个致命的缺点：

1. 事后分析：你不得不把代码写出来才能评估一种算法是否高效
2. 依赖测试环境：测试电脑的配置直接决定了算法的运行时间。
3. 测试结果受到数据规模的影响：不同的算法在不同数据规模下的表现不一致。

但是评价算法的执行效率本身就是一种“刚需”，毕竟算法就是为了提升效率，我们向领导汇报的时候，总得需要一个指标来告诉领导我们的价值吧？

所以说，我们需要一种评价方法，能够在事前就大概估算出一个算法的执行效率 。 —— 复杂度

## 2.大 O 复杂度表示法

如何把算法的效率直观的表达出来呢？我们来看一段代码

```js
function cal(n) {
  let sum = 0;
  let i = 1;
  for (; i <= n; ++i) {
    sum = sum + i;
  }
  return sum;
}
```

**复杂度计算是一种粗略的估算，那么我们也就粗略的认为对于计算机来说，执行每一行代码的时间是相同的。**  
我们假设计算机执行一行代码需要的时间为：T。 那么

- 第二行 let sum = 0；需要消耗 T\*1；
- 第三行 let i = 1； 需要消耗 T\*1；
- for 循环将执行 N 次，每次执行会做执行一次 sum=sum+1 与 ++i ，需要消耗 N \* 2T；
- 第七行 return sum；仅仅是一个返回值，并不涉及到数据运算与赋值，不算消耗。
  根据以上计算，该算法执行的时间消耗是 **（2N+2）\*T**

按照这个思路，下面这段代码

```js
function cal(int n){
    let sum = 0;
    let i = 1;
    for(; i<=n;++i){
        j = 1;
        for(; j <= n ; ++j){
            sum = sum + i * j ;
        }
    }
    return sum;
}
```

为了便于理解，我还是罗嗦的依次分析：

- 第二行，第三行 各消耗一个 T ；
- 外层 for 循环中 j = 1 与 ++i 总计执行了 2\*N 次；
- 内层 for 循环中 sum=sum + i\*j 与 ++j 总计执行了 $2N^2$次
- 总计该算法执行消耗了 T(n) = $2N^2+2N+2(T)$

你有没有发现，无论如何计算，T(n) 与 n 有一个比例关系。代码执行次数越多，T(n)与 n 的比例越大。
我们把这个规律成一个公式

> $T(n) = O(f(n))$

- T(n) 表示代码执行的总时间
- n 表示数据规模的大小
- f(n) 表示代码执行次数的总和
- 公式中 O 表示代码执行时间 T(n)与执行次数 f(n) 成正比。

第一个例子就可以写作 T(n) = O(2n+2)，第二个例子可以写作 T(n) = O($2n^2$+2n+3)。这就是**大 O 时间复杂度表示法**

大 O 的时间复杂度并不是表示具体代码需要执行多久，而是反映了**代码执行时间随着数据规模增大的变化趋势**。所以，也叫**作渐进时间复杂度**（asymptotic time complexity），简称**时间复杂度**。

在时间复杂度计算中，通常我们要把低阶、常量、系数省略掉，毕竟当 n 很大时，这些因素的影响会变得很小。更重要的是我们毕竟是一种粗略的计算。

所以，第一个例子可以写作 T(n) = O(n) 第二个例子写作 T(n) = O($n^2$)

确实，就是这么简单。
![简单.png](https://xrtech.oss-cn-shanghai.aliyuncs.com/uPic/2023-06/posts/1590597451637.png)

## 3.时间复杂度分析

复杂度的定义我们知道了，具体分析一段代码的时间复杂度时有以下几个技巧。

1. 最多法则: 只关注循环次数最多的代码
2. 加法法则：总复杂度等于量级最大的代码的复杂度
3. 乘法法则：嵌套代码的复杂度等于嵌套内外代码复杂度的乘积

篇幅有限，这里就不罗列代码举例了。多思考，多练习即可。这种入门知识想必难不倒阁下。

> 我亦无他，惟手熟尔。

下面列举一些常用的复杂度量级，并一一分析。

![](https://xrtech.oss-cn-shanghai.aliyuncs.com/uPic/2023-06/posts/1590598066901.jpg)

#### 1. O(1)

O(1)是常量级的时间复杂度表示方式，并不是说只有一行代码. 只要代码执行时间不随数据量增加变化而变化，即使它有成千上万行代码，其时间复杂度仍然是 O(1)

```
let i =2 ;
let j =3 ;
let sum = i + j ;
```

#### 2. $O(logn)$,$O(n*logn)$

对数阶时间复杂度很常见,但是也是最难的分析的一种. 我门通过一个例子来说明一下:

```c
i = 1 ;
while(i <= n){
    i = i * 2;
}
```

根据我们之所说的**最多法则**,我们只看循环次数最多的部分.第三行代码: i = i \*2; 只要我们知道这行**代码循环了多少次**,复杂度就分析出来了.
从代码中可以看出,

- 变量 i 的值从 1 开始取
- 每循环一次就乘以 2.当大于 n 时循环结束.
  那么到底循环了多少次呢? 我门不妨穷举一下 i 在每次循环中的变化:

$2^0 , 2^1 ,  2^2  ...  2^k ... 2^x$

当 $2^x$ > n 时循环结束,循环次数是 x . 根据高中数学知识,我们知道 x =$log_2 n$ .所以这段代码的时间复杂度是 $O(log_2n)$

现在我们把这段代码稍微改一下

```c
i = 1 ;
while(i <= n){
    i = i * 3;  //这里做了修改
}
```

根据我们之前的思路,不难算出它的时间复杂度是 $O(log_3n)$ .但实际计算过程中不论是以 2 为底还是以 3 位底亦或是以 10 为底,我们把对数阶复杂度都标记为 $O(logn)$

why? 我们再拿出高中数学知识, 对数之间是可以相互转换的

> $log_3n = log_3 2*log_2n$

在打 O 标记复杂度方法中,系数是可以忽略的,所以我们就粗略的认为 $O(log_2n)=O(log_3n)$ 这样在对数阶时间复杂度分析中,我们把对数的"底"这个概念忽略掉,统一表示为 O(logn)

那么 O(nlogn) 就很容易理解了: 如果一个方法的时间复杂度是 O(logn),我们把它循环 n 次,它的时间复杂度就是 O(nlogn) . Esay!

> O(nlogn) 也是一种非常常见的算法时间复杂度。比如，归并排序、快速排序的时间复杂度都是 O(nlogn)。

#### 3.O(m+n),O(m\*n)

有的时候,一个算法的时间复杂度是由两个数据共同决定的,我们再举个例子

```js
function cal(m, n) {
  let sumA = 0;
  for (let i = 0; i < m; ++i) {
    sumA = sumA + i;
  }
  let sumB = 0;
  for (let i = 0; i < n; ++i) {
    sumB = sumB + i;
  }
  return sumA + sumB;
}
```

不难看出,上面代码的时间复杂度是 O(m+n) 因为 m 和 n 的数据量大小我们都是不知道的,无法忽略,就不能参考加法原则了. 类似的还有 O(m\*n).

## 4.空间复杂度分析

掌握了时间复杂度和大 O 表示法,空间复杂度的计算就显得"A piece of cake"(这是贫道初中最喜欢用的的一个句子)

前文说到, 时间复杂度其实是 **算法执行时间与数据规模之间的增长关系.**
类似的,空间复杂度就是**算法的存储空间与数据规模之间的增长关系**

继续举个例子,毕竟,代码是工程师最擅长的语言:

```js
function print(n) {
  let arr = [];
  let i = 0;
  for (i; i < n; ++i) {
    arr.push(i * i);
  }
  for (i = n - 1; i >= 0; --i) {
    console.log(arr[i]);
  }
}
```

和时间复杂度分析一样,:

- 在第二行,我们申请了一个空间存储 arr,
- 在第三行我们申请了一个空间存储变量 i  
  除此之外所有的代码都没有占用跟多空间,所以整段代码的空间复杂度是 O(n+1),省略常量阶,省略系数最后这段代码的控件复杂度是 O(n)

我们常见的空间复杂度就是 O(1)、O(n)、O(n2 )，像 O(logn)、O(nlogn) 这样的对数阶复杂度平时都用不到。而且，空间复杂度分析比时间复杂度分析要简单很多。

> 复杂度计算就是一把尺子,一把从时间和空间的角度共同丈量算法执行效率的尺子. 这把尺子或许很粗糙,但是其实很简单很有用.

以上就是复杂度计算的基础内容了,本文是由王争的在线教程[<数据结构与算法之美>](!https://time.geekbang.org/column/intro/126)总结而来,如果想了解进阶的复杂度计算知识你可以学习这门课程或者在百度搜索:

- 最好情况时间复杂度（best case time complexity）
- 最坏情况时间复杂度（worst case time complexity）
- 平均情况时间复杂度（average case time complexity）
- 均摊时间复杂度（amortized time complexity）

而作为一个专注于解决问题的工程师,我认为你掌握到现在这个程度就完全满足以后的开发需求了.

![](https://xrtech.oss-cn-shanghai.aliyuncs.com/uPic/2023-06/posts/1590653100184.jpg)
