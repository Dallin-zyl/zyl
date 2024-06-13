---
layout: post
title: 函数与指针
date: 2023-07-03
author: Zhaoyl
tags: [procedure]
comments: true
toc: true
---
## 函数指针

首先，函数名就表示函数的地址。&函数名=函数名=函数的地址。

```c
#include <stdio.h>
int Add(int x, int y)
{
	return x + y;
}
int main()
{
	printf("%p\n", &Add); // 00B313D4
	printf("%p\n", Add);  // 00B313D4
	// &Add=Add，表示Add函数的地址
	return 0;
}
```

函数指针是指向函数的指针。

```c
#include <stdio.h>
int Add(int x, int y)
{
	return x + y;
}
 
int main()
{
	// 函数指针变量pf保存了Add函数的地址，变量类型为int (*)(int, int)
	int (*pf)(int x, int y) = &Add;
    /*
	int (*pf)(int x, int y) = Add; // Add=&Add
	int (*pf)(int, int) = &Add;    // 形参可以省略
	int (*pf)(int, int) = Add;     // Add=&Add，形参可以省略
    */
 
	// 调用Add函数
	int sum = (*pf)(3, 5);
    /*
	int sum = pf(3, 5); // pf(3, 5) = (*pf)(3, 5)
	int sum = Add(3, 5);
    */
 
	printf("%d\n", sum);
	return 0;
}
```



代码1：

```c
(*(void(*)())0)();
```

void(*)()是一个函数指针类型，指向的函数没有参数，返回类型为void。

(void(\*)())0表示把0强制类型转换为void(*)()类型，把0当做一个函数的地址。

(\*(void(*)())0)()表示调用0地址处的函数。

代码2：

```c
void(*signal(int, void(*)(int)))(int);
```

这是函数声明，声明的函数是signal。

signal(int, void(\*)(int))表示signal函数的第一个参数是int类型，第二个参数是void(*)(int)类型，即一个函数指针类型，该函数指针指向的函数参数是int，返回类型是void。

void(\*signal(int, void(\*)(int)))(int)表示signal函数的返回类型是void(*)(int)类型，即一个函数指针类型，该函数指针指向的函数参数是int，返回类型是void。
简化代码2：

```c
void(*signal(int, void(*)(int)))(int);
typedef void(*pf_t)(int); // 将void(*)(int)类型重命名为pf_t类型
pf_t signal(int, pf_t);
```

## 函数指针数组

```c
#include <stdio.h>
 
int Add(int x, int y)
{
	return x + y;
}
 
int Sub(int x, int y)
{
	return x - y;
}
 
int main()
{
	int (*pfArr[2])(int, int) = { Add,Sub }; // 函数指针数组
	
	int ret = pfArr[0](2, 3); // Add(2, 3)
	printf("%d\n", ret);      // 5
	
	ret = pfArr[1](2, 3); // Sub(2, 3)
	printf("%d\n", ret);  // -1
 
	return 0;
}
```

实现两个整数的加减乘除计算器：

使用switch语句：

```c
#include <stdio.h>
int Add(int x, int y)
{
	return x + y;
}
int Sub(int x, int y)
{
	return x - y;
}
int Mul(int x, int y)
{
	return x * y;
}
int Div(int x, int y)
{
	return x / y;
}
void menu()
{
	printf("***************************\n");
	printf("***** 1. add   2. sub *****\n");
	printf("***** 3. mul   4. div *****\n");
	printf("***** 0. exit          ****\n");
	printf("***************************\n");
}
 
int main()
{
	int input = 0;
	int x = 0;
	int y = 0;
	int ret = 0;
	do
	{
		menu();
		printf("请选择:>");
		scanf("%d", &input);
		switch (input)
		{
		case 1:
			printf("请输入2个操作数:>");
			scanf("%d %d", &x, &y);
			ret = Add(x, y);
			printf("%d\n", ret);
			break;
		case 2:	
			printf("请输入2个操作数:>");
			scanf("%d %d", &x, &y);
			ret = Sub(x, y);
			printf("%d\n", ret);
			break;
		case 3:
			printf("请输入2个操作数:>");
			scanf("%d %d", &x, &y);
			ret = Mul(x, y);
			printf("%d\n", ret);
			break;
		case 4:
			printf("请输入2个操作数:>");
			scanf("%d %d", &x, &y);
			ret = Div(x, y);
			printf("%d\n", ret);
			break;
		case 0:
			printf("退出计算器\n");
			break;
		default:
			printf("选择错误\n");
			break;
		}
	} while (input);
	return 0;
}
```

使用函数指针数组：

```c
#include <stdio.h>
int Add(int x, int y)
{
	return x + y;
}
int Sub(int x, int y)
{
	return x - y;
}
int Mul(int x, int y)
{
	return x * y;
}
int Div(int x, int y)
{
	return x / y;
}
void menu()
{
	printf("***************************\n");
	printf("***** 1. add   2. sub *****\n");
	printf("***** 3. mul   4. div *****\n");
	printf("***** 0. exit          ****\n");
	printf("***************************\n");
}
 
int main()
{
	int input = 0;
	int x = 0;
	int y = 0;
	int ret = 0;
	int (*pfArr[])(int, int) = { 0,Add,Sub,Mul,Div };
	do
	{
		menu();
		printf("请选择:>");
		scanf("%d", &input);
		if (input == 0)
		{
			printf("退出计算器\n");
			break;
		}
		if (input >= 1 && input <= 4)
		{
			printf("请输入2个操作数:>");
			scanf("%d %d", &x, &y);
			ret = pfArr[input](x, y);
			printf("%d\n", ret);
		}
		else
		{
			printf("选择错误\n");
		}	
	} while (input);
	return 0;
}
```

## 指向函数指针数组的指针

```c
int (*pf)(int, int) = &Add;              // 函数指针
int (*pfArr[2])(int, int) = { Add,Sub }; // 函数指针数组
int (*(*ppfArr)[2])(int, int) = &pfArr;  // 指向函数指针数组的指针
```

总结：

```c
int *p[10];                // 指针数组：数组大小是10，数组元素是int*类型的指针
int (*p)[10];              // 数组指针：指针指向一个数组大小是10，数组元素是int类型的数组
int *p(int);               // 函数声明：函数名是p，参数是int类型，返回值是int*类型
int (*p)(int);             // 函数指针：指针指向一个参数是int类型，返回值是int类型的函数
int (*p[10])(int);         // 函数指针数组：数组大小是10，数组元素是int(*)(int)类型的数组
int (*(*p)[10])(int, int); // 指向函数指针数组的指针：指针指向一个数组大小是10，数组元素是int(*)(int)类型的数组
```

## 回调函数

回调函数就是一个被作为参数传递的函数。在C语言中，回调函数只能使用函数指针实现。

qsort函数

```c
#include <stdlib.h>
void qsort(void* base, size_t num, size_t size, int (*compar)(const void*, const void*));
// 执行快速排序
// base      待排数据的起始地址
// num       待排数据的元素个数
// size      待排数据的元素大小（单位：字节）
// compar    函数指针，指向比较两个元素的函数
 
// 比较函数需要自己编写，规定函数原型为：
// int compar(const void* elem1, const void* elem2)
// 函数返回值的规则如下：
// 当进行升序排序时，
// 如果elem1<elem2，则返回值<0
// 如果elem1=elem2，则返回值=0
// 如果elem1>elem2，则返回值>0
```

使用qsort函数排序整型数据

```c
#include <stdio.h>
#include <stdlib.h>
 
int cmp_int(const void* e1, const void* e2)
{
	return (*(int*)e1 - *(int*)e2); // void*类型的变量必须强制类型转换成其他类型才能解引用
}
 
void print(int arr[], int n)
{
	for (int i = 0; i < n; i++)
	{
		printf("%d ", arr[i]);
	}
	printf("\n");
}
 
int main()
{
	int arr[] = { 2,1,3,7,5,9,6,8,0,4 };
	int n = sizeof(arr) / sizeof(arr[0]);
	qsort(arr, n, sizeof(arr[0]), cmp_int);
	print(arr, n);  // 0 1 2 3 4 5 6 7 8 9
	return 0;
}
```

qsort函数排序结构体类型数据

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
struct Stu
{
	char name[20];
	int age;
};
 
int cmp_stu_by_name(const void* e1, const void* e2)
{
	return strcmp(((struct Stu*)e1)->name, ((struct Stu*)e2)->name);
}
 
/*
int cmp_stu_by_age(const void* e1, const void* e2)
{
    return ((struct Stu*)e1)->age - ((struct Stu*)e2)->age;
}
*/
 
int main()
{
	struct Stu s[] = { {"zhangsan",20}, {"lisi",55}, {"wangwu",40} };
	int n = sizeof(s) / sizeof(s[0]);
	// 按照名字排序
	qsort(s, n, sizeof(s[0]), cmp_stu_by_name);
	// 按照年龄排序
	// qsort(s, n, sizeof(s[0]), cmp_stu_by_age);
	printf("%s %d\n", s[0].name, s[0].age);
	printf("%s %d\n", s[1].name, s[1].age);
	printf("%s %d\n", s[2].name, s[2].age);
	return 0;
}
```

借鉴qsort的设计思想，改写冒泡排序函数，实现对任意类型的数据的排序。

```c
#include <stdio.h>
 //整型
int cmp_int(const void* e1, const void* e2)
{
	return (*(int*)e1 - *(int*)e2); // void*类型的变量必须强制类型转换成其他类型才能解引用
}
/*  begin  *///结构体类型
struct Stu
{
	char name[20];
	int age;
};
 
int cmp_stu_by_name(const void* e1, const void* e2)
{
	return strcmp(((struct Stu*)e1)->name, ((struct Stu*)e2)->name);
}
/* end  */
void swap(char* buf1, char* buf2, int size)
{
	for (int i = 0; i < size; i++)
	{
		char tmp = *buf1;
		*buf1 = *buf2;
		*buf2 = tmp;
		buf1++;
		buf2++;
	}
}
 
void bubble_sort2(void* base, int num, int size, int (*cmp)(const void*, const void*))
{
	// 趟数
	for (int i = 0; i < num - 1; i++)
	{
		// 一趟冒泡排序
		for (int j = 0; j < num - 1 - i; j++)
		{
			if (cmp((char*)base + j * size, (char*)base + (j + 1) * size) > 0)
			{
				// 交换
				swap((char*)base + j * size, (char*)base + (j + 1) * size, size);
			}
		}
	}
}
 
void print(int arr[], int n)
{
	for (int i = 0; i < n; i++)
	{
		printf("%d ", arr[i]);
	}
	printf("\n");
}
 
int main()
{
	int arr[] = { 2,1,3,7,5,9,6,8,0,4 };
	int n = sizeof(arr) / sizeof(arr[0]);
	bubble_sort2(arr, n, sizeof(arr[0]), cmp_int);
	print(arr, n); // 0 1 2 3 4 5 6 7 8 9
    /* 结构体类型的使用
    struct Stu s[] = { {"zhangsan", 20}, {"lisi", 55}, {"wangwu", 40} };
	int n = sizeof(s) / sizeof(s[0]);
	// 按照名字排序
	bubble_sort2(s, n, sizeof(s[0]), cmp_stu_by_name);
	// 按照年龄排序
	// bubble_sort2(s, n, sizeof(s[0]), cmp_stu_by_age);
	printf("%s %d\n", s[0].name, s[0].age);
	printf("%s %d\n", s[1].name, s[1].age);
	printf("%s %d\n", s[2].name, s[2].age);
	*/
	return 0;
}
```

