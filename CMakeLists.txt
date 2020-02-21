cmake_minimum_required(VERSION 3.16)
project(sdust
        VERSION 0.1.1
        DESCRIPTION "Cmake for sdust library.")

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE STRING
            "Default BUILD_TYPE is ${default_build_type}" FORCE)
endif ()

# Use '-fPIC' or '-fPIE' when not in Debug
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(ZLIB_MIN_VER 1.2.11)
find_package(ZLIB ${ZLIB_MIN_VER} REQUIRED)

include(CheckIPOSupported)
check_ipo_supported(RESULT IPO_AVAILABLE)
if (IPO_AVAILABLE)
    message(STATUS "IPO/LTO available")
    set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
else ()
    message(STATUS "IPO/LTO not available")
endif ()

if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
    #    set(CMAKE_POSITION_INDEPENDENT_CODE OFF)
    set(CMAKE_INTERPROCEDURAL_OPTIMIZATION FALSE)
endif ()

add_library(sdust SHARED)

set_property(TARGET sdust PROPERTY C_STANDARD 99)
set_property(TARGET sdust PROPERTY C_STANDARD_REQUIRED ON)

target_sources(sdust PRIVATE sdust.c
        PUBLIC sdust.h)

target_include_directories(sdust PRIVATE .)

target_link_libraries(sdust
        ZLIB::ZLIB)

target_compile_options(sdust INTERFACE
        -O3
        -Wc++-compat
        -Wall)

