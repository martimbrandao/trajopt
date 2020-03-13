cmake_minimum_required(VERSION 2.6)

set(PROJECTNAME ctrajoptpy)
project(${PROJECTNAME})

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../cmake/modules")
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# Eigen
find_package(Eigen REQUIRED)
include_directories(${Eigen_INCLUDE_DIRS})

# OpenRAVE
find_package(OpenRAVE REQUIRED)
include_directories(${OpenRAVE_INCLUDE_DIRS})

# Trajopt
set(TRAJOPT_INCLUDE_DIRS
../src
../ext/bullet/src
../ext/json/include
)
include_directories(${TRAJOPT_INCLUDE_DIRS})
set(TRAJOPT_LIBRARIES ${CMAKE_CURRENT_SOURCE_DIR}/../build/lib/libtrajopt.so)

# Python
find_package(PythonInterp 2.7 REQUIRED)
find_package(PythonLibs 2.7 REQUIRED)
include_directories(${PYTHON_INCLUDE_DIRS})

# Boost
find_package(Boost COMPONENTS system thread program_options python timer REQUIRED)
include_directories(${Boost_INCLUDE_DIR})

# ctrajoptpy
add_library(ctrajoptpy SHARED ../src/trajopt/trajoptpy.cpp)
set_target_properties(ctrajoptpy PROPERTIES PREFIX "")
target_link_libraries(ctrajoptpy ${TRAJOPT_LIBRARIES} ${Boost_LIBRARIES} ${PYTHON_LIBRARIES})
