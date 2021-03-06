cmake_minimum_required(VERSION 2.8.8 FATAL_ERROR)

find_package( PkgConfig )
pkg_check_modules( EIGEN3 REQUIRED eigen3 )

find_package(gazebo REQUIRED)
FIND_PACKAGE( Boost 1.40 COMPONENTS system REQUIRED )
if (PKG_CONFIG_FOUND)
  pkg_check_modules(GAZEBO gazebo)
  pkg_check_modules(SDF sdformat)
endif()
add_definitions(-std=c++11)

include_directories(
  include
  ${GAZEBO_INCLUDE_DIRS}
  ${SDF_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  ${CMAKE_CURRENT_BINARY_DIR}/msgs
  )

link_directories(${GAZEBO_LIBRARY_DIRS} ${CMAKE_CURRENT_BINARY_DIR}/msgs)
add_subdirectory(msgs)

list(APPEND CMAKE_CXX_FLAGS "${GAZEBO_CXX_FLAGS}")

add_library(hello_world SHARED src/hello_world.cc)
target_link_libraries(hello_world ${GAZEBO_LIBRARIES})

add_library(rotors_gazebo_controller_interface SHARED src/gazebo_controller_interface.cpp)
target_link_libraries(rotors_gazebo_controller_interface ${GAZEBO_LIBRARIES})
add_dependencies(rotors_gazebo_controller_interface mav_msgs)

add_library(rotors_gazebo_motor_model SHARED src/gazebo_motor_model.cpp)
target_link_libraries(rotors_gazebo_motor_model mav_msgs ${Boost_LIBRARIES} ${GAZEBO_LIBRARIES} ${SDF_LIBRARIES})
add_dependencies(rotors_gazebo_motor_model mav_msgs)

add_library(rotors_gazebo_multirotor_base_plugin SHARED src/gazebo_multirotor_base_plugin.cpp)
target_link_libraries(rotors_gazebo_multirotor_base_plugin mav_msgs ${Boost_LIBRARIES} ${GAZEBO_LIBRARIES} ${SDF_LIBRARIES})
add_dependencies(rotors_gazebo_multirotor_base_plugin mav_msgs)

add_library(rotors_gazebo_imu_plugin SHARED src/gazebo_imu_plugin.cpp)
target_link_libraries(rotors_gazebo_imu_plugin mav_msgs ${Boost_LIBRARIES} ${GAZEBO_LIBRARIES} ${SDF_LIBRARIES})
add_dependencies(rotors_gazebo_imu_plugin mav_msgs)

add_library(rotors_gazebo_mavlink_interface SHARED src/gazebo_mavlink_interface.cpp)
target_link_libraries(rotors_gazebo_mavlink_interface mav_msgs ${Boost_LIBRARIES} ${GAZEBO_LIBRARIES} ${SDF_LIBRARIES})
add_dependencies(rotors_gazebo_mavlink_interface mav_msgs)
