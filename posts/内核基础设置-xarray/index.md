# 内核基础设施之XArray



## Entry的不同类型
```c
/*
 * The bottom two bits of the entry determine how the XArray interprets
 * the contents:
 *
 * 00: Pointer entry
 * 10: Internal entry
 * x1: Value entry or tagged pointer
 *
 * Attempting to store internal entries in the XArray is a bug.
 *
 * Most internal entries are pointers to the next node in the tree.
 * The following internal entries have a special meaning:
 *
 * 0-62: Sibling entries
 * 256: Retry entry
 * 257: Zero entry
 *
 * Errors are also represented as internal entries, but use the negative
 * space (-4094 to -2).  They&#39;re never stored in the slots array; only
 * returned by the normal API.
 */


```

---

> Author: Kristoffer  
> URL: http://localhost:1313/posts/%E5%86%85%E6%A0%B8%E5%9F%BA%E7%A1%80%E8%AE%BE%E7%BD%AE-xarray/  

