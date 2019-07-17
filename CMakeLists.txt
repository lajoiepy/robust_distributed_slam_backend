# 
# This CMakeLists.txt should only be used for ROS
#

# Set minimum required version
cmake_minimum_required(VERSION 2.8.12)
project(robust_distributed_slam_module)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  geometry_msgs
  rtabmap_ros
  multi_robot_separators
  roslaunch
)
roslaunch_add_file_check(launch)
include_directories(${catkin_INCLUDE_DIRS})

catkin_package(
  CATKIN_DEPENDS roscpp multi_robot_separators geometry_msgs std_msgs message_runtime rtabmap_ros tf tf_conversions tf2_ros eigen_conversions sensor_msgs std_srvs
  DEPENDS system_lib
)

option(ARGOS_SIMULATION "Build for ARGoS Simulation" OFF)
option(IRL_ROBOT "Build for IRL robot" OFF)
option(ROS_IRL_ROBOT "Build for IRL robot with ROS" ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/buzz_slam/src/)