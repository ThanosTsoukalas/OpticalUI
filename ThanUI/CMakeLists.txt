cmake_minimum_required(VERSION 3.15)
project(ThanUI LANGUAGES C CXX)

# ------------------------------------------------------------------
# Paths to local copies of third‑party libraries
# ------------------------------------------------------------------
set(LOCAL_GLFW_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/extern_libs/glfw")
set(LOCAL_GLAD_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/extern_libs/glad")
set(LOCAL_IMGUI_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/extern_libs/imgui")

# ------------------------------------------------------------------
# Verify that the required sources exist
# ------------------------------------------------------------------
if (NOT EXISTS "${LOCAL_GLFW_ROOT}/CMakeLists.txt")
    message(FATAL_ERROR "GLFW source not found at ${LOCAL_GLFW_ROOT}")
endif()
if (NOT EXISTS "${LOCAL_GLAD_ROOT}/src/glad.c")
    message(FATAL_ERROR "GLAD source not found at ${LOCAL_GLAD_ROOT}")
endif()
if (NOT EXISTS "${LOCAL_IMGUI_ROOT}/imgui.cpp")
    message(FATAL_ERROR "ImGui source not found at ${LOCAL_IMGUI_ROOT}")
endif()

# ------------------------------------------------------------------
# Collect ThanUI sources
# ------------------------------------------------------------------
file(GLOB_RECURSE THANUI_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
file(GLOB_RECURSE THANUI_HDR "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h" "${CMAKE_CURRENT_SOURCE_DIR}/include/*.hpp")

# ------------------------------------------------------------------
# Create the static library
# ------------------------------------------------------------------
add_library(ThanUI STATIC ${THANUI_SRC} ${THANUI_HDR})

target_include_directories(ThanUI
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_compile_features(ThanUI PUBLIC cxx_std_17)

# ------------------------------------------------------------------
# Add third‑party sub‑directories (they each produce a target)
# ------------------------------------------------------------------
add_subdirectory("${LOCAL_GLFW_ROOT}" "${CMAKE_CURRENT_BINARY_DIR}/glfw_build")
add_subdirectory("${LOCAL_GLAD_ROOT}" "${CMAKE_CURRENT_BINARY_DIR}/glad_build")
add_subdirectory("${LOCAL_IMGUI_ROOT}" "${CMAKE_CURRENT_BINARY_DIR}/imgui_build")

# ------------------------------------------------------------------
# Link ThanUI to the three dependencies
# ------------------------------------------------------------------
target_link_libraries(ThanUI PUBLIC glfw glad imgui)

# ------------------------------------------------------------------
# Expose the headers of the dependencies
# ------------------------------------------------------------------
target_include_directories(ThanUI PUBLIC
    $<BUILD_INTERFACE:${LOCAL_GLFW_ROOT}/include>
    $<BUILD_INTERFACE:${LOCAL_GLAD_ROOT}/include>
    $<BUILD_INTERFACE:${LOCAL_IMGUI_ROOT}>          # ImGui public headers
    $<INSTALL_INTERFACE:include>
)

# ------------------------------------------------------------------
# (Optional) Install rules – uncomment if you want `make install`
# ------------------------------------------------------------------
# install(TARGETS ThanUI EXPORT ThanUITargets
#         ARCHIVE DESTINATION lib
#         LIBRARY DESTINATION lib
#         RUNTIME DESTINATION bin)
# install(DIRECTORY include/ DESTINATION include)
# install(EXPORT ThanUITargets FILE ThanUITargets.cmake
#         NAMESPACE ThanUI::
#         DESTINATION lib/cmake/ThanUI)
