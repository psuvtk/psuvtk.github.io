# CMAKE




# 模板
```cmake
cmake_minimum_required(VERSION 3.11)
project(ProjectName)
target_link_libraries(ProjectName libarchive)
target_include_directories(ProjectName ${CMAKE_SOURCE_DIR}/include)
```


# 链接静态库
```cmake
target_link_libraries(csgo
    ${CMAKE_SOURCE_DIR}/libarchive_static.a
)

```

---

> Author: Kristoffer  
> URL: http://localhost:1313/posts/cmake/  

