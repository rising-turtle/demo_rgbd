project(demo_rgbd)

cmake_minimum_required(VERSION 2.4.6)
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -w")
SET(CMAKE_BUILD_TYPE "RelWithDebInfo")

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

# rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

set(OpenCV_DIR "/home/hzhang8/work/3rdlib/opencv-3.3.1/build")
find_package(OpenCV REQUIRED)
message("OpenCV_INCLUDE_DIR: ${OpenCV_INCLUDE_DIRS}")
include_directories("/home/hzhang8/work/3rdlib/opencv-3.3.1/install/include")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    cv_bridge
    sensor_msgs
    std_msgs
    pcl_conversions
    pcl_ros
)

catkin_package(
    # DEPENDS Eigen
    # CATKIN_DEPENDS roscpp std_msgs sensor_msgs cv_bridge
    # INCLUDE_DIRS include
    # LIBRARIES
)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})
include_directories("${PROJECT_SOURCE_DIR}/include")
include_directories(${catkin_INCLUDE_DIRS})

add_subdirectory(isamlib)
link_libraries(isamlib)
link_libraries(${catkin_LIBRARIES})
link_libraries(${OpenCV_LIBS})

add_executable(featureTracking src/featureTracking.cpp)
add_executable(visualOdometry src/visualOdometry.cpp)
add_executable(bundleAdjust src/bundleAdjust.cpp)
add_executable(processDepthmap src/processDepthmap.cpp)
add_executable(stackDepthPoint src/stackDepthPoint.cpp)
add_executable(transformMaintenance src/transformMaintenance.cpp)
add_executable(registerPointCloud src/registerPointCloud.cpp)

# rosbuild_add_executable(featureTracking src/featureTracking.cpp)
# rosbuild_add_executable(visualOdometry src/visualOdometry.cpp)
# rosbuild_add_executable(bundleAdjust src/bundleAdjust.cpp)
# rosbuild_add_executable(processDepthmap src/processDepthmap.cpp)
# rosbuild_add_executable(stackDepthPoint src/stackDepthPoint.cpp)
# rosbuild_add_executable(transformMaintenance src/transformMaintenance.cpp)
# rosbuild_add_executable(registerPointCloud src/registerPointCloud.cpp)
