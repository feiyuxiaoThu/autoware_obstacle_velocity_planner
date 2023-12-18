
cmake_minimum_required(VERSION 3.21)
project(obstacle_velocity_planner)

#set(CMAKE_CXX_STANDARD 14)
add_compile_options(-std=c++17 -Wall)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})



find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIRS})

set(osqp_DIR ${PROJECT_SOURCE_DIR}/thirdparty/osqp/install/lib/cmake/osqp)
find_package(osqp REQUIRED)

#指定库路径
file(GLOB_RECURSE Opencv2.7_LIB "/usr/lib/python2.7/config-x86_64-linux-gnu/*.so")
#指定头文件路径
set(Opencv2.7_INLCUDE_DIRS "/usr/include/python2.7")
#添加头文件到工程
include_directories(include
        ${Opencv2.7_INLCUDE_DIRS}
        )
# 通常这样设置就可以，如果你的项目包含qrc文件，那么需要将它们单独添加进来
aux_source_directory(. DIRS_SRCS)

include_directories(include)
include_directories(${Boost_INCLUDE_DIR})


include_directories(${PYTHON_INCLUDE_DIRS})


add_executable(obstacle_velocity_planner main.cpp
                                         src/interpolation/linear_interpolation.cpp
                                         src/interpolation/spline_interpolation.cpp
                                         src/csc_matrix_conv.cpp
                                         src/osqp_interface.cpp
                                         src/velocity_optimizer.cpp
                                         src/velocity_smoother.cpp)

target_link_libraries(obstacle_velocity_planner osqp::osqp ${Opencv2.7_LIB})