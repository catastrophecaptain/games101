cmake_minimum_required(VERSION 3.10)
project(work1)

# set(OpenCV_DIR "D:\\APP\\lib\\opencv-4.9.0\\x64\\mingw\\lib")
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# set(Eigen3_DIR "D:\\APP\\lib\\eigen-3.4.0\\share\\eigen3\\cmake")
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE "Debug")

# include_directories(/usr/local/include)

add_executable(Rasterizer main.cpp rasterizer.hpp rasterizer.cpp Triangle.hpp Triangle.cpp)
target_link_libraries(Rasterizer ${OpenCV_LIBRARIES})
