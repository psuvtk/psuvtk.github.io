# 结构体位域与大小端


&lt;!--more--&gt;

## 大小端
```
Data:          0x   AB  CD  12  34
Big Endian:         AB  CD  12  34
Little Endian:      34  12  CD  AB
```


## 结构体位域

首先看相同的结构体定义在不同大小端机器上的表现;

```c
struct S {
  uint32_t A : 5;
  uint32_t B : 6;
  uint32_t C : 7;
  uint32_t D : 8;
  uint32_t E : 6;
};
```

如果上述结构体位域用于小端机器，则表示的`数据`和`存储格式`分别如下:  
```
                    31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
                     E  E  E  E  E  E  D  D  D  D  D  D  D  D  C  C  C  C  C  C  C  B  B  B  B  B  B  A  A  A  A  A

Little Endian:      |         Byte0         |         Byte1         |         Byte2         |         Byte2         |
                     7  6  5  4  3  2  1  0  7  6  5  4  3  2  1  0  7  6  5  4  3  2  1  0  7  6  5  4  3  2  1  0
                     B  B  B  A  A  A  A  A  C  C  C  C  C  B  B  B  D  D  D  D  D  D  C  C  E  E  E  E  E  E  D  D 
```
如果上述结构体位域用于大端机器，则表示的`数据`和`存储格式`分别如下：
```
                    31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
                     A  A  A  A  A  B  B  B  B  B  B  C  C  C  C  C  C  C  D  D  D  D  D  D  D  D  E  E  E  E  E  E

Big Endian   :      |         Byte0         |         Byte1         |         Byte2         |         Byte2         |
                     7  6  5  4  3  2  1  0  7  6  5  4  3  2  1  0  7  6  5  4  3  2  1  0  7  6  5  4  3  2  1  0
                     A  A  A  A  A  B  B  B  B  B  B  C  C  C  C  C  C  C  D  D  D  D  D  D  D  D  E  E  E  E  E  E
```

## 实例

然后看相同的格式如何在不同的大小端机器上面定义, 此处用IPv4报文头格式举例（一般从左到右标注MSB-&gt;LSB, 此处可以理解为从左到右为数据包BIT流到达的顺序）：
```
0                   1                   2                   3   
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;
|Version|  IHL  |Type of Service|          Total Length         |
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;
|         Identification        |Flags|      Fragment Offset    |
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;
|  Time to Live |    Protocol   |         Header Checksum       |
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;
|                       Source Address                          |
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;
|                    Destination Address                        |
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;
|                    Options                    |    Padding    |
&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;-&#43;

                Example Internet Datagram Header

                            Figure 4.
```
### 小端

如果是小端机器，则需要使用如下定义:
```c
struct {
  uint32_t version          : 4;
  uint32_t ihl              : 4;
  uint32_t type_of_service  : 8;
  uint32_t total_length     : 16;
  ...
};
```
或者
```c
struct {
  uint8_t version          : 4;
  uint8_t ihl              : 4;
  uint8_t type_of_service  : 8;
  uint16_t total_length    : 16;
  ...
};
```
或者(Linux内核中 [iphdr定义](https://elixir.bootlin.com/linux/v5.10/source/include/uapi/linux/ip.h#L86))

```c
struct {
  uint8_t version : 4, ihl : 4;
  uint8_t type_of_service  : 8;
  uint16_t total_length    : 16;
  ...
};
```

### 大端
如果是大端机器，则需要使用如下定义:
```c
struct {
  uint32_t total_length     : 16;
  uint32_t type_of_service  : 8;
  uint32_t ihl              : 4;
  uint32_t version          : 4;
  ...
};
```


### 在线测试
可以在[在线编译网站](https://www.onlinegdb.com/online_c_compiler)上面测试一下:
```c
/******************************************************************************

                            Online C Compiler.
                Code, Compile, Run and Debug C program online.
Write your code in this editor and press &#34;Run&#34; button to compile and execute it.

*******************************************************************************/

#include &lt;stdio.h&gt;
#include &lt;stdint.h&gt;

struct A {
  uint32_t version          : 4;
  uint32_t ihl              : 4;
  uint32_t type_of_service  : 8;
  uint32_t total_length     : 16;
};

struct B {
  uint8_t version          : 4;
  uint8_t ihl              : 4;
  uint8_t type_of_service  : 8;
  uint16_t total_length    : 16;
};

struct C {
  uint8_t version : 4, ihl : 4;
  uint8_t type_of_service  : 8;
  uint16_t total_length    : 16;
};

int main()
{
    struct A a = {0};
    struct B b = {0};
    struct C c = {0};
    
    a.version = 1;
    a.ihl = 2;
    a.type_of_service = 0x33;
    a.total_length = 0x4444;
    b.version = 1;
    b.ihl = 2;
    b.type_of_service = 0x33;
    b.total_length = 0x4444;
    c.version = 1;
    c.ihl = 2;
    c.type_of_service = 0x33;
    c.total_length = 0x4444;
    
    printf(&#34;struct A: 0x%08x\n&#34;, *(uint32_t*)&amp;a);
    printf(&#34;struct B: 0x%08x\n&#34;, *(uint32_t*)&amp;b);
    printf(&#34;struct C: 0x%08x\n&#34;, *(uint32_t*)&amp;c);
    return 0;
}
```

预期输出:
```c
struct A: 0x44443321
struct B: 0x44443321
struct C: 0x44443321
```


---

> Author: Kristoffer  
> URL: https://psuvtk.github.io/posts/419e98c/  

