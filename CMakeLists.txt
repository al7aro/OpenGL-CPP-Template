cmake_minimum_required(VERSION 3.25.0)
set(PROJECT_NAME ProjectName)
project(${PROJECT_NAME} VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR/bin})

set(PROJECT_DIR ${CMAKE_SOURCE_DIR}/${PROJECT_NAME})
set(PROJECT_INCLUDE_DIR ${PROJECT_DIR}/src)
set(PROJECT_SOURCE_DIR ${PROJECT_DIR}/src)
set(VENDOR_DIR ${PROJECT_DIR}/vendor)

# GLFW
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_DIR ${VENDOR_DIR}/glfw)
add_subdirectory(${GLFW_DIR})

# GLM
set(GLM_DIR ${VENDOR_DIR}/glm)
add_subdirectory(${GLM_DIR})

# IMGUI
set(IMGUI_DIR ${VENDOR_DIR}/imgui)
add_subdirectory(${IMGUI_DIR})

# GLAD
set(GLAD_DIR ${VENDOR_DIR}/glad)
add_subdirectory(${GLAD_DIR})

# OpenGL
find_package(OpenGL REQUIRED)

# Project Sources
file(GLOB_RECURSE source_files
    "${PROJECT_DIR}/src/*.cpp"
    "${PROJECT_DIR}/include/*.hpp"
	
	"${IMGUI_DIR}/*.h"
)

set(SOURCES ${source_files})

add_executable(${PROJECT_NAME} ${SOURCES} )

# Libraries
target_link_libraries(${PROJECT_NAME} glfw)
target_link_libraries(${PROJECT_NAME} glad)
target_link_libraries(${PROJECT_NAME} glm)
target_link_libraries(${PROJECT_NAME} imgui)
target_link_libraries(${PROJECT_NAME} OpenGL::GL)

# Headers
target_include_directories(${PROJECT_NAME} PUBLIC "${IMGUI_DIR}")

target_include_directories(${PROJECT_NAME} PUBLIC "${GLM_DIR}")
target_include_directories(${PROJECT_NAME} PUBLIC "${GLFW_DIR}/include")
target_include_directories(${PROJECT_NAME} PUBLIC "${GLAD_DIR}/include")

target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)

install(TARGETS ${PROJECT_NAME})
