# 要求的最低CMake版本
cmake_minimum_required(VERSION 3.16)
# 项目名称+语言
project(TinyWebServer CXX)

# 生成可执行文件
add_executable(server
    main.cpp
    webserver.cpp
    config.cpp
    timer/lst_timer.cpp
    http/http_conn.cpp
    log/log.cpp
    CGImysql/sql_connection_pool.cpp
)

target_compile_features(server PRIVATE cxx_std_11)
target_compile_options(server PRIVATE -Wall -Wextra)
# 默认用 Debug；想用 Release 就在 cmake 时指定
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()


# 链接库
find_library(MYSQL_LIB mysqlclient)
find_path(MYSQL_INCLUDE mysql/mysql.h)

if(NOT MYSQL_LIB)
    message(FATAL_ERROR "找不到 libmysqlclient")
endif()

target_include_directories(server PRIVATE ${MYSQL_INCLUDE})
target_link_libraries(server pthread ${MYSQL_LIB})


# cmake_minimum_required(VERSION 3.10)   # ① 最低版本
#project(名字 CXX)                       # ② 项目名 + 语言

#add_executable(目标 a.cpp b.cpp)        # ③ 生成可执行文件
#add_library(库名 STATIC a.cpp)         # ④ 生成库（静态/动态）

#target_link_libraries(目标 pthread)     # ⑤ 链接库
#target_include_directories(目标 PRIVATE include/)   # ⑥ 头文件路径
#target_compile_options(目标 PRIVATE -Wall -O2)      # ⑦ 编译选项

#find_package(Qt5 COMPONENTS Widgets REQUIRED)       # ⑧ 找依赖
#set(变量 值)                                        # ⑨ 定义变量
#add_subdirectory(子目录)                            # ⑩ 加子模块
