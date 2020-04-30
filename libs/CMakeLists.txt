cmake_minimum_required(VERSION 3.1)
include(ExternalProject)

set(MVE_INCLUDE_DIRS
    ${TBB_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}/ext_libs/eigen/Eigen
    ${CMAKE_CURRENT_SOURCE_DIR}/rayint/libs
    ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs
    ${CMAKE_CURRENT_SOURCE_DIR}/mapmap
    ${CMAKE_CURRENT_SOURCE_DIR}/mapmap/mapmap
    ${CMAKE_CURRENT_SOURCE_DIR}/mapmap/ext/dset
   CACHE INTERNAL "MVE_INCLUDE_DIRS"
)

set(MVE_LIBRARY
   ${TBB_LIBRARIES}
   ${OpenMP_CXX_LIBRARIES}
   ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/mve/libmve.a
   ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/util/libmve_util.a
   ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${TIFF_LIBRARIES}
   CACHE INTERNAL "MVE_LIBRARY"
)

## ROC: External project is added to use the build command
## `make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/mve && make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/util`
## But the mve source code does not need to be downloaded
externalproject_add(mve
   PREFIX          mve_prefix
   # GIT_REPOSITORY  https://github.com/nmoehrle/mve.git
   UPDATE_COMMAND  ""
   SOURCE_DIR      ${CMAKE_CURRENT_SOURCE_DIR}/mve
   CONFIGURE_COMMAND ""
   BUILD_COMMAND   make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/mve && make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/util #not platform independent
   INSTALL_COMMAND ""
)
