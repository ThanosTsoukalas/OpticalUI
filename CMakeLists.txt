# ThanUI/demo/CMakeLists.txt
cmake_minimum_required(VERSION 3.15)
project(ThanUIDemo LANGUAGES CXX)

# Pull in the library target defined in the parent directory
add_subdirectory("ThanUI")   # “..” = path to the library CMakeLists.txt

# Build the demo executable
add_executable(ThanUIDemo src/main.cpp)

# Link it **statically** to the library we just added
target_link_libraries(ThanUIDemo PRIVATE ThanUI)


# Ensure the demo can see the public headers
target_include_directories(ThanUIDemo PRIVATE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>)
