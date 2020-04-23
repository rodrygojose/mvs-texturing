#include(ExternalProject)

#externalproject_add(mve
#    PREFIX          mve_prefix
#    GIT_REPOSITORY  https://github.com/nmoehrle/mve.git
#    UPDATE_COMMAND  ""
#    SOURCE_DIR      ${CMAKE_BINARY_DIR}/mve
#    CONFIGURE_COMMAND ""
#    BUILD_COMMAND   make -C ${CMAKE_BINARY_DIR}/mve/libs/mve && make -C ${CMAKE_BINARY_DIR}/mve/libs/util #not platform independent
#    INSTALL_COMMAND ""
#)

add_subdirectory(mve)
