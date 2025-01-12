
project(lua)

cmake_minimum_required(VERSION 3.21)

#
# Functions
#

# 'message' wrapper
function(print what)
    message("   ${what}")
endfunction()


#
# Paths
#

# Local paths
set(DIR_ROOT "${PROJECT_SOURCE_DIR}")
set(DIR_INCLUDE "${DIR_ROOT}/include")
set(DIR_SOURCE "${DIR_ROOT}/source")
set(DIR_LIBRARY "${DIR_ROOT}/lib")
set(DIR_TEST "${DIR_ROOT}/test")

# VCPKG paths
set(DIR_VCPKG_INSTALL "${DIR_VCPKG_ROOT}/installed/${VCPKG_TARGET_TRIPLET}")
set(DIR_VCPKG_INCLUDE "${DIR_VCPKG_INSTALL}/include")
set(DIR_VCPKG_LIBRARY "${DIR_VCPKG_INSTALL}/lib")
set(DIR_VCPKG_LIBRARY_DEBUG "${DIR_VCPKG_INSTALL}/debug/lib")

# Binary output paths
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${DIR_LIBRARY}/deb")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG "${DIR_LIBRARY}/deb")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${DIR_ROOT})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${DIR_LIBRARY}/rel")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE "${DIR_LIBRARY}/rel")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${DIR_ROOT})


#
# CXX
#

set(CMAKE_CXX_STANDARD 17)

if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang") # Clang
    #
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU") # GCC
    set(COMPILER_FLAGS "-Wall -Wextra -ansi -pedantic -std=c++17")
    set(COMPILER_FLAGS_DEBUG "-MTd")
    set(COMPILER_FLAGS_RELEASE "-O3 -DNDEBUG -MT")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC") # MSVC
    set(COMPILER_FLAGS "")
    set(COMPILER_FLAGS_DEBUG "/Zi /Ob0 /Od /RTC1 /MTd")
    set(COMPILER_FLAGS_RELEASE "/O2 /Ob2 /DNDEBUG /MT")
endif ()

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} ${COMPILER_FLAGS} ${COMPILER_FLAGS_DEBUG}")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} ${COMPILER_FLAGS} ${COMPILER_FLAGS_RELEASE}")


# Dependencies


FUNCTION(DEBUG_WX_LIBS var)
    SET(liblist "")
    FOREACH (f ${var})
        STRING(REGEX REPLACE "${DIR_VCPKG_LIBRARY}/wx(.*)u(.*)//.lib" "${DIR_VCPKG_LIBRARY_DEBUG}/wx//1ud//2.lib" f ${f})
        LIST(APPEND liblist "${f}")
    ENDFOREACH ()
    SET(wxWidgets_LIBRARIES "${liblist}" PARENT_SCOPE)
ENDFUNCTION()

IF ("${CMAKE_BUILD_TYPE}" MATCHES "Debug")
    DEBUG_WX_LIBS("${wxWidgets_LIBRARIES}")
ELSE ()

ENDIF ()


# CMAKE

set(INCLUDE_DIRECTORIES
        ${DIR_INCLUDE}
        ${DIR_SOURCE}

        ${DIR_VCPKG_INCLUDE})

include_directories(${INCLUDE_DIRECTORIES})

if (CMAKE_BUILD_TYPE EQUAL "DEBUG")
    set(LIBRARY_DIRECTORIES
            ${DIR_VCPKG_LIBRARY}
            ${DIR_VCPKG_LIBRARY}/deb)
else()
    set(LIBRARY_DIRECTORIES
            ${DIR_VCPKG_LIBRARY}
            ${DIR_VCPKG_LIBRARY}/rel)
endif()

link_directories(${LIBRARY_DIRECTORIES})


# Sources

set(SOURCES
    lutf8lib.c
    lvm.c
    lzio.c
    lapi.c
    lcode.c
    lcorolib.c
    lctype.c
    ldebug.c
    ldo.c
    ldump.c
    lfunc.c
    lgc.c
    linit.c
    llex.c
    lmem.c
    loadlib.c
    lobject.c
    lopcodes.c
    loslib.c
    lparser.c
    lstate.c
    lstring.c
    ltable.c
    ltests.c
    ltm.c
    lundump.c
	lvm.c
	lzio.c
)

set(SOURCES_LIB
    lauxlib.c
    lbaselib.c
    ldblib.c
    liolib.c
    lmathlib.c
    lstrlib.c
    ltablib.c
)

set(SOURCES_LUAC
    dump.c
    opt.c
    print.c
    stubs.c
)

source_group("src"      FILES ${SOURCES})
source_group("src/lib"  FILES ${SOURCES_LIB})
source_group("src/luac" FILES ${SOURCES_LUAC})


# Binaries

add_library(lua ${SOURCES})
add_library(lualib ${SOURCES_LIB})

add_executable(luai lua.c)
add_executable(luac luac.c ${SOURCES_LUAC})

target_link_libraries(luai lualib lua)
target_link_libraries(luac lualib lua)
