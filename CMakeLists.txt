# CMake version
cmake_minimum_required (VERSION 3.8.2 FATAL_ERROR)

# Include cmake modulse
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/CMake")

# Include cmake modules
project ("CMake_OpenGL")

# Includes
include_directories(Includes)
include_directories(Libraries)

# Compile options
include(CMake/CompileOptions.cmake)

# Project modules
add_subdirectory(Sources)
add_subdirectory(Libraries/imgui)
add_subdirectory(Libraries/glad)
