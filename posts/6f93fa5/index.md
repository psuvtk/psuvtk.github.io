# CMake备忘




## 进阶技巧

碰到就学习，有用就记住 

### FetchContent

https://cmake.org/cmake/help/latest/module/FetchContent.html




## Modern CMake

https://hsf-training.github.io/hsf-training-cmake-webpage/aio/index.html

https://gist.github.com/mbinna/c61dbb39bca0e4fb7d1f73b0d66a4fd1


https://cliutils.gitlab.io/modern-cmake/chapters/intro/dodonot.html



## 常用

```cmake
cmake_minimum_required(VERSION 3.11)

project(ProjectName)

target_link_libraries(ProjectName libarchive)
target_include_directories(ProjectName ${CMAKE_SOURCE_DIR}/include)


# 链接静态库
target_link_libraries(csgo
    ${CMAKE_SOURCE_DIR}/libarchive_static.a
)


# 安装


```


## 启动构建

统一的构建命令
```bash
cmake -S . -B build
cmake --build build --target install
```

old style
```bash
mkdir -p build 
cd build
cmake ..
make 
make install 
```


&lt;!--more--&gt;


---

> Author:   
> URL: http://localhost:1313/posts/6f93fa5/  

