cmake_minimum_required(VERSION 3.12)
project(ex_simp)

# A simple modern CMake file that uses ament_cmake to export a target (ex_thing) and a dependency (rclcpp).

# Reference:
# https://docs.ros.org/en/humble/How-To-Guides/Ament-CMake-Documentation.html#building-a-library

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)

add_library(ex_thing SHARED src/ex_thing.cpp)

# Add an ALIAS
add_library(ex_simp::ex_thing ALIAS ex_thing)

# Declare a PUBLIC dependency on rclcpp
target_link_libraries(ex_thing
  PUBLIC
    rclcpp::rclcpp)

# Mark the include files PUBLIC
target_include_directories(ex_thing
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

# Manually install include directory
install(DIRECTORY include/${PROJECT_NAME}
  DESTINATION include)

# Install the target(s), marking them for export
install(TARGETS ex_thing
  EXPORT ex_simp_targets
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)

# Calls install(EXPORT ex_simp_targets ...) to export the target(s)
ament_export_targets(ex_simp_targets HAS_LIBRARY_TARGET)

# Export the dependencies
ament_export_dependencies(rclcpp)

ament_package()
