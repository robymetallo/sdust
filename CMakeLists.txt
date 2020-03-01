cmake_minimum_required(VERSION 3.16)
project(sdust
        VERSION 0.1.1
        DESCRIPTION "Cmake for sdust library.")

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE STRING
            "Default BUILD_TYPE is ${default_build_type}" FORCE)
endif ()

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

add_library(sdust SHARED)

if (IPO_AVAILABLE)
    set_target_properties(sdust PROPERTIES INTERPROCEDURAL_OPTIMIZATION TRUE)
endif()
set_target_properties(sdust PROPERTIES
        C_STANDARD 99
        C_STANDARD_REQUIRED ON)

target_sources(sdust PRIVATE sdust.c
        PUBLIC sdust.h)

target_include_directories(sdust PRIVATE .)

target_link_libraries(sdust
        ZLIB::ZLIB)

target_compile_options(sdust PRIVATE
        -O3
        -Wc++-compat
        -Wall)

