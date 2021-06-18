https://blog.csdn.net/weixin_30877181/article/details/96754442

https://www.bilibili.com/video/BV16V411k7eF

cmake 这玩意儿是跨平台的，用起来比 makefile 似乎要容易一点

然后就是 clion 似乎是以 cmake 为编译工具的。。。。

makefile 更适合 gdb 和 vim 吧，我猜。。。。

名字： CMakeLists.txt

第一步：CMAKE_MINIMUM_REQUIRED(VERSION 3.10)

第二步： PROJECT(projectname )

第三步：ADD_EXECUTABLE(object_name test.cpp)

当文件越来越多的时候，添加源文件就很操蛋。。。。。所以，需要自动添加，函数为

AUX_SOURCE_DIRECTORY(dir var_name)

然后把 var_name 放入 第三步的源文件中，但是需要加 ${var_name}

如何写子目录的 CMakeLists.txt

aux_source_directory(. dir_lib_srcs)

add_library(Mylib STATIC ${dir_lib_srcs})

然后在顶层的 CMakeLists.txt里面加上

ADD_SUBDIRECTORY(./mylib)

TARGET_LINK_LIBRARY(demo Mylib)



执行 cmake

cmake ./  在当前目录执行



cmakelists添加下面几句，可以编译多线程
FIND_PACKAGE(Threads)
TARGET_LINK_LIBRARIES(multi_thread ${CMAKE_THREAD_LIBS_INIT})
上面的 mutil_thread 是add_excutable()生成的执行文件

