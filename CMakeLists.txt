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
        lgc.c
        lfunc.c
        ldo.c
        ldebug.c
        ldblib.c
        lcode.c
        lbaselib.c
        lauxlib.c
        lapi.c
        lzio.c
        lvm.c
        lundump.c
        ltm.c
        ltests.c
        lstrlib.c
        ltable.c
        lstring.c
        lstate.c
        lparser.c
        lobject.c
        lmathlib.c
        lmem.c
        liolib.c
        llex.c)
