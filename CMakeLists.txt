cmake_minimum_required(VERSION 2.8)
project(imgui_Vulkan-imgui C CXX)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()

set(CMAKE_CXX_STANDARD 11)
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DVK_PROTOTYPES")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DVK_PROTOTYPES")

# GLFW
set(GLFW_DIR submodules/glfw) # Set this to point to an up-to-date GLFW repo
option(GLFW_BUILD_EXAMPLES "Build the GLFW example programs" OFF)
option(GLFW_BUILD_TESTS "Build the GLFW test programs" OFF)
option(GLFW_BUILD_DOCS "Build the GLFW documentation" OFF)
option(GLFW_INSTALL "Generate installation target" OFF)
option(GLFW_DOCUMENT_INTERNALS "Include internals in documentation" OFF)
add_subdirectory(${GLFW_DIR} binary_dir EXCLUDE_FROM_ALL)
include_directories(${GLFW_DIR}/include ${IMGUI_DIR}/backends)

# Dear ImGui:
set(IMGUI_DIR submodules/imgui)
include_directories(${IMGUI_DIR} ${IMGUI_DIR}/backends)
file(GLOB IMGUI_SRC ${IMGUI_DIR}/*.cpp)

# Libraries:
find_package(Vulkan REQUIRED)
set(LIBRARIES "glfw;Vulkan::Vulkan")

# GLFW with Vulkan headers:
include_directories(${GLFW_DIR}/deps)

# Src files.
# file(GLOB sources src/*.cpp)

# add_executable(Vulkan-imgui ${sources} ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp ${IMGUI_DIR}/backends/imgui_impl_vulkan.cpp ${IMGUI_SRC})
add_executable(Vulkan-imgui src/test.cpp ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp ${IMGUI_DIR}/backends/imgui_impl_vulkan.cpp ${IMGUI_SRC})
target_link_libraries(Vulkan-imgui ${LIBRARIES})
target_compile_definitions(Vulkan-imgui PUBLIC -DImTextureID=ImU64)
