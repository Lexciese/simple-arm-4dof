cmake_minimum_required(VERSION 3.16)
project(simple_arm LANGUAGES CXX)

if(CMAKE_CXX_COMPILER_ID MATCHES "(GNU|Clang)")
  add_compile_options(-Wall -Wextra)
endif()

# set the same behavior for windows as it is on linux
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

# find dependencies
set(THIS_PACKAGE_INCLUDE_DEPENDS
  hardware_interface
  pluginlib
  rclcpp
  rclcpp_lifecycle
)

# Specify the required version of ros2_control
find_package(controller_manager 4.0.0)
# Handle the case where the required version is not found
if(NOT controller_manager_FOUND)
  message(FATAL_ERROR "ros2_control version 4.0.0 or higher is required. "
  "Are you using the correct branch of the ros2_control_demos repository?")
endif()

# find dependencies
find_package(backward_ros REQUIRED)
find_package(ament_cmake REQUIRED)
foreach(Dependency IN ITEMS ${THIS_PACKAGE_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()

## COMPILE
add_library(
  simple_arm
  SHARED
  hardware/rrbot.cpp
)
target_compile_features(simple_arm PUBLIC cxx_std_17)
target_include_directories(simple_arm PUBLIC
$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/hardware/include>
$<INSTALL_INTERFACE:include/simple_arm>
)
ament_target_dependencies(
  simple_arm PUBLIC
  ${THIS_PACKAGE_INCLUDE_DEPENDS}
)

# Export hardware plugins
pluginlib_export_plugin_description_file(hardware_interface simple_arm_hardware_interface_plugin.xml)

# INSTALL
install(
  DIRECTORY hardware/include/
  DESTINATION include/simple_arm
)
install(
  DIRECTORY description/launch description/ros2_control description/urdf description/gazebo description/meshes description/rrbot
  DESTINATION share/simple_arm
)
install(
  DIRECTORY bringup/launch bringup/config
  DESTINATION share/simple_arm
)
install(TARGETS simple_arm
  EXPORT export_simple_arm
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

if(BUILD_TESTING)
  find_package(ament_cmake_pytest REQUIRED)

  ament_add_pytest_test(example_9_urdf_xacro test/test_urdf_xacro.py)
  ament_add_pytest_test(view_example_9_launch test/test_view_robot_launch.py)
  ament_add_pytest_test(run_example_9_launch test/test_rrbot_launch.py)
endif()

## EXPORTS
ament_export_targets(export_simple_arm HAS_LIBRARY_TARGET)
ament_export_dependencies(${THIS_PACKAGE_INCLUDE_DEPENDS})
ament_package()
