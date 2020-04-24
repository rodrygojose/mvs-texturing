include(ExternalProject)

## ROC: External project is added in this way to use the
## build command `make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/mve && make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/util`
## however, 
externalproject_add(mve
   PREFIX          mve_prefix
   GIT_REPOSITORY  https://github.com/nmoehrle/mve.git
   UPDATE_COMMAND  ""
   SOURCE_DIR      ${CMAKE_CURRENT_SOURCE_DIR}/mve
   CONFIGURE_COMMAND ""
   BUILD_COMMAND   make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/mve && make -C ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/util #not platform independent
   INSTALL_COMMAND ""
)

set(MVE_INCLUDE_DIRS
    ${CMAKE_CURRENT_SOURCE_DIR}/ext_libs/eigen
    ${TBB_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}/rayint/libs
    ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs
    ${CMAKE_CURRENT_SOURCE_DIR}/mapmap
    ${CMAKE_CURRENT_SOURCE_DIR}/mapmap/mapmap
    ${CMAKE_CURRENT_SOURCE_DIR}/mapmap/ext/dset
   CACHE INTERNAL "MVE_INCLUDE_DIRS"
)

set(MVE_LIBS_DIR
   ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/mve
   ${CMAKE_CURRENT_SOURCE_DIR}/mve/libs/util
   CACHE INTERNAL "MVE_LIBS_DIR"
)
