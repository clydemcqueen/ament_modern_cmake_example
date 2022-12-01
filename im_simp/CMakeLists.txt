cmake_minimum_required(VERSION 3.12)
project(im_simp)

# A simple modern CMake file that uses ament_cmake to import a target (ex_thing) and a dependency (rclcpp).

find_package(ament_cmake REQUIRED)
find_package(ex_simp REQUIRED)

add_library(im_thing SHARED src/im_thing.cpp)

target_link_libraries(im_thing
  PRIVATE
    ex_simp::ex_thing)

target_include_directories(im_thing
  PRIVATE
    include)

ament_package()
