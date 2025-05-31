cmake_minimum_required(VERSION 3.0.2)
project(balm3)

set(CMAKE_BUILD_TYPE "Release")
# set(CMAKE_CXX_FLAGS "-std=c++14")
# set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Find OpenMP package
find_package(OpenMP REQUIRED)
if(OpenMP_FOUND)
  message(STATUS "OpenMP found")
endif()

ADD_COMPILE_OPTIONS(-std=c++14 )
set( CMAKE_CXX_FLAGS "-std=c++14 -O3 ${OpenMP_CXX_FLAGS}" )

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fexceptions ${OpenMP_C_FLAGS}" )
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -pthread -std=c++0x -std=c++14 -fexceptions ${OpenMP_CXX_FLAGS}")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  pcl_conversions
  pcl_ros
)

find_package(Eigen3 REQUIRED)
find_package(PCL REQUIRED)
find_package(Ceres REQUIRED)

catkin_package(
  CATKIN_DEPENDS geometry_msgs nav_msgs roscpp rospy std_msgs 
  DEPENDS EIGEN3 PCL
  INCLUDE_DIRS
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${CERES_INCLUDE_DIRS}
)

add_executable(consistency src/simulation/consistency.cpp)
target_link_libraries(consistency ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenMP_CXX_LIBRARIES})

add_executable(benchmark_virtual src/benchmark/benchmark_virtual.cpp)
target_link_libraries(benchmark_virtual ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenMP_CXX_LIBRARIES})

add_executable(benchmark_realworld src/benchmark/benchmark_realworld.cpp)
target_link_libraries(benchmark_realworld ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenMP_CXX_LIBRARIES})


