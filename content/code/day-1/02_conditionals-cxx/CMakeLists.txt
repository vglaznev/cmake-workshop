cmake_minimum_required(VERSION 3.18)

project(02_conditionals-cxx LANGUAGES CXX)

set(MAKE_SHARED_LIBRARY NO)

if(MAKE_SHARED_LIBRARY)
  add_library(message-library SHARED Message.hpp Message.cpp)
else()
  add_library(message-library STATIC Message.hpp Message.cpp)
endif()

set(target-type "")
get_target_property(target-type message-library TYPE)
include(CMakePrintHelpers)
cmake_print_variables(target-type)

add_executable(hello-world hello-world.cpp)

target_link_libraries(hello-world PRIVATE message-library)
