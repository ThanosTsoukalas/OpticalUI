cmake_minimum_required(VERSION 3.15)

add_library(glad STATIC)
# sources
target_sources(glad PRIVATE src/glad.c)

target_include_directories(glad PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_compile_features(glad PUBLIC cxx_std_17)
