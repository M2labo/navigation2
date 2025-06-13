cmake_minimum_required(VERSION 3.5)
project(nav2_through_poses_navigator)

find_package(ament_cmake REQUIRED)
find_package(ament_index_cpp REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav2_common REQUIRED)
find_package(nav2_core REQUIRED)
find_package(nav2_msgs REQUIRED)
find_package(nav2_util REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(pluginlib REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)

nav2_package()

add_library(nav2_navigate_through_poses SHARED
  src/navigators/navigate_through_poses.cpp
)

target_include_directories(nav2_navigate_through_poses PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include/${PROJECT_NAME}>
)

target_link_libraries(nav2_navigate_through_poses PUBLIC
  ${geometry_msgs_TARGETS}
  nav2_core::nav2_core
  ${nav2_msgs_TARGETS}
  ${nav_msgs_TARGETS}
  nav2_util::nav2_util_core
  rclcpp::rclcpp
  rclcpp_action::rclcpp_action
)

target_link_libraries(nav2_navigate_through_poses PRIVATE
  ament_index_cpp::ament_index_cpp
  pluginlib::pluginlib
  rclcpp_lifecycle::rclcpp_lifecycle
)

pluginlib_export_plugin_description_file(nav2_core navigator_through_poses_plugins.xml)

install(TARGETS nav2_navigate_through_poses
  EXPORT export_nav2_navigate_through_poses
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY include/
  DESTINATION include/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_export_include_directories(include/${PROJECT_NAME})
ament_export_libraries(nav2_navigate_through_poses)
ament_export_dependencies(geometry_msgs nav2_core nav2_msgs nav2_util nav_msgs pluginlib rclcpp rclcpp_action)
ament_export_targets(export_nav2_navigate_through_poses)

ament_package()
