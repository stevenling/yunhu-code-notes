# C++ Vector 易错点

### 一、概述

`vector` 的 `resize()` 函数会分配一个大小，这时候使用 `push_back()` 函数会在尾部进行添加。

### 二、分析

#### 2.1 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	vector<int> a;
	a.clear();
	a.resize(5);
	for(int i = 0; i < 5; i++)
	{
		a.push_back(i+1);
	}
	for(int i = 0; i < a.size(); i++)
	{
		cout << a[i] << " ";
	}
	
   return 0;
}
```

#### 2.2 输出结果

> 0 0 0 0 0 1 2 3 4 5

这不是我们想要的，使用以下方法解决。

### 三、解决方法

#### 3.1 方法 1

使用 `push_back()` 进行添加值，那么 `clear()` 之后就不要再分配大小了。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	vector<int> a;
	a.clear();
	// 不要进行 resize
	for(int i = 0; i < 5; i++)
	{
		a.push_back(i+1);
	}
	for(int i = 0; i < a.size(); i++)
	{
		cout << a[i] << " ";
	}
	
   return 0;
}
```

#### 3.2 方法 1 输出结果

> 1 2 3 4 5

#### 3.3 方法 2

不要使用 `push_back()` 进行添加值，直接通过赋值的方法。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	vector<int> a;
	a.clear();
	a.resize(5);
	for(int i = 0; i < 5; i++)
	{
		a[i] = i + 1;
	}
	for(int i = 0; i < a.size(); i++)
	{
		cout << a[i] << " ";
	}
   return 0;
}
```

#### 3.4 方法 2 输出结果

> 1 2 3 4 5

### 三、总结

很小的细节，但是不注意就容易出错，要细心。
