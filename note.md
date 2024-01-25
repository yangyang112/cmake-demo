https://www.hahack.com/codes/cmake/

1.参考mymuduo 写一个动态链接的demo出来 

# demo0 

project(cmake_demo) 可以在多个CMakeLists里出现
cmake_minimum_required (VERSION 2.8)　多次出现没有问题
add_subdirectory($name $dependencies) $name在一个项目中，只能出现一次



# demo 1 
add_executable(Demo main.cc)
单个文件　
Demo1/
├── CMakeLists.txt
└── main.cc
# demo2 
同一目录　多个源文件

add_executable(Demo ${DIR_SRCS})
```
# 查找目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
```

# demo3 
多个目录，多个源文件

```
Demo3/
├── CMakeLists.txt
├── main.cc
└── math   //这里输出一个.a 文件
    ├── CMakeLists.txt
    ├── MathFunctions.cc
    └── MathFunctions.h

add_library (MathFunctions ${DIR_LIB_SRCS})  // 出来的是静态库.a 
target_link_libraries(Demo3 MathFunctions)
```

# demo4 
自定义编译选项　

## 设置　

在CMakeLists.txt 中使用
```
option (USE_MYMATH
	   "Use provided math implementation" ON)
configure_file (
  "${PROJECT_SOURCE_DIR}/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
  )
```


## 使用
cmake -DUSER_MYMATH=OFF ../

```
// 这个文件记录该工程记录的所有设置
cat Demo4/config.h 　
/* #undef USE_MYMATH */
```

cmake -DUSER_MYMATH=ON ../ 　
```
cat Demo4/config.h 
#define USE_MYMATH

```

除了通过观看cmakelist.txt之外，还有什么方法能够知道这些选项
```
# cmake path/to/source -LAH

// Use provided math implementation
USE_MYMATH:BOOL=ON

```

# demo5
添加测试　和安装

```
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```
