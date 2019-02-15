---
layout: post_for_cs
title:  "Bubble Sort, Quicksort"
date:   2019-02-15
excerpt: "排序——交换类算法之冒泡排序、快速排序..."
image: "/images/exchangesort.jpg"
comments: true
---

## **交换类排序算法原理**

## `"ExchangeSort.h"`
```c++
#pragma once

#include <iostream>
#include <vector>

#ifndef BUBBLE_SORT
#define BUBBLE_SORT 
#define BUBBLE_SORT_TESTCASE  //Open for main()

/*
 * 冒泡排序算法的运作如下：

比较相邻的元素。如果第一个比第二个大，就交换他们两个。
对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
针对所有的元素重复以上的步骤，除了最后一个。
持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
 * 
 *
  最坏时间复杂度	O(n^{2})
  最优时间复杂度	O(n)
  平均时间复杂度	O(n^{2})
  最坏空间复杂度	总共O(n)，需要辅助空间O(1)
 */

int bubble_sort(std::vector<int> &v);

#endif // !BUBBLE_SORT

#ifndef QUICK_SORT
#define QUICK_SORT
#define QUICK_SORT_TESTCASE

/* 
  快速排序（英语：Quicksort），又称划分交换排序（partition-exchange sort），简称快排，一种排序算法，最早由东尼·霍尔提出。在平均状况下，
   排序 n个项目要  O(nlogn) （大O符号）次比较。在最坏状况下则需要 O(n^{2})次比较，但这种状况并不常见。
   事实上，快速排序(nlogn)通常明显比其他算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地达成。
 *
  快速排序使用分治法（Divide and conquer）策略来把一个序列（list）分为两个子序列（sub-lists）。
  步骤为：
  从数列中挑出一个元素，称为“基准”（pivot），
  重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（相同的数可以到任何一边）。在这个分割结束之后，该基准就处于数列的中间位置。这个称为分割（partition）操作。
  递归地（recursively）把小于基准值元素的子数列和大于基准值元素的子数列排序。
 *
 */

int quick_sort_recursive(std::vector<int> &v, std::vector<int>::iterator begin, std::vector<int>::iterator tail);
#endif // !QUICK_SORT

```

## `"ExchangeSort.cpp"`
```c++
#include "pch.h"
#include "ExchangeSort.h"

using namespace std;
using std::vector;

#ifdef BUBBLE_SORT

int bubble_sort(vector<int> &v)
{
	for (vector<int>::size_type i = 0; i != v.size() - 1; i++)
	{
		for (auto it = v.begin(); it != v.end() - i - 1; it++) //TYPE:vector<int>::iterator
		{
			if (*it > *(it + 1))
				swap(*it, *(it + 1));
		}
	}
	return 0;
}
#endif // BUBBLE_SORT


#ifdef QUICK_SORT
//递归版本
int quick_sort_recursive(vector<int> &v, vector<int>::iterator begin, vector<int>::iterator tail)
{
	if (begin >= tail)
		return 0;
	auto pivot = tail; //default the last one is pivot
	auto left = begin, right = tail - 1;
	while (left < right)
	{
		while (*left < *pivot && left < right)
			left++;
		while (*right >= *pivot && left < right)
			right--;
		swap(*left, *right);
	}
	if (*left >= *pivot)
		swap(*left, *pivot);
	else
		left++;
	if (begin != left)
		quick_sort_recursive(v, begin, left - 1);  // Trouble Case: when left-1 < begin that out of vector's range.
	if (tail != left)
		quick_sort_recursive(v, left + 1, tail);
	if (begin == left || tail == left)
		return 0;
}
#endif // QUICK_SORT

```


## `"SortAlgorithm.cpp"(main())`
```c++
#include "pch.h"
#include "ExchangeSort.h"

using namespace std;
using std::vector;

int main()
{
#ifdef BUBBLE_SORT_TESTCASE
	vector<int> v3{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
	vector<int> &v3_ref = v3;
	if (!bubble_sort(v3_ref))
		testcase("bubble_sort");
	for (auto i : v3)
		cout << i << ' ';
	cout << endl;
#endif // BUBBLE_SORT_TESTCASE

#ifdef QUICK_SORT_TESTCASE
	vector<int> v4{ 5, 6, 9, 8, 7, 4, 3, 2, 1, 0 };
	vector<int> &v4_ref = v4;
	vector<int>::iterator begin = v4_ref.begin();
	vector<int>::iterator tail = v4_ref.end() - 1;
	if (!quick_sort_recursive(v4_ref, begin, tail))
		testcase("quick_sort_recursive");
	for (auto i : v4)
		cout << i << ' ';
	cout << endl;
	
#endif // QUICK_SORT_TESTCASE

	system("pause");
	return 0;
```
