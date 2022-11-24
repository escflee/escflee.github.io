#背包问题 #动态规划 

## 题目描述

Many years ago , in Teddy’s hometown there was a man who was called “Bone Collector”. This man like to collect varies of bones , such as dog’s , cow’s , also he went to the grave …
The bone collector had a big bag with a volume of V ,and along his trip of collecting there are a lot of bones , obviously , different bone has different value and different volume, now given the each bone’s value along his trip , can you calculate out the maximum of the total value the bone collector can get ?

## Input

The first line contain a integer T , the number of cases.
Followed by T cases , each case three lines , the first line contain two integer N , V, (N <= 1000 , V <= 1000 )representing the number of bones and the volume of his bag. And the second line contain N integers representing the value of each bone. The third line contain N integers representing the volume of each bone.

## Output

One integer per line representing the maximum of the total value (this number will be less than 231).

## 输入样例

```text
1
5 10
1 2 3 4 5
5 4 3 2 1
```

## 输出样例

```text
14
```

## 题解

0-1背包

```c++
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int t,n,maxc,v[1005],c[1005],dp[1005];
    scanf("%d",&t);
    while(t--)
    {
        memset(dp,0,sizeof(dp));
        scanf("%d%d",&n,&maxc);
        for(int i=1;i<=n;i++)
            scanf("%d",&v[i]);
        for(int i=1;i<=n;i++)
            scanf("%d",&c[i]);
        for(int i=1;i<=n;i++)
        {
            for(int j=maxc;j>=c[i];j--)//0-1背包，逆序按容量由大到小更新
                dp[j]=max(dp[j-c[i]]+v[i],dp[j]);
        }
        printf("%d\n",dp[maxc]);
    }
    return 0;
}
```