cmake_minimum_required(VERSION 3.10)

project(vlfeat
VERSION 0.9.21
HOMEPAGE_URL "https://github.com/vlfeat/vlfeat"
LANGUAGES C)

include(GNUInstallDirs)

set(CMAKE_C_STANDARD 11)

option(ENABLE_AVX "Build with AVX enabled" ON)
option(ENABLE_SSE2 "Build with SSE2 enabled" ON)

add_subdirectory(vl)