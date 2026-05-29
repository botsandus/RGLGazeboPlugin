cmake_minimum_required(VERSION 3.10.2 FATAL_ERROR)

# This is required for Qt
if(POLICY CMP0100)
    cmake_policy(SET CMP0100 NEW)
endif()

project(RGLVisualize)

set(CMAKE_AUTOMOC ON)

find_package(gz-cmake REQUIRED)
gz_find_package(gz-msgs REQUIRED)
gz_find_package(gz-rendering REQUIRED)
gz_find_package(gz-gui REQUIRED)

# Gazebo Jetty (gz-gui 10+) GUI is built against Qt6
find_package(Qt6 REQUIRED COMPONENTS Core Quick QuickControls2 Widgets)

qt_add_resources(resources_RCC RGLVisualize.qrc)

add_library(RGLVisualize SHARED
    RGLVisualize.cc
    ${resources_RCC}
)

target_link_libraries(RGLVisualize
    PRIVATE
    gz-msgs::gz-msgs
    gz-rendering::gz-rendering
    gz-gui::gz-gui
)

## Install libraries

install(TARGETS RGLVisualize
    DESTINATION RGLVisualize
)
