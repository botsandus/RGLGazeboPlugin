
########################################################################
# RobotecGPULidar
########################################################################
# By default the RobotecGPULidar library and its API header are vendored in
# this repository under `external/rgl` (built for Ubuntu 26.04 / ROS 2 Lyrical).
#
# To use a different build (e.g. a newer RGL, or a build with extra extensions),
# set both RGL_CUSTOM_LIBRARY_PATH and RGL_CUSTOM_API_HEADER_PATH. The API header
# version MUST match the library version: RGLServerPluginManager checks them at
# startup (see RGLServerPlugin/src/Utils.cc) and refuses to run on a mismatch.

set(RGL_VENDORED_DIR "${CMAKE_CURRENT_SOURCE_DIR}/rgl")
set(RGL_SO_FILENAME "libRobotecGPULidar.so")

set(RGL_CUSTOM_LIBRARY_PATH "" CACHE FILEPATH
    "Path to a custom RobotecGPULidar library build. Overrides the vendored library.")
set(RGL_CUSTOM_API_HEADER_PATH "" CACHE PATH
    "Path to the include directory with API headers compatible with the custom library build
     provided with `RGL_CUSTOM_LIBRARY_PATH`.
     In most cases it will be path to `include` directory of RobotecGPULidar project")

if((RGL_CUSTOM_LIBRARY_PATH AND NOT RGL_CUSTOM_API_HEADER_PATH) OR
   (NOT RGL_CUSTOM_LIBRARY_PATH AND RGL_CUSTOM_API_HEADER_PATH))
    message(FATAL_ERROR "It is required to set both `RGL_CUSTOM_LIBRARY_PATH` and `RGL_CUSTOM_API_HEADER_PATH`\
                         to finish custom RobotecGPULidar configuration")
endif()

if(RGL_CUSTOM_LIBRARY_PATH AND RGL_CUSTOM_API_HEADER_PATH) # Use a custom RobotecGPULidar build
    set(RGL_SO_PATH "${RGL_CUSTOM_LIBRARY_PATH}")
    set(RGL_INCLUDE_DIR "${RGL_CUSTOM_API_HEADER_PATH}")
else() # Use the RobotecGPULidar library vendored in this repository
    set(RGL_SO_PATH "${RGL_VENDORED_DIR}/lib/${RGL_SO_FILENAME}")
    set(RGL_INCLUDE_DIR "${RGL_VENDORED_DIR}/include")
endif()

if(NOT EXISTS "${RGL_SO_PATH}")
    message(FATAL_ERROR "RobotecGPULidar library not found at: ${RGL_SO_PATH}")
endif()

add_library(RobotecGPULidar SHARED IMPORTED GLOBAL)
set_target_properties(RobotecGPULidar PROPERTIES
    IMPORTED_LOCATION "${RGL_SO_PATH}"
    INTERFACE_INCLUDE_DIRECTORIES "${RGL_INCLUDE_DIR}"
)
