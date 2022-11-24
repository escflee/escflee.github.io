# STL

# 1.排序

头文件：#include <algorithm>

```cpp
//默认进行升序排序
sort(begin, end);//数组首地址，尾地址+1
//如对数组a[20]排序：
sort(a, a+20);
```

```cpp
//自定义比较函数以实现降序、结构体等特殊排序
sort(begin, end, compare);
bool compare(int a, int b)//返回a是否在b的前面
{
	return a<b;
}
```

