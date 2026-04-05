# Trie


字典树？

acwing 算法基础课


```c
#include &lt;iostream&gt;
#include &lt;stdio.h&gt;
using namespace std;

const int N = 100010;
// 子节点编号(索引)
int son[N][26];
int cnt[N];
// 当前使用节点的索引
// 索引为0的点既是根节点 也是空节点
int idx; 

int insert(const char* s)
{
    int p = 0;
    for (int i = 0; s[i]; i&#43;&#43;) {
        int u = s[i] - &#39;a&#39;;
        if (!son[p][u]) son[p][u] = &#43;&#43;idx;
        p = son[p][u];
    }
    cnt[p] &#43;&#43;;
}

int query(const char *s)
{
    int p = 0;
    for (int i = 0; s[i]; i&#43;&#43;) {
        int u = s[i] - &#39;a&#39;;
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}

char str[N];
int main ()
{
    int n = 0;
    char op;
    scanf(&#34;%d\n&#34;, &amp;n);

    while(n--) {
        scanf(&#34;%c %s\n&#34;, &amp;op, &amp;str[0]);
        if (op == &#39;I&#39;) insert(str);
        else printf(&#34;%d\n&#34;, query(str));
    }
    
}
```




```c

const int N = 100010;
// 子节点编号(索引)
int son[N][32];
int cnt[N];
// 当前使用节点的索引
// 索引为0的点既是根节点 也是空节点
int idx; 

```



## 练习
AcWing 143. 最大异或对---Trie详解
《算法竞赛进阶指南》, 模板题

WHY: 为什么从高位往低位判断
ANS: 高位为1 数值越大

构造的Trie的每个叶子节点表示一个数, 重点节点表示有该位为0、1的树
将内层循环的遍历N个数, 复杂度 O(n) -&gt; 优化为 -&gt; 遍历至多31层的Trie, 复杂度O(1)

---

> Author:   
> URL: https://psuvtk.github.io/algo/trie/  

