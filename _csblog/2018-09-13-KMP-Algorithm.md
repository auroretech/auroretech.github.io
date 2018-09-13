---
layout: post_for_cs
title:  "模式串匹配算法简介"
date:   2018-09-13
excerpt: "模式串匹配算法的基本思想和简单的介绍..."
image: "/images/2018-9-13-kmp-covergirl.jpg"
comments: true
---
<<<<<<< HEAD
![KMP](https://www.auroretech.com/KMP.png)
=======

## 模式串匹配算法思想
> [Source Here](http://www.auroretech.com/computer-science/KMP Algorithm/)
#### 简单的模式串匹配算法
> 利用计数指针**i**和**j**分别指示模式串和主串的字符位置
藉由循环判断**string1[ i ] == string2[ j ]**,则继续比较之后的字符，否则主串从
下一个字符起重新判断。

- 当处理字符时，其时间复杂度较好情况可达到**O(n+m)**
- 当处理01串时，其时间复杂度最坏情况可达**O(n x m)**是一个糟糕的选择

#### KMP简介
> ![](http://latex.codecogs.com/gif.latex?\\mathcal{D.E.Knuth\\space\\space\\spaceJ.H.Morris\\space\\space\\spaceV.R.Pratt})

- 时间复杂度**O(n+m)**
- 其算法的思想改进在于：每当第一趟匹配过程出现字符不等时，**不需回溯指针**，而是利用已经得到的“部分匹配”将模式向右“滑动”尽可能远的一段拘留了后，继续进行比较
> 问题：**主串**中的第**i**个字符($i$ 指针不回溯)应与**模式串**中的哪个字符再比较？

- 假设应与模式中第k(k<j)个字符继续比较则：
![](http://latex.codecogs.com/gif.latex?\\prime P_1 P_2\\dotsP_{k-1}\\prime=\\primeS_1S_2S_3\\dotsS_{i-1}\\prime)
而已经得到的“部分结果”是
![](http://latex.codecogs.com/gif.latex?\\primeP_{i-k+1}P_{j-k+2}\\dotsP_{j-1}\\prime=\\primeS_{i-k+1}S_{i-k+2}\\dotsS_{i-1}\\prime)
![](http://latex.codecogs.com/gif.latex?\\Longrightarrow\\primeP_1P_2\\dotsP_{k-1}\\prime=\\primeP_{i-k+1}P_{j-k+2}\\dotsP_{j-1}\\prime)
故：
![](http://latex.codecogs.com/gif.latex?next[j]=\\begin{cases}0&\\text{当j=1}\\\Max\\{k\\bracevert1<k<j且\\primeP_1P_2\\dotsP_{k-1}\\prime=\\primeP_{i-k+1}P_{j-k+2}\\dotsP_{j-1}\\prime&\\text{当此集合不空时}\\\1\\text{其他情况}\\end{cases})
![](http://latex.codecogs.com/gif.latex?\\Longrightarrow)
![](http://latex.codecogs.com/gif.latex?\S_i==P_j)时，则i和j自增，否则i不变，j退至next[j]位置继续比较，此时有两种结果：
1. j退到某个next值时字符比较相等，各指针自增1，继续匹配。
2. j退至值为零(即第一个字符就匹配失败)，则**主串**的下一个字符起和模式重新匹配。
> 下面是代码的描述
```
int index_KMp(string S, string T, int position)
{
    i = pos;//应要求从pos位置开始匹配
    j = 1;//模式串从1开始计数
    while(i <= length(S) && j<= length(T))
    {
        if(j == 0 || S[i]==T[j]){++i; ++j;}//j=0时依旧使i,j自增
        else j = next[j];
    }
    if(j > length(T))return i-length(T);//当匹配成功时，j会比模式串长度大1
    else return 0;
}
```
![KMP](http://www.auroretech.com/images/2018-9-12-kmp.jpg)

> 参考：《数据结构》（C语言版） 严蔚敏、吴伟民编著

> TODO：Next求值是KMP的关键，见下篇分析。
>>>>>>> 627dd56feebf72533e831bf1731fe60da764d86f
