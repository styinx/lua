cmake_minimum_required(VERSION 3.21)
project(lua)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR})


# Build the library
add_library(lua STATIC
        lua/lgc.c
        lua/lfunc.c
        lua/ldo.c
        lua/ldebug.c
        lua/ldblib.c
        lua/lcode.c
        lua/lbaselib.c
        lua/lauxlib.c
        lua/lapi.c
        lua/lzio.c
        lua/lvm.c
        lua/lundump.c
        lua/ltm.c
        lua/ltests.c
        lua/lstrlib.c
        lua/ltable.c
        lua/lstring.c
        lua/lstate.c
        lua/lparser.c
        lua/lobject.c
        lua/lmathlib.c
        lua/lmem.c
        lua/liolib.c
        lua/llex.c)
