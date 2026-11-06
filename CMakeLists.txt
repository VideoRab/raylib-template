cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_STANDARD 20)

set(BUILD_SHARED_LIBS OFF CACHE BOOL "Build Raylib as static library" FORCE)

project(raylib-template)

add_subdirectory(thirdparty/raylib-6.2)
add_subdirectory(thirdparty/imgui-1.93.0-docking)
add_subdirectory(thirdparty/rlImGui-Raylib_6_0)

add_executable("${CMAKE_PROJECT_NAME}")

file(GLOB_RECURSE MY_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

target_sources("${CMAKE_PROJECT_NAME}" PRIVATE ${MY_SOURCES})

if(PRODUCTION_BUILD)
	target_compile_definitions("${CMAKE_PROJECT_NAME}" PUBLIC RESOURCES_PATH="./resources/") 
	target_compile_definitions("${CMAKE_PROJECT_NAME}" PUBLIC PRODUCTION_BUILD=1) 

else()
	target_compile_definitions("${CMAKE_PROJECT_NAME}" PUBLIC RESOURCES_PATH="${CMAKE_CURRENT_SOURCE_DIR}/resources/")
	target_compile_definitions("${CMAKE_PROJECT_NAME}" PUBLIC PRODUCTION_BUILD=0) 

endif()

target_link_libraries(${PROJECT_NAME} raylib_static imgui rlimgui)

if (APPLE)
    target_link_libraries(${PROJECT_NAME} "-framework IOKit")
    target_link_libraries(${PROJECT_NAME} "-framework Cocoa")
    target_link_libraries(${PROJECT_NAME} "-framework OpenGL")
endif()