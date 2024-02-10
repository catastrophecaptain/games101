cmake_minimum_required(VERSION 3.10)
project(work5)

set(CMAKE_CXX_STANDARD 17)

add_executable(work5 main.cpp Object.hpp Vector.hpp Sphere.hpp global.hpp Triangle.hpp Scene.cpp Scene.hpp Light.hpp Renderer.cpp)
target_compile_options(work5 PUBLIC -pedantic)
target_compile_features(work5 PUBLIC cxx_std_17)
target_link_libraries(work5 PUBLIC)
