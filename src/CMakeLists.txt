# Copyright (c) Huawei Technologies Co., Ltd. 2019. All rights reserved.

# CMake lowest version requirement
cmake_minimum_required(VERSION 3.5.1)

# project information
project(classification_video)

# Compile options
add_compile_options(-std=c++11)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY  "../../../out")
set(CMAKE_CXX_FLAGS_DEBUG "-fPIC -O0 -g -Wall")
set(CMAKE_CXX_FLAGS_RELEASE "-fPIC -O2 -Wall")

set(INC_PATH $ENV{DDK_PATH})

if (NOT DEFINED ENV{DDK_PATH})
    set(INC_PATH "/usr/local/Ascend")
    message(STATUS "set default INC_PATH: ${INC_PATH}")
else ()
    message(STATUS "env INC_PATH: ${INC_PATH}")
endif()

set(LIB_PATH $ENV{NPU_HOST_LIB})

if (NOT DEFINED ENV{NPU_HOST_LIB})
    set(LIB_PATH "/usr/local/Ascend/acllib/lib64/stub/")
    message(STATUS "set default LIB_PATH: ${LIB_PATH}")
else ()
    message(STATUS "env LIB_PATH: ${LIB_PATH}")
endif()

if (${CMAKE_CXX_COMPILER} MATCHES "aarch")
    set(ARCH "arm")
else ()
    set(ARCH "x86")
endif()

# Header path
#include_directories(
#    $ENV{HOME}/ascend_ddk/${ARCH}/include/
#    $ENV{HOME}/ascend_ddk/${ARCH}/include/opencv4
#    ${INC_PATH}/acllib/include/
#    ../inc/
#)

include_directories(
    $ENV{HOME}/ascend_ddk/${ARCH}/include/
    $ENV{HOME}/ascend_ddk/${ARCH}/include/opencv4
    #/usr/local/include/
    #/usr/local/include/opencv4
    #/home/zoufangyu/Ascend/ascend-toolkit/3.3.0.alpha001/x86_64-linux/acllib/include/
    /home/zoufangyu/Ascend/ascend-toolkit/3.3.0.alpha001/arm64-linux/acllib/include/
    /home/zoufangyu/Ascend/ascend-toolkit/3.3.0.alpha001/x86_64-linux/toolkit/toolchain/hcc/aarch64-target-linux-gnu/sys-include/

    ${INC_PATH}/acllib/include/
    ../inc/
)

if(target STREQUAL "Simulator_Function")
    add_compile_options(-DFUNC_SIM)
endif()

# add host lib path
#link_directories(
#    /usr/local/lib
#    ${LIB_PATH}
#    $ENV{HOME}/ascend_ddk/${ARCH}/lib/
#    $ENV{HOME}/Ascend/
#    ${INC_PATH}/atc/lib64
#)

# add host lib path
link_directories(
    ${LIB_PATH}
    $ENV{HOME}/ascend_ddk/${ARCH}/lib/
    ${INC_PATH}/atc/lib64
    
    #/home/zoufangyu/Ascend/ascend-toolkit/3.3.0.alpha001/x86_64-linux/acllib/lib64
    /home/zoufangyu/Ascend/ascend-toolkit/3.3.0.alpha001/arm64-linux/acllib/lib64/
    /usr/aarch64-linux-gnu
)


add_executable(main
        utils.cpp
        model_process.cpp
        sample_process.cpp
        main.cpp)

if(target STREQUAL "Simulator_Function")
    target_link_libraries(main funcsim)
else()
    target_link_libraries(main
			  /usr/lib/gcc-cross/aarch64-linux-gnu/libstdc++.so.6
			  ascendcl acl_dvpp stdc++ opencv_highgui  opencv_core opencv_imgproc opencv_imgcodecs opencv_calib3d opencv_features2d opencv_videoio)
endif()

install(TARGETS main DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
