# 并查集


## acwing 基础课 第一章 基础算法(一)


## 快排

```c
void quick_sort(int *q, int l, int r)
{
    // 边界处理, 记住其中一种即可
    // 取端点可能会退化为O(n)
    // 左端点要用 (l,j), (j&#43;1,r)
    // int x = q[l];
    // 右端点要用 (l,i-1), (i,r)
    // int x = q[r];
    // int x = q[(l&#43;r&#43;1) &gt;&gt; 1];
    int x = q[rand() % (r - l &#43; 1) &#43; l];

    // off-the-end, 后面直接先增加
    int i = l - 1;
    int j = r &#43; 1;
    
    if (l &gt;= r) return;
    
    while (i &lt; j) {
        do i&#43;&#43;; while (q[i] &lt; x);
        do j--; while (q[j] &gt; x);
        if (i &lt; j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j&#43;1, r);
}
```

---

> Author:   
> URL: https://psuvtk.github.io/algo/%E6%8E%92%E5%BA%8F/  

