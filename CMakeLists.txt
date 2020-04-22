cmake_minimum_required(VERSION 3.1)
project(MVSTexturing)
cmake_policy(SET CMP0077 NEW)
cmake_policy(SET CMP0069 NEW)

option(MVS_TEXTURING_BUILD_STATIC "Build a static MVE TEXTURING libraries, instead of shared" ON)
if(${MVS_TEXTURING_BUILD_STATIC})
    set(MVE_BUILD_STATIC ON)
else()
    set(MVE_BUILD_STATIC OFF)
endif(${MVS_TEXTURING_BUILD_STATIC})

LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/elibs/tbb")
include(ExternalProject)
if(NOT CMAKE_BUILD_TYPE)
    message(STATUS "Setting build type to 'RELWITHDEBINFO' as none was specified.")
    set(CMAKE_BUILD_TYPE RELWITHDEBINFO)
endif()

#Search for TBB library (release build)
set(CMAKE_BUILD_TYPE_BACKUP ${CMAKE_BUILD_TYPE})
set(CMAKE_BUILD_TYPE RELEASE)
FIND_PACKAGE(TBB REQUIRED)
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE_BACKUP})

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED yes)
add_definitions(-DEIGEN_MPL2_ONLY)

FIND_PACKAGE(OpenMP)
if(OPENMP_FOUND)
    add_definitions(-DWITH_OPENMP)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -Wall -Wextra -Wundef -pedantic")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mfpmath=sse -funroll-loops")
endif()

find_package(PNG REQUIRED)
find_package(JPEG REQUIRED)
find_package(TIFF REQUIRED)

add_subdirectory(elibs)

set(MVS_TEXTURING_INCLUDE
    ${CMAKE_CURRENT_SOURCE_DIR}/ext_libs/eigen
    ${TBB_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/rayint/libs
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/mve/libs
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/mapmap/
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/mapmap/mapmap
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/mapmap/ext/dset
    libs
)

include_directories(SYSTEM ${MVS_TEXTURING_INCLUDE})

link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/mve/libs/mve
    ${CMAKE_CURRENT_SOURCE_DIR}/elibs/mve/libs/util
)

add_subdirectory(libs)
add_subdirectory(apps)
