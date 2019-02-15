---
layout: post_for_cs
title:  "Insertion, Binary Insertion, Shell's Sort"
date:   2019-02-15
excerpt: "排序——插入类算法之直接插入排序、折半插入排序、希尔排序..."
image: "/images/insertionalgorithm.jpg"
comments: true
---

## **插入类排序算法原理**

## `"InsertionAlgorithm.h"`
```c++
#pragma once
#include <iostream>
#include <string>
#include <vector>

#define TEST
#ifdef TEST
#define testcase(name) cout << name << " test passed!" << endl; 
#endif // TEST


#ifndef INSERTION_SORT
#define INSERTION_SORT
#define INSERTION_SORT_TESTCASE //Open for main()
/*
 * 插入排序在实现上，通常采用in-place排序
 * 在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。
 * 最坏时间复杂度	O(n^2)
 * 最优时间复杂度	O(n)
 * 平均时间复杂度	O(n^2)
 * 最坏空间复杂度	总共 O(n) ，需要辅助空间 O(1)
 */

int insertion_sort(int arr[], int len);

#endif // !INSERTION_SORT


#ifndef BINARY_INSERTION_SORT
#define BINARY_INSERTION_SORT
//#define BINARY_INSERTION_SORT_TESTCASE  //Open for main()
/*
 * 二分插入排序(折半插入)：采用二分查找法在比较操作上减少数目
 * 
 */

// 二分查找位置key
int binary_insertion_sort(std::vector<int> &v);

#endif // !BINARY_INSERTION_SORT


#ifndef SHELL_SORT
#define SHELL_SORT
//#define SHELL_SORT_TESTCASE  //Open for main()
/*
 * 希尔排序，也称递减增量排序算法，是插入排序的一种更高效的改进版本。希尔排序是非稳定排序算法。
 * 希尔排序是基于插入排序的以下两点性质而提出改进方法的：
 * 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率;
 * 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位;

 * 算法实现：希尔排序通过将比较的全部元素分为几个区域来提升插入排序的性能。
   这样可以让一个元素可以一次性地朝最终位置前进一大步。
   然后算法再取越来越小的步长进行排序，算法的最后一步就是普通的插入排序，
   但是到了这步，需排序的数据几乎是已排好的了（此时插入排序较快）。


   步长序列	最坏情况下复杂度
    n/2^i         O(n^2)
    2^k - 1       O(n^{3/2})
    2^i 3^j       O( n\log^2 n )
 */

int shell_sort(std::vector<int> &v);
#endif // !SHELL_SORT
```


## `"InsertionAlgorithm.cpp"`
```c++
#include "pch.h"
#include "InsertionAlgorithm.h"

using namespace std;
using std::vector;

#ifdef INSERTION_SORT

int insertion_sort(int arr[], int len)
{
	for (int i = 1; i < len; i++)
	{
		int key = arr[i];
		int j = i - 1;
		while (j >= 0 && key < arr[j])
		{
			arr[j + 1] = arr[j];
			j--;
		}
		arr[j + 1] = key;
	}
	return 0;
}

#endif // INSERTION_SORT


#ifdef BINARY_INSERTION_SORT

int binary_insertion_sort(vector<int> &v)
{
	auto begin = v.begin();

	auto key = begin + 1;
	auto end = key;
	auto mid = begin + (end - begin) / 2;

	for (auto it = v.begin() + 1; it != v.end(); it++)
	{
		while (end != mid)
		{
			if (*key < *mid)
				end = mid;
			else
				begin = mid + 1;
			mid = begin + (end - begin) / 2;
		}
		// 移动元素
		auto temp = *key;
		for (auto it = key; it != begin; it--)
		{
			*it = *(it - 1);
		}
		* mid = temp;
		//下一次循环值
		key++;
		end = key;
	}
	return 0;
}

#endif // BINARY_INSERTION_SORT


#ifdef SHELL_SORT
/*
 * 
 伪代码：
    input: an array a of length n with array elements numbered 0 to n − 1
    inc <- round(n/2)
    while inc > 0 do:    
        for i = inc .. n − 1 do:        
            temp <- a[i]        
            j <- i        
            while j ≥ inc and a[j − inc] > temp do:            
                a[j] <- a[j − inc]            
                j <- j − inc        
            a[j] <- temp    
        inc <- round(inc / 2)
 *
 */

int shell_sort(vector<int> &v)
{
	vector<int>::size_type gap = v.size() / 2;
	while (gap != 0)
	{
		for (vector<int>::size_type i = gap; i != v.size(); i++)
		{
			auto temp = v[i];
			auto j = i;
			while (j >= gap && v[j-gap] > temp)
			{
				v[j] = v[j - gap];
				j = j - gap;
			}
			v[j] = temp;
		}
		gap /= 2;	
	}
	return 0;
}
#endif // SHELL_SORT
```

## `"SortAlgorithm.cpp"(main())`
```c++
#include "pch.h"
#include "InsertionAlgorithm.h"

using namespace std;
using std::vector;

int main()
{

#ifdef INSERTION_SORT_TESTCASE
	int arr[10] = { 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
	int len = 10;
	if (!insertion_sort(arr, len))
		testcase("insertion_sort");
	for (auto it = begin(arr); it != end(arr); it++)
		cout << *it << ' ';
	cout << endl;
#endif // INSERTION_SORT_TESTCASE

#ifdef BINARY_INSERTION_SORT_TESTCASE
	vector<int> v1{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
	vector<int> &ref_v1 = v1;
	if (!binary_insertion_sort(ref_v1))
		testcase("binary_insertion_sort");
	for (auto i : v1)
		cout << i << ' ';
	cout << endl;

#endif // BINARY_INSERTION_SORT_TESTCASE

#ifdef SHELL_SORT_TESTCASE
	vector<int> v2{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
	vector<int> &v2_ref = v2;
	if (!shell_sort(v2_ref))
		testcase("shell_sort");
	for (auto i : v2)
		cout << i << ' ';
	cout << endl;
#endif // SHELL_SORT_TESTCASE

	system("pause");
	return 0;
```