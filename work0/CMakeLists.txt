# CMakeList.txt: work0 的 CMake 项目，在此处包括源代码并定义
# 项目特定的逻辑。
#

# 将源代码添加到此项目的可执行文件。
cmake_minimum_required (VERSION 3.8.11)
project (work0)
set(CMAKE_CXX_STANDARD 20)
set(Eigen3_DIR "D:\\APP\\lib\\eigen-3.4.0\\share\\eigen3\\cmake")
find_package(Eigen3 REQUIRED)
message("${EIGEN3_INCLUDE_DIR}")
include_directories(${EIGEN3_INCLUDE_DIR})
add_executable(work0 "main.cpp" )


# TODO: 如有需要，请添加测试并安装目标。
