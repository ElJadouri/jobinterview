cmake_minimum_required(VERSION 3.5)
project(order_optimizer)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(visualization_msgs REQUIRED)
find_package(msg_package REQUIRED)
find_package(rcl_yaml_param_parser REQUIRED)

find_package(yaml-cpp REQUIRED)
include_directories(${YAML_INCLUDE_DIRS})
target_link_libraries(${YAML_CPP_LIBRARIES})

# Add these lines to generate ROS message interfaces
#find_package(rosidl_default_generators REQUIRED)
#rosidl_generate_interfaces(${PROJECT_NAME}
#  "msg/Order.msg"
#)

ament_export_include_directories(include)
set(dependencies "geometry_msgs" "visualization_msgs" "rclcpp" "msg_package" "rcl_yaml_param_parser" "yaml-cpp")

add_library(${PROJECT_NAME} src/OrderOptimizerMain.cpp src/OrderOptimizerNode.cpp)
ament_target_dependencies(${PROJECT_NAME} ${dependencies})
target_link_libraries(${PROJECT_NAME} yaml-cpp)

add_executable(${PROJECT_NAME}_node src/OrderOptimizerMain.cpp src/OrderOptimizerNode.cpp)
target_link_libraries(${PROJECT_NAME}_node ${PROJECT_NAME})

install(TARGETS
    ${PROJECT_NAME}_node
    DESTINATION lib/${PROJECT_NAME}
    )
  
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()

