+++
title = "Partition Methods of Quicksort"
author = ["Sduby"]
date = 2022-07-11T04:42:00+08:00
tags = ["algorithm", "sort"]
draft = false
summary = "Generally speaking, Quicksort can use two types of partition methods. The first of which partitions the array into 3 parts: =(<=pivot), (pivot), (>pivot)=. And the latter partitions the array into 2 parts. I will introduce two coresponding algorithms: Hoare's and Lomuto partition algorithm."
+++

Generally speaking, Quicksort can use two types of partition methods. The first of which partitions the array into 3 parts: `(<=pivot), (pivot), (>pivot)`. And the latter partitions the array into 2 parts. I will introduce two coresponding algorithms: Hoare's and Lomuto partition algorithm.


## Lomuto Partition Algorithm {#lomuto-partition-algorithm}

Lomuto Partition Algorithm is very likely to be the partition algorithm that you learn from books. It partitions the array into three parts:

-   First subset: **less or equal than** `pivot`
-   Second subset: One element, **equals** `pivot`
-   Third subset: **greater than** `pivot`

The special thing is that after you've done the partitioning, the single element in the second subset (the pivot) is guaranteed to be located at the correct order position. (_Why?_)

```c++
int partition(int* vec, int l, int r) {
    int k = l+1;
    int pivot_pos = l + rand() % (r-l);
    int pivot = vec[pivot_pos];
    swap(vec[l], vec[pivot_pos]);

    for (int i = l+1; i != r; i++) {
        if (vec[i] <= pivot) {
            swap(vec[k++], vec[i]);
        }
    }

    swap(vec[k-1], vec[l]);
    return k-1;
}
```

When use [Lomuto Partition Algorithm](#lomuto-partition-algorithm) with quicksort, make sure you don't touch the middle(pivot) element, because it's already been placed at the corrent position.

```c++
void Qsort_r(int* vec, int l, int r) {
    if (l+1 >= r) return;
    auto mid = partition(vec, l, r);
    Qsort_r(vec, l, mid);
    Qsort_r(vec, mid+1, r); // dont touch the mid(pivot) because it's already placed at the corrent pos!
}
```


## Hoare's Partition Algorithm {#hoare-s-partition-algorithm}

Hoare's Partition Algorithm basically partitions the array into two parts:

-   The first \\([left, mid)\\) subset is **less or equal to** `pivot`
-   The second \\([mid, right)\\) subset is **greater or equal to** `pivot`

> **Think: If elements with the same value as the pivot(let's say \\(p\\)) appear multiple times in the array, does it mean that \\(p\\) will appear in both two subsets? If so, can we still use it in quicksort algorithm? Why? Does Lomuto Algorithm have this problem?**

```c++
int partition(int* vec, int l, int r) {
    int pivot_pos = l + rand() % (r-l);
    int pivot = vec[pivot_pos];
    int i = l, j = r-1;
    while(i <= j) {
        while(vec[i] < pivot) i++;
        while(vec[j] > pivot) j--;
        if (i <= j) {
            swap(vec[i], vec[j]);
            i++; j--;
        }
    }
    return i;
}
```

When use [Hoare's Partition Algorithm](#hoare-s-partition-algorithm) with quicksort, make sure you do qsort recursively to two continuous subsets.

```c++
void Qsort_r(int* vec, int l, int r) {
    if (l+1 >= r) return;
    auto mid = partition(vec, l, r);
    Qsort_r(vec, l, mid);
    Qsort_r(vec, mid, r);
}
```

> **Think: If you pick a pivot in advance, partition the rest \\(n-1\\) elements with Hoare's method, and finally swap the pivot back to `mid` place, then use the same `Qsort_r` function as the Lomuto method(don't touch `mid`). Will it yield the right result? Why not and which step leads to problem?**
