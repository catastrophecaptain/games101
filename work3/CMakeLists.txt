cmake_minimum_required(VERSION 3.10)
project(work3)
#set(TOOLCHAIN_PATH "D:/APP/lib/vcpkg/scripts/buildsystems/vcpkg.cmake")
#set(OpenCV_DIR "D:\\APP\\lib\\opencv-4.9.0\\x64\\mingw\\lib")
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# set(Eigen3_DIR "D:\\APP\\lib\\eigen-3.4.0\\share\\eigen3\\cmake")
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE "Debug")

add_executable(work3 main.cpp rasterizer.hpp rasterizer.cpp global.hpp Triangle.hpp Triangle.cpp Texture.hpp Texture.cpp Shader.hpp OBJ_Loader.h)
target_link_libraries(work3 ${OpenCV_LIBRARIES})
#target_compile_options(Rasterizer PUBLIC -Wall -Wextra -pedantic)