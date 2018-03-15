cmake_minimum_required(VERSION 3.5)
project(fecal)

set(CMAKE_CXX_STANDARD 11)

# Dependency: GF256 library source files
set(GF256_LIB_SRCFILES
        gf256.cpp
        gf256.h)

# FEC-AL library source files
set(FECAL_LIB_SRCFILES
        fecal.cpp
        fecal.h
        FecalCommon.cpp
        FecalCommon.h
        FecalDecoder.cpp
        FecalDecoder.h
        FecalEncoder.cpp
        FecalEncoder.h)

add_library(gf256 ${GF256_LIB_SRCFILES})
add_library(fecal ${FECAL_LIB_SRCFILES})

add_executable(benchmark tests/benchmark.cpp)
target_link_libraries(benchmark gf256 fecal)
