---
title: NOI Online 普及组 解题报告
date: "2020-3-8 00:00:00"
body: [article, comments]
cover: false
icons: [far fa-fire red]
valine:
  placeholder: 有什么感想? 发射犇犇
mathjax: true

headimg: https://cdn.jsdelivr.net/gh/Linhk1606/blog-cdn@0.0.7.1/img/vector_landscape_3.svg
tags:
 - DP
categories:
 - [OI, Solution]
description: "这次比赛我其实并没有参加, 所以题解也懒得看, 因此很有可能会咕"
---

## T1

{% folding blue, 查看题目 %}

<fancybox> <img src='https://cdn.jsdelivr.net/gh/Linhk1606/blog-cdn@0.0.5.4/img/noionline1.png'> </fancybox>

<btn center large>[<i class='fad fa-code'></i> 提交代码](https://www.luogu.com.cn/problem/P6188#submit)</btn>

{% endfolding %}

**分类讨论**

{% noteblock info %}
可以枚举三个数中**最小**的
{% endnoteblock %}

下面以 $a$ 最小的情况为例

* $$\because 7a+4b+3c=n$$
* $$\therefore 7a=n-4b-3c$$
* 由于我们要考虑 $a+b+c$ 最大. 因为 $b$ 的系数比 $c$ 要大, 而**和为定值**, 所以当 $a$ 固定时, $b$ **越小越好**
* 所以我们就可以令 $b$ 为 $a, a+1, a+2$ 中的一个(为了满足 $c$ 是整数, 找一个值使得 $n-4b-7a \equiv 0 \pmod {3}$)

将上面的方法重复套到 $b$ 和 $c$ 中, 跑三遍 `for` (有点丑)即可

{% noteblock warning %}
最后然后别忘了**校验得出来的值是否合理**
{% endnoteblock %}

{% codeblock 完整代码 lang:cpp %}
#include <cstdio>
#include <iostream>

#define min3(a, b, c) std::min(std::min(a, b), c)

const int coeff_a = 7, coeff_b = 4, coeff_c = 3;

int main()
{
    int n;
    int ans_a = -1, ans_b = -1, ans_c = -1;

    scanf("%d", &n);
    
    for (int a = 0; a <= n; a++) // a <= b, c
    {
        int b = a, c;
    
        while (((n - a * coeff_a - b * coeff_b) % coeff_c + coeff_c) % coeff_c != 0)
            b++;
        c = (n - a * coeff_a - b * coeff_b) / coeff_c;
    
        if (c < 0 || c < a)
            break; // 矛盾
    
        if (min3(ans_a, ans_b, ans_c) < a)
            ans_a = a, ans_b = b, ans_c = c;
        else if (min3(ans_a, ans_b, ans_c) == a && ans_a + ans_b + ans_c < a + b + c)
            ans_a = a, ans_b = b, ans_c = c;
    }
    
    for (int b = 0; b <= n; b++) // b <= a, c
    {
        int a = b, c;
    
        while (((n - a * coeff_a - b * coeff_b) % coeff_c + coeff_c) % coeff_c != 0)
            a++;
        c = (n - a * coeff_a - b * coeff_b) / coeff_c;
    
        if (c < 0 || c < b)
            break; // 矛盾
    
        if (min3(ans_a, ans_b, ans_c) < b)
            ans_a = a, ans_b = b, ans_c = c;
        else if (min3(ans_a, ans_b, ans_c) == b && ans_a + ans_b + ans_c < a + b + c)
            ans_a = a, ans_b = b, ans_c = c;
    }
    
    for (int c = 0; c <= n; c++) // c <= a, b
    {
        int a = c, b;
    
        while (((n - a * coeff_a - c * coeff_c) % coeff_b + coeff_b) % coeff_b != 0)
            a++;
        b = (n - a * coeff_a - c * coeff_c) / coeff_b;
    
        if (b < 0 || b < c)
            break; // 矛盾
    
        if (min3(ans_a, ans_b, ans_c) < c)
            ans_a = a, ans_b = b, ans_c = c;
        else if (min3(ans_a, ans_b, ans_c) == c && ans_a + ans_b + ans_c < a + b + c)
            ans_a = a, ans_b = b, ans_c = c;
    }
    
    if (ans_a == -1)
        puts("-1");
    else
        printf("%d %d %d", ans_a, ans_b, ans_c);
    return 0;
}

{% endcodeblock %}

## T2 & T3

> 留坑待填