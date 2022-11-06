## 一，全局数组和局部数组的区别
- 全局数组（定义数组再main函数外面）会自动将数组初始化为0。
```c++
#include <iostream>
#include <bits/stdc++.h>
using namespace std;

int a[26];
int main()
{	
	int length = sizeof(a)/sizeof(int);
	for(int i=0;i<length;++i)
	{
		cout << a[i] << " ";
	}
	// 0 0 0 0 0 0
}
```
- 局部数组会随机分配数
```c++
#include <iostream>
#include <bits/stdc++.h>
using namespace std;


int main()
{	int a[26];
	int length = sizeof(a)/sizeof(int);
	for(int i=0;i<length;++i)
	{
		cout << a[i] << " ";
	}
	// 8 0 4254401 0 4253392 0 34 0 14488416 0 1 0 -1 -1 4253493 0 1 0 4254425 0 0 0
}
```

## 二，vector
- 一种动态数组 他会自动给Int型的初始化为0，不像c语言的局部数组一样随机分配数字。
- 常用初始化方法，
	






