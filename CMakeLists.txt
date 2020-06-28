# cmake_minimum_required(VERSION 3.16)
project(performance_tuning)

set(CMAKE_CXX_STANDARD 14)

add_executable(performance_tuning main.cpp)

add_executable(traverse_2d_array ./cpu-cache/traverse_2d_array/main.cpp)

add_executable(branch_predict ./cpu-cache/branch_predict/main.cpp)
