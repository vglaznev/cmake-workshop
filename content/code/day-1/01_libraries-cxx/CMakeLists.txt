cmake_minimum_required(VERSION 3.18)

project(01_libraries_cxx LANGUAGES CXX)

add_library(message-library Message.hpp Message.cpp)

add_executable(hello-world hello-world.cpp)

target_link_libraries(hello-world PRIVATE message-library)
