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
- ![](https://tuceng-1312762148.cos.ap-nanjing.myqcloud.com/Obsidian/202211061852805.png)

## 三，函数的传址和传值
### 1，传值
就是函数将实参的值copy给形参。接下来我们看一个案例。
```C
void swap(int a, int b)
{
	int temp;
	temp = a;
	a = b;
	b = temp;
}

int main()
{
	int test1=1, test2=2;
	swap(test1, test2);
	printf("test1=%d,test2=%d", test1, test2); // test1=1, test2=2 
}
```
我们看这里使用了交换函数之后test1和test2的值并没有交换，这是为什么呢？其实这就是因为形参只是copy的实参的值，函数确实把实参的数交换了，但是函数执行完之后形参都释放了，并不会影响实参。


### 2，传址
就是将实参的地址传给形参，对形参的地址操作时侯会影响实参。我们来看一个案例。
```C
void swap(int* a, int* b)
{
	int temp;
	// 这里都是操作的地址
	temp = *a;
	*a = *b;
	*b = temp; 	
}

int main()
{
	int test_1=1, test_2=2;
	// 因为函数是对地址进行操作，所有要进行取地址&
	swap(&test_1, &test_2);
	printf("test_1=%d, test_2=%d",test_1,test_2);  // test_1=2,test_2=1;
}

```
这里我们看到经过交换函数后，这里的两个数交换了，因为这里是对实参的地址进行的操作。

# 四，函数使用
## 1，memset初始化数组
- 头文件 #include<string.h>
- 语法
	- memset(数组名,初始化数,sizeof(数组名))

# 五，内存分配
[参考博客](https://www.cnblogs.com/wanghuaijun/p/6509016.html)
## 1，区域
![](https://tuceng-1312762148.cos.ap-nanjing.myqcloud.com/Obsidian/%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D.png)

- **代码区**
	- 存放CPU执行的机器指令（可共享、只读）
	- 规划了局部变量的相关信息

- **全局初始化数据区/静态数据区**（只初始化一次）
	- 包含了在程序中明确被初始化的全局变量、静态变量（包括全局静态变量和局部静态变量）和常量数据（如字符串常量）[[C-C++#六，全局变量、局部变量、静态全局变量、静态局部变量的区别 | 全局变量和静态变量]]
	- 例如，一个不在任何函数内的声明（全局数据）

- **未初始化区（BBS)：** 存入全局未初始化变量，在运行时改变值。

- **栈区：** 编译器自动分配释放，存放函数的参数值、局部变量的值。不存在碎片。**增长方向**向下，向内存地址减小的方向。连续的地址区域。

- **堆区：** 用于动态分配内存，由程序员分配和释放，如果不释放可能有OS回收。**缺点** 会产生碎片，影响程序效率。| **增长方向**向上，向内存地址增加的方向。用链表存储地址。

- **总结** 
	- 临时数据及需要再次使用的代码、函数参数值、局部变量值，在运行时放入栈区中，生命周期短。
	- 全局数据和静态数据有可能在整个程序执行过程中都需要访问，因此单独存储管理。
	- 堆区由用户自由分配，以便管理。
例子：
```C++
//main.cpp   
int a = 0;    //a在全局已初始化数据区   
char *p1;    //p1在BSS区（未初始化全局变量）   
main()   
{  
int b;    //b在栈区  
char s[] = "abc"; //s为数组变量，存储在栈区，  
//"abc"为字符串常量，存储在已初始化数据区  
char *p1，p2;  //p1、p2在栈区  
char *p3 = "123456"; //123456\0在已初始化数据区，p3在栈区   
static int c =0；  //C为全局（静态）数据，存在于已初始化数据区  
//另外，静态数据会自动初始化  
p1 = (char *)malloc(10);//分配得来的10个字节的区域在堆区  
p2 = (char *)malloc(20);//分配得来的20个字节的区域在堆区  
free(p1);  
free(p2);  
}
```

# 六，全局变量、局部变量、静态全局变量、静态局部变量的区别
- **静态存储方式** 变量定义时就分配存储单元并一直保持不变、直到整个程序结束（全局变量、静态变量）

- **从作用域看**
	- **全局变量**：具有全局作用域，只在一个源文件中定义，就可以用于所有的源文件。
	- **静态局部变量：** 具有局部作用域，只被初始化一次，只对定义自己的函数体始终可见。
	- **局部变量：** 局部作用域、自动对象，只在函数执行期间存在，函数的一次调用结束后就会销毁，其占用的内存也会被收回。
	- **静态全局变量：** 全局作用域 ，和全局变量的区别，只能作用到定义它的文件中，不能作用到其他的文件。

- **从分配内存空间看**
	- 全局变量、静态局部变量、静态全局变量都在静态存储区分配空间，局部变量在栈空间分配空间。
	- 全局变量本身就是静态存储方式。
	- **静态全局变量和全局变量的区别：** 全局变量作用域是各个源文件都能用，静态全局变量只有该源文件可用。
	- 静态变量会被放在静态数据存储区，下一次调用保持原来的赋值。和堆栈变量和堆变量的区别。

# 七，STL
## 1，vector 动态数组
### 基本操作
- 声明 vector<类型> a;
- a.push_back(4)   增加4  是先创建这个元素然后再copy到容器的后面。可能带来内存溢出。**注意清空元素** 
- a.emplace_back(4)  也是在尾部添加一个元素，但是直接在尾部创建。
- a.size()  数组长度
- a.clear()  清空数组
### vector的内存分配与释放
- [参考博客](https://zhuanlan.zhihu.com/p/338390842)
- 分配
	- b.size() : **容器当前拥有的元素个数** 
	- b.capacity():**容器申请的最大可容纳元素总数** 
	- capacity和size相等就会扩容
	- **通常vector会申请大于当前已有元素的内存** 
	- 如果添加元素没有超出capacity不会重新申请内存。
	- b.push_back()：插入一个新元素时候如果内存不够了，就开辟个新的，把之前的元素copy到新内存中，并释放原有的内存。效率低，防止频繁分配，每次申请空间都会多申请空间。
	- b.clear()   如果是一维的会清空元素，但是不会回收内存。
- 释放
	- **用swap()强制回收内存**
	```c++
	vector<int>(v).swap(v);

	vector(int)(v)是一个匿名对象，操作完了系统自动回收内存
```

		
![](https://tuceng-1312762148.cos.ap-nanjing.myqcloud.com/Obsidian/202211151516032.png)

### 统计内存开辟次数
```C++
vector<int> v;
		
		int num = 0; // 统计开辟次数
		int *p = NULL; 
		for (int i=0; i<100000; i++)
		{
			v.push_back(i);
			
			if(p!=&v[0])
			{
				p = &v[0];
				++num;
			}
		}
		
		cout << "num=" << num << endl;
```


## 2，set集合
- 属性
	- 不会有重复元素，有重复会自动删除一个
	- 自动按照从小到大的顺序进行排序
- 基本操作
	- 声明 set<类型> a;
	- a.insert(4)  增加4
	- a.erase(5)  删除5，集合里没有也不会报错
	- a.cout(4)  用来判断有没有4
	- a.size() 集合长度
- 输出
```C++
for(set<int>::iterator it = a.begin(); it != a.end(); it++){//特别注意，it是一个指针。 cout << *it << " ";//由于it是一个指针，所以要用*it。 }
```

### unordered_set容器
这是个无序set容器，也交哈希集合（因为无序容器的底层使用哈希存储数据的，拉链法解决冲突）
- 不以键值对形式存储数据，直接存储数据的值
- 容器内部存储的各个元素的值互不相等，且不能 被修改
- 不会对内部存储的数据进行排序。
- 适用于**查找问题** 
- **少了排序的过程还降低了时间复杂度** 
-  **基本操作** 
	- 1，s.emplace(i)   插入数据
	- 2，s.erase(i)   删除数据


## 3，map映射表
- 基本操作
	- map<string, int>   m;
	- m[key] = val   赋值
	- m[key]  输出
	- cout(key)  判断一个数是否在映射表上

## 4，unordered_map容器
可以看作是无序的map容器，容器内部不会自行对存储的键值对进行排序。

## 5，队列queue
 - 基本操作
 ```c++
  1，queue<int> q; 定义空队列<br>
  2，q.push(i);  压入队
  3，q.front()  队头
  4，q.back()  队尾
  5，q.size() 元素个数。
```

## 6，hashset
- 基本操作
```c++
	1,HashSet<string>  name = new HashSet();  创建
	2,hashset.add("abc");  
	3,hashset.clear(); 清空。  
	4,hashset.remove(Object o); 如果指定元素存在于此 set 中，则将其移除。  
	5,hashset.isEmpty()  判空  
	6,hashset.contains(Object o) 如果此 set 包含指定元素，则返回 true。  
	7,hashset.size()  返回此 set 中的元素的数量（set 的容量）。  
	8,System.out.println(hashset); 输出
```

## 7，string
- 基本操作
```c++
	1,str.max_size()  出现的最大字符
	2,str.insert(pos, char)  在指定的位置pos前插入字符char
	3,str.replace(pos1, pos2, char）将pos1到pos2位置的字符换成char
	4,tolower(s[i])  转换成小写
	5,toupper(s[i])  转换成大写
	6,str.find("char")  找
	7,str.find("i", pos) 从下标pos寻找字符i
	8,str.substr(pos,n)  子字符串，从pos开始n个字符
	9,str.substring(
```

# 五，栈
- 基本操作
```c++
1，stack<int> a； 初始化
2，a.push(i);   将一个元素压入栈中
3，a.pop()；访问栈顶元素
4，a.size()；移除栈顶元素
```

>1，栈是从高地址向低地址拓展（栈底地址高）
>2，栈是自顶向下增长




