# 算法STL


不管读字符还是读字符串都用%s来读取
```c
// 读取 I abc
char op[2];
scanf(&#34;%s%s&#34;, op, str);
```



```c

vector    支持[]
    size()
    empty()
    clear()   清空
    front()/back()   数组头尾元素
    push_back()/pop_back
    begin()/end()   首尾元素
    支持字典序比较
pair
    first   第一个元素
    second   第二个元素


string
    size()/length   返回长度
    empty()
    clear()
    substr(a,b)  得到一个下标位a长度位b的子字符串
    substr(a)   得到一个下表为a长度取最长的子字符串
    c_str()   取string定义数组的首地址


queue
    size()
    empty()
    push()   在队尾插入一个元素
    front()/back()   返回队头/队尾元素
    pop()   弹出队头元素


priority_queue 优先队列(堆) 默认大根堆
    size()
    empty()
    push()
    top()
    pop()
    定义小根堆: priority_queue&lt;int , vector&lt;int&gt; , greater&lt;int&gt;&gt;


stack    
    size()
    empty()
    push()
    pop()
    top()


deque       支持[]
    size()
    empty()
    clear()
    front()/back()   头尾元素
    push_back()/push_front()   在头尾插入
    begin()/end()



set , map , multimap , multiset , 基于平衡二叉树 (红黑树) 维护一个有序数列 大部分函数时间复杂度都是logn (因为是树)
    size()
    empty()
    clear()
    begin()/end()  支持&#43;&#43; -- 时间复杂度 O(logn)
    lower_bound()/upper_bound()
    lower_bound(x)     返回一个大于等于x的最小值
    upper_bound(x)     返回一个大于x的最小值


    set / multiset    set不支持重复数插入 插入重复数直接跳过
        insert()    插入一个数
        find()      查找一个数
        count()     返回一个数存入的个数
        erase()
            (1) 输入是一个x , 删除全部x , 时间复杂度 O(k &#43; logn)
            (2) 输入是一个迭代器 , 删除这个迭代器

    map / multimap
        insert()    插入的是一个pair
        erase()     输入的参数是pair 或者 迭代器
        find()
        支持[]



unordered_set , unordered_map , unordered_multiset , unordered_multimap
    基本跟上面一个一样 查找删改时间复杂度是 O(1)
    不支持lower_bound()/upper_bound()  迭代器的&#43;&#43; --



bitset   压位
    bitset&lt;放大小&gt; s;   初始化时候全为0  —— false;
    ~取反  &amp;且  |或  ^异或
    == , !=
    移位 &lt;&lt;   &gt;&gt;
    支持[]

    count()     返回有多少个1

    any()       判断是否有1
    none()      判断是否全为0

    set()       把所有位置为1
    set(k,v)    将第k位变成v
    reset()     把所有位置为0
    flip()      等价于~
    flip(x)     第k位取反

作者：yxc_助手1387号
链接：https://www.acwing.com/solution/content/90205/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

---

> Author:   
> URL: http://localhost:1313/algo/stl/  

