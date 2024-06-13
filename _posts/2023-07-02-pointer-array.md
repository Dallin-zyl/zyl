---
layout: post
title: 指针数组和数组指针
date: 2023-07-02
author: Zhaoyl
tags: [procedure]
comments: true
toc: true
---
指针数组是存放指针的数组，数组指针是指向数组的指针。

## 数组指针与指针数组

这两个概念在词语上很相近，但事实上是两个完全不同的东西

举个例子说明，如下：

int * a[5] 和 int (*a)[5]

可以看出的是，他们只相差一个括号，由于存在优先级的关系  ( ) > [ ] > \*  ，如果不加括号，a会先与[ ]结合，加了括号，a则先与*结合。加括号为的是改变运算的结合顺序。

int * a[5]没有括号，a先与[ ]结合，先是个数组；

int (\*a)[5]加了括号，a先和*结合，先是个指针。

故：

int * a[5]是指针数组，本质上是一个数组，数组里存放的元素是指针，指针的类型是int *型，指向一个整型数。

int (*a)[5]是数组指针，本质上是一个指针，指针所指对象是一个数组，数组的类型是int [5]型。

```c
int* arr1[10];    // 存放一级整型指针的一维数组
char* arr2[4];    // 存放一级字符指针的一维数组
char** arr3[5];   // 存放二级字符指针的一维数组
int** arr4[3][4]; // 存放二级整型指针的二维数组
```

```c
int* p1[10];  // 指针数组
int(*p2)[10]; // 数组指针
```

## 数组指针的用法

数组指针与指针数组两个用的比较多的还是数组指针，并且多用于处理二维数组。接下来详细介绍数组指针的用法。

### **数组指针与一维数组**

```c
int a[5]={1,2,3,4,5};
int (*p)[5]=NULL;
p=&a;
```

***数组名表示数组首元素的地址***，在给数组指针赋值时，取地址&后代表了整个数组的地址，赋给了数组指针p，\*p相当于数组名。*此处数组指针是为了方便理解，一般是不这样用的哈哈哈哈*。

```c
int i;
for(i=0;i<5;i++)
{
    printf("%d ",*(*p+i));//遍历输出数组的值
}
```

```c
#include <stdio.h>
int main()
{
	int arr[5] = { 0 };
	int(*p)[5] = &arr; // p保存的是整个数组的地址
 
	printf("%d\n", sizeof(*p));     // *p是整个数组，大小为5×4=20个字节
	printf("%d\n", sizeof(*p + 0)); // *p是数组首元素的地址，大小为4/8个字节（32/64位机器）
 
	printf("%p\n", &(*p));     // 010FFAA8 *p是整个数组，&(*p)是整个数组的地址
	printf("%p\n", &(*p) + 1); // 010FFABC &(*p)是整个数组的地址，&(*p)+1跳过整个数组（20个字节）
	printf("%p\n", *p + 1);    // 010FFAAC *p是数组首元素的地址，*p+1跳过4个字节，是数组第二个元素的地址
 
	return 0;
}
```

### 数组指针与二维数组

```c
int a[2][3]={1,2,3,4,5,6};
int (*p)[3]=NULL;
p=a;
```

值得注意的是，int （*p）[3]中的3，此处应该写上的是二维数组的列数，亦或者是每个一维数组的元素个数。

另外，通过比较一维数组和二维数组给p赋值，不难发现，两者有差别，因为二维数组的数组名a的类型就是int (*)[N]，类型相同，所以可以直接赋值。

```c
int i,j;
for(i=0;i<2;i++)
{
    for(j=0;j<3;j++)
        {
            printf("%d ",*(*(p+i)+j));//遍历输出
        }
}
```

对于\*(*(p+i)+j) ，一个通俗的理解，里层 * 是为了先选定数组，取数组，然后外层 * 再对数组中的元素取值，i，j分别是两次取值的偏移量。我们说对数组指针对二维数组才有意义，其意义主要体现在里层的p+i上，可以通过移动p，访问连续的不同一维数组。

## 实际运用

输入字符串，并进行排序输出

```c
#include<stdio.h>
#include<string.h>
#define M 5   
#define N 20
void sort(char (*p)[N]);
int main()
{
    char a[M][N]={0}; 
    int i;
    printf("please input %d character strings:\n",M);
    for(i=0;i<M;i++)
    {
        scanf("%s",a[i]);//此处不能写成*a++，因为a是一个常量，不是变量，不能移动处理
    }
    sort(a);
    printf("after sorting,the sequence is :\n");
    for(i=0;i<M;i++)
    {
        printf("%s\n",a[i]);//此处也同上，不能使用*a++，一个道理
    }
    return 0;
}
void sort(char (*p)[N])
{
    int i,j;
    char temp[N]={0};
    for(i=0;i<M-1;i++)    //选择排序法
    {
        for(j=i+1;j<M;j++)
        {
            if(strcmp(p[i],p[j])>0)
            {
                strcpy(temp,p[i]);//因strcpy函数的传参类型限制，此处只能写p[i],不能是p+i
                strcpy(p[i],p[j]);//p[i]的类型是char *，而p+i的类型是char (*p)[N]
                strcpy(p[j],temp);//不管何时，一定一定注意数据类型的匹配
            }
        }
    }
}
```

## 数组参数和指针参数

### 实参是一维数组的数组名时的形参

```c
void test1(int arr[10], int n)   // ok 形参是一维数组
{}
void test1(int arr[], int n)     // ok 形参是一维数组，数组大小可以省略
{}
void test1(int* arr, int n)      // ok 形参是一级指针
{}
 
void test2(int* arr2[20], int n) // ok 形参是一维指针数组
{}
void test2(int* arr2[], int n)   // ok 形参是一维指针数组，数组大小可以省略
{}
void test2(int** arr2, int n)    // ok 形参是二级指针
{}
 
int main()
{
	int arr1[10] = { 0 };  // 一维数组
	int n1 = sizeof(arr1) / sizeof(arr1[0]);
 
	int* arr2[20] = { 0 }; // 一维指针数组
	int n2 = sizeof(arr2) / sizeof(arr2[0]);
 
	test1(arr1, n1);
	test2(arr2, n2);
 
	return 0;
}
```

### 实参是二维数组的数组名时的形参

```c
void test(int arr[3][5], int n) // ok  形参是二维数组
{}
void test(int arr[][5], int n)  // ok  形参是二维数组，行数可以省略
{}
void test(int arr[3][], int n)  // err 形参是二维数组，列数不可以省略
{}
void test(int arr[][], int n)   // err 形参是二维数组，列数不可以省略
{}
 
void test(int(*arr)[5], int n)  // ok  形参是数组指针，指向二维数组的首元素（首行），即一个大小为5的一维数组
{}
void test(int* arr, int n)      // err 形参不可以是一级指针
{}
void test(int* arr[5], int n)   // err 形参不可以是一级指针数组
{}
void test(int** arr, int n)     // err 形参不可以是二级指针
{}
 
int main()
{
	int arr[3][5] = { 0 }; // 二维数组
	int n = sizeof(arr) / sizeof(arr[0]);
	test(arr, n);
	return 0;
}
```

### 形参是一级指针时的实参

```c
void test(int* p)        // 形参是一级整型指针
{}
void test(int* p, int n) // 形参是一级整型指针
{} 
int main()
{
	int a = 0;
	test(&a);     // ok 实参是整型变量地址
 
	int* p = &a;
	test(p);      // ok 实参是一级整型指针
 
	int arr[10] = { 0 };
	int n = sizeof(arr) / sizeof(arr[0]);
	test(arr, n); // ok 实参是一维整型数组的数组名
 
	return 0;
}
```

### 形参是二级指针时的实参

```c
void test(int** p)       // 形参是二级整型指针
{}
void test(int** p,int n) // 形参是二级整型指针
{}
 
int main()
{
	int a = 0;
 
	int* pa = &a;
	test(&pa);    // ok 实参是一级整型指针地址
 
	int** ppa = &pa;
	test(ppa);    // ok 实参是二级整型指针
 
	int* arr[10] = { 0 };
	int n = sizeof(arr) / sizeof(arr[0]);
	test(arr, n); // ok 实参是一维整型指针数组的数组名
 
	return 0;
}
```





