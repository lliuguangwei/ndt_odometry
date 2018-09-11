cmake_minimum_required(VERSION 2.8.3)
project(ndt_odometry)

# set(CMAKE_CXX_STANDARD 11)
add_definitions(-std=c++11 -msse -msse2 -msse3 -msse4 -msse4.1 -msse4.2)
set(CMAKE_CXX_FLAGS "-std=c++11 -msse -msse2 -msse3 -msse4 -msse4.1 -msse4.2")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")

set(CMAKE_BUILD_TYPE "RELEASE")

find_package(PCL REQUIRED)

IF (NOT (PCL_VERSION VERSION_LESS "1.7.2"))
    SET(PCL_OPENMP_PACKAGES pcl_omp_registration)
ENDIF (NOT (PCL_VERSION VERSION_LESS "1.7.2"))

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  roscpp
  sensor_msgs
  std_msgs
  novatel_msgs
  pcl_ros
  pcl_conversions
  velodyne_pointcloud
  geodesy
  nmea_msgs
  message_generation
  message_filters
  ndt_gpu
)

catkin_package(
  CATKIN_DEPENDS std_msgs velodyne_pointcloud  ${PCL_OPENMP_PACKAGES}
        DEPENDS PCL
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

find_package(OpenCV 3.0 QUIET)
if(NOT OpenCV_FOUND)
   find_package(OpenCV 2.4.3 QUIET)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
   endif()
endif()

# find_package(Eigen3 3.1.0 REQUIRED)
find_package(Eigen3 REQUIRED)

add_executable(ndt_odometry ndt_odometry.cc)
target_link_libraries(ndt_odometry ${catkin_LIBRARIES})
add_dependencies(ndt_odometry ${catkin_EXPORTED_TARGETS})
