cmake_minimum_required(VERSION 3.12)

project(descriptor-mutable-test)

# Always add debug symbols
if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
endif()

# GLFW
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

add_subdirectory(${CMAKE_SOURCE_DIR}/glfw-3.3.8)

# Vulkan
find_package(Vulkan REQUIRED)
find_package(glm REQUIRED)

add_executable(${CMAKE_PROJECT_NAME} descriptor-mutable-test.cpp)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE Vulkan::Vulkan glm::glm glfw ${CMAKE_DL_LIBS})
target_compile_features(${CMAKE_PROJECT_NAME} PUBLIC cxx_std_11)


# Compile vertex shader
set(shader_bin "${CMAKE_CURRENT_BINARY_DIR}/vert.spv")
execute_process(COMMAND "glslc" "${CMAKE_CURRENT_SOURCE_DIR}/shader.vert" "-o${shader_bin}" COMMAND_ECHO STDOUT) 

# Compile fragment shader
set(shader_bin "${CMAKE_CURRENT_BINARY_DIR}/frag.spv")
execute_process(COMMAND "glslc" "${CMAKE_CURRENT_SOURCE_DIR}/shader.frag" "-o${shader_bin}" COMMAND_ECHO STDOUT) 