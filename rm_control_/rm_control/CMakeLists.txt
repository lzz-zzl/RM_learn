cmake_minimum_required(VERSION 3.0.2)
project(rm_control)

find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        std_msgs
        controller_interface
        hardware_interface
        pluginlib
        geometry_msgs
        std_srvs
        nav_msgs
        sensor_msgs
        tf2
        tf2_ros
)

catkin_package(
        INCLUDE_DIRS include
        LIBRARIES standard_controller
        CATKIN_DEPENDS controller_interface hardware_interface pluginlib geometry_msgs

        nav_msgs
        sensor_msgs
        tf2
        tf2_ros
)

include_directories(
        include
        ${catkin_INCLUDE_DIRS}
)

# standard_controller控制器插件

add_library(standard_controller
        src/standard_controller.cpp
        src/omni_odom.cpp
)
target_link_libraries(standard_controller ${catkin_LIBRARIES})
add_dependencies(standard_controller ${catkin_EXPORTED_TARGETS})

install(TARGETS standard_controller
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

install(FILES standard_controller_plugin.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
# standard_controller控制器插件结束

# 发送
add_executable(standard_cmd_handle src/standard_cmd_handle.cpp
)
target_include_directories( standard_cmd_handle PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${catkin_INCLUDE_DIRS}
)
add_dependencies(standard_cmd_handle ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(standard_cmd_handle
        ${catkin_LIBRARIES}
)

add_executable(sim_shooter src/sim_shooter.cpp
)

add_dependencies(sim_shooter ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(sim_shooter
        ${catkin_LIBRARIES}
)

#add_executable(omni_odom src/omni_odom.cpp
#)
#
#add_dependencies(omni_odom ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
#
#target_link_libraries(omni_odom
#        ${catkin_LIBRARIES}
#)
#线性代收库
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIRS})