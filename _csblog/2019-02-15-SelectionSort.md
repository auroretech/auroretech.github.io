---
layout: post_for_cs
title:  "Simple Selection sort, Heapsort"
date:   2019-02-15
excerpt: "排序——选择类算法之简单选择排序、堆排序..."
image: "/images/selectionsort.jpg"
comments: true
---

## **选择类排序算法原理**

## `"SelectionSort.h"`

```
#pragma once

#include <iostream>
#include <vector>

#ifndef SIMPLE_SELECTION_SORT
#define SIMPLE_SELECTION_SORT
#define SIMPLE_SELECTION_SORT_TESTCASE

/*
 * 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
   然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
 * 最坏时间复杂度	О(n²)
   最优时间复杂度	О(n²)
   平均时间复杂度	О(n²)
   最坏空间复杂度	О(n) total, O(1) auxiliary
 */

int simple_selection_sort(std::vector<int> &v);
#endif // !SIMPLE_SELECTION_SORT


#ifndef HEAP_SORT
#define HEAP_SORT

#define MAX_HEAP
#ifdef MAX_HEAP
#define MAX_HEAP_SORT_TESTCASE
#endif // MAX_HEAP

#define PRIORITY_QUEUE
#ifdef PRIORITY_QUEUE
#define PRIORITY_QUEUE_TESTCASE
#endif // PRIORITY_QUEUE

#define LEFT(i) ((i) << 1) + 1
#define RIGHT(i) ((i) << 1) + 2
#define PARENT(i) (i - 1) >> 1 // not used now

/*
 * 堆排序（英语：Heapsort）是指利用堆这种数据结构所设计的一种排序算法。
   堆是一个近似完全二叉树的结构，并同时满足堆积的性质：即子节点的键值或索引总是小于（或者大于）它的父节点。
 * 通常堆是通过一维数组来实现的。在数组起始位置为0的情形中：
   父节点i的左子节点在位置 2i+1;
   父节点i的右子节点在位置(2i+2);
   子节点i的父节点在位置 floor((i-1)/2);
 *
  最坏时间复杂度	O(n\log n)
  最优时间复杂度	O(n\log n)
  平均时间复杂度	\Theta(n\log n)
  最坏空间复杂度	O(n) total, O(1) auxiliary

 *在堆的数据结构中，堆中的最大值总是位于根节点（在优先队列中使用堆的话堆中的最小值位于根节点）。
  堆中定义以下几种操作：
  最大堆调整（Max Heapify）：将堆的末端子节点作调整，使得子节点永远小于父节点
  创建最大堆（Build Max Heap）：将堆中的所有数据重新排序
  堆排序（HeapSort）：移除位在第一个数据的根节点，并做最大堆调整的递归运算
 */

/*
 * 维护堆的性质：MAX-HEAPIFY (最大堆)
 */
int max_heapify(std::vector<int> &v, size_t i);

/*
 * 建堆:BUILD-MAX-HEAP
 */
int build_max_heap(std::vector<int> &);

/*
 * 堆排序：HEAPSORT
 */
int max_heap_sort(std::vector<int> &);

#endif // !HEAP_SORT
```

## `"SelectionSort.cpp"`

```c++
#include "pch.h"
#include "SelectionSort.h"

using namespace std;
using std::vector;

#ifdef SIMPLE_SELECTION_SORT

int simple_selection_sort(std::vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		auto min = it;
		for (auto it2 = it + 1; it2 != v.end(); it2++)
		{
			if (*it2 < *min)
				min = it2;
		}
		swap(*it, *min);
	}
	return 0;
}
#endif // SIMPLE_SELECTION_SORT


#ifdef HEAP_SORT

#ifdef MAX_HEAP

// @step1
int max_heapify(vector<int> &v, size_t i) //T = O(logn)
{
	auto lc = LEFT(i); //左孩子下标(以0开头)
	auto rc = RIGHT(i); //右孩子
	size_t largest = 0;
	if (lc <= v.size() - 1 && v[lc] > v[i])
		largest = lc; //结点i,其左孩子lc,右孩子rc。局部调整符合大堆
	else
		largest = i;
	if (rc <= v.size() - 1 && v[rc] > v[largest])
		largest = rc;
	if (largest != i) // 当前最大堆的调整可能会影响其子结点，故递归修正
	{
		swap(v[i], v[largest]);
		max_heapify(v, largest);
	}
	else
		return 0;
}

// @step2
int build_max_heap(vector<int> &v) // T = O(n)
{
	// the bottom index of the tree-like structure
	for (size_t i = (v.size() >> 1) - 1; i != -1; i--) //i is not a negative one forever; so out of index happened
		max_heapify(v, i); //从近似完全二叉树的最后包含叶结点的根结点开始，逆序递归调整为大顶堆
	return 0;
}

// @step3
int max_heap_sort(vector<int> &v)
{
	build_max_heap(v);
	for (size_t i = v.size() - 1; i != 0; i--) // i stops 1 not 0!!!
	{
	//建堆后，从最后结点开始，依次交换最大最小值，且输出最大值同时重新建堆
		swap(v[0], v[i]);
		cout << v[i] << ' ';
		v.pop_back();
		max_heapify(v, 0);
	}
	cout << v[0] << endl;
	return 0;
}
#endif // MAX_HEAP


#ifdef PRIORITY_QUEUE
/*
 * 基于堆排序的优先队列算法
 * 用来维护由一组元素构成的集合S的数据结构，每个元素都有一个相关的值，称key。
 * 最大优先队列支持操作：
 * INSERT(S, x): S = S U {x};
 * MAXIMUM(S): 返回S中具有最大关键字的元素
 * EXTRACT-MAX(S): 去掉并返回S中的具有最大关键字的元素
 * INCREASE-KEYS(S, x, k): 将元素x的关键字值增加到k
 */
#endif // PRIORITY_QUEUE

#endif // HEAP_SORT
```

## `"SortAlgorithm.cpp"(main())`
```c++
#include "pch.h"
#include "ExchangeSort.h"

using namespace std;
using std::vector;

int main()
{
#ifdef SIMPLE_SELECTION_SORT_TESTCASE
	vector<int> v5{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
	vector<int> &v5_ref = v5;
	if (!simple_selection_sort(v5_ref))
		testcase("simple_selection_sort");
	for (auto i : v5)
		cout << i << ' ';
	cout << endl;
#endif // SIMPLE_SELECTION_SORT_TESTCASE

#ifdef MAX_HEAP_SORT_TESTCASE
	vector<int> v6 { 9, 5, 2, 1, 3, 6, 7, 8, 4, 0, 15, 98, 111, 100000, -1 };
	vector<int> &v6_ref = v6;
	max_heap_sort(v6_ref);
	
#endif // MAX_HEAP_SORT_TESTCASE

#ifdef PRIORITY_QUEUE_TESTCASE
	// pass
#endif // PRIORITY_QUEUE_TESTCASE

	system("pause");
	return 0;
}
```
