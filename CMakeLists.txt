cmake_minimum_required(VERSION 3.5)
project(naoqi_bridge_msgs)

set(FastRTPS_INCLUDE_DIR /opt/ros/eloquent/include) 
set(FastRTPS_LIBRARY_RELEASE /opt/ros/eloquent/lib/libfastrtps.so)

# ROS2 now supports native Windows, those flags give this compatibility
if(NOT WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wextra")
endif()

set(MESSAGE_DEPS
  actionlib_msgs
  sensor_msgs
  nav_msgs
  trajectory_msgs
  builtin_interfaces
  rosidl_default_generators
  geometry_msgs)

find_package(ament_cmake REQUIRED)

find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(actionlib_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(trajectory_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
#find_package(genmsg REQUIRED)

# Find all the messages files
FILE(GLOB MSG_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
      "${CMAKE_CURRENT_SOURCE_DIR}/msg/*.msg")

## Generate messages in the 'msg' folder
set(msg_files
  ${MSG_FILES}
)

# Find all the service files
FILE(GLOB SRV_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
      "${CMAKE_CURRENT_SOURCE_DIR}/srv/*.srv")

## Generate messages in the 'srv' folder
set(srv_files
  ${SRV_FILES}
)

# Find all the action files
FILE(GLOB ACTION_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
      "${CMAKE_CURRENT_SOURCE_DIR}/action/*.action")

## Generate messages in the 'action' folder
set(action_files
  ${ACTION_FILES}
)

## Generate added messages and services with any dependencies listed here
rosidl_generate_interfaces(${PROJECT_NAME}
   ${msg_files}
   ${srv_files}
   ${action_files}
   DEPENDENCIES ${MESSAGE_DEPS}
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()

install(
  FILES rosbridge_mapping_rules.yaml
  DESTINATION share/${PROJECT_NAME})

