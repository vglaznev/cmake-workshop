cmake_minimum_required(VERSION 3.18)

project(03_loops-cxx LANGUAGES CXX)

function(print_target_sources_compiler_options target)
  get_target_property(target_compile_options ${target} COMPILE_OPTIONS)
  get_target_property(source_list ${target} SOURCES)
  foreach(source ${source_list})
    get_source_file_property(compiler_options ${source} COMPILE_OPTIONS)
    message("Source ${source} compiler options: ${compiler_options}. " 
            "Target ${target} compiler options: ${target_compile_options}")
  endforeach()
endfunction()

#Use all files in folder to produce target

#[[
1.
Globbing. Dont recommend by CMake developers.
Needs to run cmake generation process after adding new files.
]]

#file(GLOB sources RELATIVE ${CMAKE_CURRENT_LIST_DIR} "*.cpp" "*.hpp")

#[[
2.
Still not recommended. CONFIGURE_DEPENDS doesnt work properly.
]]

file(GLOB sources RELATIVE ${CMAKE_CURRENT_LIST_DIR} CONFIGURE_DEPENDS "*.cpp" "*.hpp")

#[[
3.
]]

#set(sources 
#  geometry_circle.cpp
#  geometry_circle.hpp
#  geometry_polygon.cpp
#  geometry_polygon.hpp
#  geometry_rhombus.cpp
#  geometry_rhombus.hpp
#  geometry_square.cpp
#  geometry_square.hpp
#) 

list(REMOVE_ITEM sources compute-areas.cpp)

add_library(geometry_lib ${sources})
target_compile_options(geometry_lib PRIVATE -O3)

list(APPEND sources_with_other_compiler_flags
  geometry_polygon.cpp
  geometry_polygon.hpp
  geometry_rhombus.cpp
  geometry_rhombus.hpp
)

foreach(source ${sources_with_other_compiler_flags})
  set_source_files_properties(${source} PROPERTIES COMPILE_OPTIONS -O2)
endforeach()

print_target_sources_compiler_options(geometry_lib)

add_executable(compute_areas compute-areas.cpp)
target_link_libraries(compute_areas PRIVATE geometry_lib)
