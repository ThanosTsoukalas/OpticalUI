cmake_minimum_required(VERSION 3.15)

add_library(imgui STATIC)

# ImGUI sources
target_sources(imgui PRIVATE
    imgui.cpp
    imgui_draw.cpp
    imgui_widgets.cpp
    imgui_tables.cpp
    imgui_demo.cpp
)


# GLFW + OpenGL‑3 backend sources
target_sources(imgui PRIVATE
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
)

# Public include directories (core + backends)
target_include_directories(imgui PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/backends>
    $<INSTALL_INTERFACE:include>
)

# Link ImGui to its runtime dependencies
find_package(OpenGL REQUIRED)
target_link_libraries(imgui PUBLIC glfw glad OpenGL::GL)

target_compile_features(imgui PUBLIC cxx_std_17)