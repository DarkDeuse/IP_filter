cmake_minimum_required(VERSION 3.25.3)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(ip_filter VERSION ${PROJECT_VERSION})

configure_file(version.h.in version.hpp)

add_executable(ip_filteration main.cpp)

add_library(ip_filter ip_filter.cpp)


target_include_directories(ip_filter PRIVATE "${CMAKE_BINARY_DIR}")

set_target_properties(ip_filteration ip_filter PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)

target_link_libraries(ip_filteration PRIVATE
    ip_filter
)

if (CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
    CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    target_compile_options(ip_filteration PRIVATE 
        -g
        -Wall
        -Wextra
        -Werror
        -pedantic
    )
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(ip_filter PRIVATE
        /EHsc
        /W4
        /WX
    )
endif()