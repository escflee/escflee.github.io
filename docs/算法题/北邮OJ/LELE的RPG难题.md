#动态规划 

## 题目描述

人称“AC女之杀手”的超级偶像LELE最近忽然玩起了深沉，这可急坏了众多“Cole”（LELE的粉丝,即”可乐”）,经过多方打探，某资深Cole终于知道了原因，原来，LELE最近研究起了著名的RPG难题:

有排成一行的ｎ个方格，用红(Red)、粉(Pink)、绿(Green)三色涂每个格子，每格涂一色，要求任何相邻的方格不能同色，且首尾两格也不同色．求全部的满足要求的涂法.

以上就是著名的RPG难题.

如果你是Cole,我想你一定会想尽办法帮助LELE解决这个问题的;如果不是,看在众多漂亮的痛不欲生的Cole女的面子上,你也不会袖手旁观吧?

## Input

输入数据包含多个测试实例,每个测试实例占一行,由一个整数N组成，(0<n<=50)。

## Output

对于每个测试实例，请输出全部的满足要求的涂法，每个实例的输出占一行。

## 输入样例

```text
1
2
```

## 输出样例

```text
3
6
```

## 题解

以前n-1格的涂色方式中首尾颜色是否相同为分类依据，相同时种类数共有2*d[n-2]种，不相同时种类共d[n-1]种。
注意d[3]并不满足递推公式。

```c++
#include <bits/stdc++.h>
using namespace std;
int main()
{
    long long d[51];
    d[1] = 3;
    d[2] = 6;
    d[3] = 6;//注意
    for (int i = 4; i < 51; i++)
    {
        d[i] = d[i - 1] + 2 * d[i - 2];
    }
    int n;
    while (scanf("%d", &n) == 1)
    {
        printf("%lld\n", d[n]);
    }
    return 0;
}
```