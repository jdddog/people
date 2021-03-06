cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

# Look for Bullet
find_package(PkgConfig REQUIRED)
pkg_check_modules(BULLET bullet)
include_directories(${BULLET_INCLUDE_DIRS})

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

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


rosbuild_add_executable(people_tracker src/people_tracking_node.cpp)
target_link_libraries(people_tracker people_tracking_filter ${BULLET_LIBRARIES})
rosbuild_add_library(people_tracking_filter 
                     src/uniform_vector.cpp 
                       src/gaussian_vector.cpp 
                       src/gaussian_pos_vel.cpp 
                       src/mcpdf_pos_vel.cpp
                       src/mcpdf_vector.cpp
                       src/sysmodel_pos_vel.cpp
                       src/sysmodel_vector.cpp
                       src/measmodel_pos.cpp
                       src/measmodel_vector.cpp
		       src/tracker_particle.cpp 
		       src/tracker_kalman.cpp 
		       src/detector_particle.cpp )

