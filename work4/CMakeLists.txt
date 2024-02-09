cmake_minimum_required(VERSION 3.10)
project(work4)

find_package(OpenCV REQUIRED)

set(CMAKE_CXX_STANDARD 14)

add_executable(work4 main.cpp)

target_link_libraries(work4 ${OpenCV_LIBRARIES})
