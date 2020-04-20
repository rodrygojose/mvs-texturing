cmake_minimum_required(VERSION 3.1)

LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/elibs/tbb")

project(MVSTexturing)
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
    ${TBB_INCLUDE_DIRS}
    ${CMAKE_BINARY_DIR}/src/rayint/libs
    ${CMAKE_BINARY_DIR}/src/mve/libs
    ${CMAKE_BINARY_DIR}/src/eigen
    ${CMAKE_BINARY_DIR}/src/mapmap/
    ${CMAKE_BINARY_DIR}/src/mapmap/mapmap
    ${CMAKE_BINARY_DIR}/src/mapmap/ext/dset
    ${CMAKE_SOURCE_DIR}/libs)

include_directories(SYSTEM ${MVS_TEXTURING_INCLUDE})

link_directories(
    ${CMAKE_BINARY_DIR}/src/mve/libs/mve
    ${CMAKE_BINARY_DIR}/src/mve/libs/util
)

## Roc: add all headers and .cpp from mve
#file (GLOB HEADERS "libs/tex/*.h")
#file (GLOB SOURCES "libs/tex/[^_]*.cpp")
file (GLOB HEADERS "libs/tex/*.h" "${CMAKE_BINARY_DIR}/src/mve/[^_]*.h")
file (GLOB SOURCES "libs/tex/[^_]*.cpp" "${CMAKE_BINARY_DIR}/src/mve/[^_]*.cpp")

set(LIBRARY mvstex)
add_library(${LIBRARY} STATIC ${SOURCES})
set_property(TARGET ${LIBRARY} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)
add_dependencies(${LIBRARY} mve rayint eigen mapmap)
#target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} -lmve -lmve_util ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${TIFF_LIBRARIES})
target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${TIFF_LIBRARIES})
#target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} mve mve_util ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${TIFF_LIBRARIES})
#install(TARGETS ${LIBRARY} ARCHIVE DESTINATION lib)

##########

#add_subdirectory(apps)
