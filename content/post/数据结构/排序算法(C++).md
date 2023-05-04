---
title: "排序算法"
date: 2023-04-22
tags: ["学习","C++","数据结构"]
description: ""
---

### 排序算法分类
![sort_classify](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_classify.png)

### 冒泡

![bubble](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_bubble.gif)

```C++
template <typename T, unsigned N>
void bubleSort(T (&arr)[N]) {
    bool isSwap = true;
    for(int i = N - 1; i > 0 && isSwap; --i) {
        isSwap = false;
        for(int j = 0; j < i; ++j)
            if(arr[j] > arr[j+1]){
                swap_(arr[j], arr[j+1]);
                isSwap = true;
            }
    }
}
```

#### `分析`
- 时间复杂度：
    - 最坏：数组的顺序与排序顺序完全相反，内层循环分别执行`n-1`、`n-2`、...、`1`次，总共执行`n(n-1)/2`次，时间复杂度量级为`O(n^2)`
- 空间复杂度：
    - 数组交换需要用到临时变量存储中间值，空间复杂度量级为`O(1)`
- 稳定性：
    - 冒泡排序过程整体是有序的，对于值相等的数组元素，冒泡排序不对其进行交换，所以是稳定

### 选择

![select](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_select.gif)

```C++
template <typename T, unsigned N>
void selectSort(T (&arr)[N]) {
    int temp;
    for(int i = 0; i < N - 1; ++i){
        temp = i;
        for(int j = i + 1; j < N; ++j) 
            if(arr[j] < arr[temp])
                temp = j;
        swap(arr[i], arr[temp]);
    }
}
```

#### `分析`
- 时间复杂度：
    - 最坏：数组的顺序与排序顺序完全相反，内层循环分别执行`n-1`、`n-2`、...、`1`次，总共执行`n(n-1)/2`次，时间复杂度量级为`O(n^2)`
- 空间复杂度：
    - 数组交换需要用到临时变量存储中间值，空间复杂度量级为`O(1)`
- 稳定性：
    - 在一趟选择过程中，如果一个元素比当前元素小，而元素恰好出现在一个和当前元素相等的元素后面，那么交换后相等的元素的相对顺序就相反了，所以是不稳定的

### 插入

![insert](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_insert.gif)

```C++
template <typename T, unsigned N>
void insertSort(T (&arr)[N]) {
    int temp, j;
    for (int i = 1; i < N; ++i) {
        temp = arr[i];
        for(j = i - 1; j >= 0 && arr[j] > temp; j--) 
            arr[j + 1] = arr[j];
        arr[j+1] = temp;
    }
}
```

#### `分析`
- 时间复杂度：
    - 最坏：数组的顺序与排序顺序完全相反，内层循环分别执行`n-1`、`n-2`、...、`1`次，总共执行`n(n-1)/2`次，时间复杂度量级为`O(n^2)`
- 空间复杂度：
    - 数组交换需要用到临时变量存储中间值，空间复杂度量级为`O(1)`
- 稳定性：
    - 插入排序过程整体是有序的，其相等的元素相对顺序不会改变，是稳定的

### 希尔

![shell](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_shell.gif)

```C++
template <typename T, unsigned N>
void shellSort(T (&arr)[N]) {
    int i, j, temp;
    int gap = N;
    do {
        gap = gap / 3 + 1;
        for(i = gap; i < N; i += gap) {
            temp = arr[i];
            for(j = i - gap; j >= 0 && arr[j] > temp; j -= gap) 
                arr[j + 1] = arr[j];
            arr[j+1] = temp;
        }
    } while(gap > 1);
}
```

#### `分析`
- 时间复杂度：
    - 最坏：`O(n^2)`
- 空间复杂度：
    - 数组交换需要用到临时变量存储中间值，空间复杂度量级为`O(1)`
- 稳定性：
    - 属于插入排序的改进方案，是稳定的

### 归并

![归并](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_merge.gif)

```C++
template <typename T, unsigned N>
void merge(T (&arr)[N], int left, int mid, int right) {
    int n = right - left + 1;
    T temp_arr[n];
    int i = left, j = mid + 1, index = 0;
    while(i <= mid && j <= right) 
        temp_arr[index++] = (arr[i] <= arr[j]) ? arr[i++] : arr[j++];
    while(i <= mid) temp_arr[index++] = arr[i++];
    while(j <= right) temp_arr[index++] = arr[j++];
    for (int k = 0; k < n; ++k) arr[left + k] = temp_arr[k];
}

template <typename T, unsigned N>
void merge_sort(T (&arr)[N], int left, int right) {
    if (left >= right) return;
    int mid = (left + right)/2;
    merge_sort(arr, left, mid);
    merge_sort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

template <typename T, unsigned N>
void mergeSort(T (&arr)[N]) {
    merge_sort(arr, 0, N - 1);
}
```

#### `分析`
- 时间复杂度：
    - 最坏：递归的深度为`logn`，而每一层都需要遍历所有的元素，因此时间复杂度量级为`O(logn)`
- 空间复杂度：
    - 递归深度为`logn`，需要消耗栈空间，用于临时存储所有元素的数组
- 稳定性：
    - 属于插入排序的改进方案，是稳定的

### 快速
![quick](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_quick.gif)
```C++
template <typename T, unsigned N>
T quick(T (&arr)[N], int left, int right) {
    /*mid_index取随机值
    default_random_engine dre;
    uniform_int_distribution<int> u(left, right);
    swap_(arr[left], arr[u(dre)]);
    */
    int mid_index = left;
    for (int i = left + 1; i <= right; ++i) 
        if (arr[i] < arr[left]) swap_(arr[i], arr[++mid_index]);
    swap_(arr[left], arr[mid_index]);
    return mid_index;
}

template <typename T, unsigned N>
void quick_sort(T (&arr)[N], int left, int right) {
    if (left >= right) return;
    int mid_index = quick(arr, left, right);
    quick_sort(arr, left, mid_index - 1);
    quick_sort(arr, mid_index + 1, right);
}

template <typename T, unsigned N>
void quickSort(T (&arr)[N]) {
    quick_sort(arr, 0, N - 1);
}
```

#### `分析`
- 时间复杂度：
    - 最坏：每一次选取到的枢轴元素都是最大或最小的，快速排序退化为冒泡排序
    - 最有：每一次选取到的枢轴元素都刚好位于所有元素中间，此时递归的深度为`log2n`，每一层递归都会遍历一次所有元素，因此时间复杂度的量级为`O(nlogn)`
    - 平均：`O(logn)`，最坏情况发生的概率并不高
- 空间复杂度：
    - 递归深度为`logn`，需要消耗栈空间，用于临时存储所有元素的数组，因此空间复杂度量级为`O(logn)`，但若是最坏情况，递归的深度则是`n`，对应的空间复杂度量级为`O(n)`
- 稳定性：
    - 对于数组而言，快速排序会不断交换元素，一旦中间元被交换，与其相同的中间元的相对位置可能会此改变，因此是不稳定的

### 堆

![heap](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_heap.gif)

```C++
template <typename T, unsigned N>
void max_heapify(T (&arr)[N], int start, int end) {
    int current = start;
    int child = (current<<1)+1;
    while (child <= end) {
        if(child < end && arr[child] < arr[child+1]) ++child;
        if(arr[current] > arr[child]){
            return;
        } else{
            swap_(arr[current], arr[child]);
            current = child;
            child = (current<<1)+1;
        }
    }
}

template <typename T, unsigned N>
void heapSort(T (&arr)[N]) {
    int i;
    for(i = (N>>1)-1; i >= 0; --i) max_heapify(arr, i, N-1);
    for(i = N-1; i > 0; --i) {
        swap_(arr[0], arr[i]);
        max_heapify(arr, 0, i-1);
    }
}
```

#### `分析`
- 时间复杂度：
    - 最坏：初始将待排元素看成一个完全二叉树，并不需要任何代价，而将万元二叉树转化为大/小顶堆，其深度为`log2n`，当所有节点元素都需要被移动时，时间复杂度量级为`O(nlogn)`，此外，虽然堆排序与快速排序的时间复杂度量级一致，但是其的时间常数要更大，因此排序耗时会更慢，堆排序难以并行
- 空间复杂度：
    - 不需要额外的存储空间，空间复杂度量级为`O(1)`
- 稳定性：
    - 对构建的过程中，会交换不同的节点，此操作会改变相同值的元素的相对顺序，比如一个所有元素都相等的数组，在构建堆的第一次迭代中完全二叉树的根节点被放到了数组末尾，此时数组的稳定性已经消失了，因此堆排序是不稳定的


### 计数

![count](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_count.gif)

```C++
template <typename T, unsigned N>
void countSort(T (&arr)[N]) {//不支持负数排序
    int max = arr[0], min = arr[0], i;
    for(i = 1; i < N; ++i) {
        if(max < arr[i]) max = arr[i];
        if(min > arr[i]) min = arr[i];
    }
    int length = max-min+1;
    T count[length];
    fill(count, count + length, 0);
    for(i = 0; i < N; ++i) ++count[arr[i]];
    for(int k = 0, i = 0; k < length; ++k) 
        while(count[k]){
            arr[i++] = k;
            --count[k];
        }
}
```

#### `分析`
- 时间复杂度：
    - 最坏：使用额外的空间存储所有的数组元素，当前数组以及用于暂存当前数组的结构都需要被遍历一次，因此时间复杂度量级为`O(n+k)`
- 空间复杂度：
    - `O(n+k)`
- 稳定性：
    - 稳定

### 桶

![bucket](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_bucket.png)

```C++
template <typename T, unsigned N>
void bucketSort(T (&arr)[N]) {
    int max = arr[0], min = arr[0], i, j, index;
    for(i = 1; i < N; ++i) {
        if(max < arr[i]) max = arr[i];
        if(min > arr[i]) min = arr[i];
    }
    int gap = (max-min)/N+1;
    int bucket_num = (max-min)/gap+1;
    vector<T> bucket[bucket_num];
    for(i = 0; i < N; ++i) 
        bucket[(arr[i]-min)/gap].push_back(arr[i]);
    for(i = 0; i < bucket_num; ++i) 
        if(bucket[i].size() > 0) 
            sort(bucket[i].begin(), bucket[i].end());
    for(i = 0; i < bucket_num; ++i) 
        for(j = 0; j < bucket[i].size(); ++j) 
            arr[index++] = bucket[i][j];
}
```

#### `分析`
- 时间复杂度：
    - 最坏：桶排序是计数排序的升级版，将原始数组的元素分到每一个范围不同的桶中，当原始数组中大多数元素大小集中在一个区间，这是会导致其中一个桶的大小接近数组大小，此时桶排序退化为一般排序，其时间复杂度取决于桶内元素的排序方法，最坏可能时间复杂度量级可能是`O(n^2)`
    - 最好：每个桶一个元素，此时完成分桶操作即完成了排序，因此时间复杂度量级为`O(n)`
    - 平均：`O(n+k)`
- 空间复杂度：
    - `O(n+k)`
- 稳定性：
    - 稳定


### 基数

![radix](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_radix.gif)

```C++
template <typename T, unsigned N>
void radixSort(T (&arr)[N]) {//不支持负数排序
    vector<T> base_[10];
    int i, j, arr_index, base_till;
    int n = 1 + (int)log10(*max_element(arr, arr + N));
    for(i = 0; i < n; ++i) {
        for(j = 0; j < N; ++j) 
            base_[(arr[j]/(int)pow(10, i))%10].push_back(arr[j]);
        arr_index = 0;        
        for(j = 0; j < 10; ++j) {
            sort(base_[j].rbegin(), base_[j].rend());
            base_till = base_[j].size() - 1;
            while(base_[j].size() > 0) {
                arr[arr_index++] = base_[j][base_till--];
                base_[j].pop_back();
            }
        }
    }
}
```

#### `分析`
- 时间复杂度：
    - 最坏：计数排序时间复杂度跟待排序元素的最大位数`k`有关，每有1位，就需要多遍历一次数组，因此时间复杂度量级为`O(n*k)`
- 空间复杂度：
    - 需要使用大小为的`k`二维数组或容器来临时存储数组元素，因此空间复杂度量级为`O(n+k)`
- 稳定性：
    - 稳定


### `通用接口函数以及依赖项`
```C++
#include <iostream>
#include <random>
#include <vector>
#include <algorithm>
using namespace std;

//打印数组
template <typename T, unsigned N>
void printArr(T (&arr)[N]) {
    for(int i = 0; i < N; ++i) {
        cout<<arr[i]<<"\t";
        if (i % 10 == 9)
            cout<<endl;
    }
}

//将size为N的数组用随机数初始化
template <unsigned N>
void randomIntArr(int (&arr)[N], int rangeL, int rangeR) {
    default_random_engine dre;
    uniform_int_distribution<int> u(rangeL, rangeR);
    for (int i = 0; i < N; ++i) {
        arr[i] = u(dre);
    }
}

//交换数组成员
template <typename T>
void swap_(T &a, T &b) {
    int temp = a;
    a = b;
    b = temp;
}

```



### 总结
![sort_classify](https://gitlab.com/DJVQ/image/-/raw/master/blog/sort_analysis.png)