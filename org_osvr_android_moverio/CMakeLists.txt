cmake_minimum_required(VERSION 3.1)
project(org_osvr_android_moverio)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# This looks for an osvrConfig.cmake file - most of the time it can be
# autodetected but you might need to create/extend CMAKE_PREFIX_PATH to include something like
# C:/Users/Ryan/Desktop/build/OSVR-Core-vc12 or
# C:/Users/Ryan/Downloads/OSVR-Core-Snapshot-v0.1-406-gaa55515-build54-vs12-32bit
# in the CMake GUI or command line.
find_package(osvr)

# This generates a header file, from the named json file, containing a string literal
# named com_osvr_android_sensorTracker_json (not null terminated)
# The file must be added as a source file to some target (as below) to be generated.
osvr_convert_json(org_osvr_android_moverio_json
    org_osvr_android_moverio.json
    "${CMAKE_CURRENT_BINARY_DIR}/org_osvr_android_moverio_json.h")

# Be able to find our generated header file.
include_directories("${CMAKE_CURRENT_BINARY_DIR}")

# This is just a helper function wrapping CMake's add_library command that
# sets up include dirs, libraries, and naming convention (no leading "lib")
# for an OSVR plugin. It also installs the plugin into the right directory.
# Pass as many source files as you need. See osvrAddPlugin.cmake for full docs.
osvr_add_plugin(NAME org_osvr_android_moverio
    CPP # indicates we'd like to use the C++ wrapper
    SOURCES
    org_osvr_android_moverio.cpp
  "${CMAKE_CURRENT_BINARY_DIR}/org_osvr_android_moverio_json.h")

# If you use other libraries, find them and add a line like:
target_link_libraries(org_osvr_android_moverio android log)
