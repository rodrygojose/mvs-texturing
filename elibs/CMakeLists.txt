externalproject_add(mapmap
    PREFIX          ${CMAKE_BINARY_DIR}/libs/mapmap
    GIT_REPOSITORY  https://github.com/dthuerck/mapmap_cpu.git
    GIT_TAG         master
    UPDATE_COMMAND  ""
    SOURCE_DIR      ${CMAKE_BINARY_DIR}/src/mapmap
    CONFIGURE_COMMAND ""
    BUILD_COMMAND   ""
    INSTALL_COMMAND ""
)

externalproject_add(rayint
    PREFIX          ${CMAKE_BINARY_DIR}/libs/rayint
    GIT_REPOSITORY  https://github.com/nmoehrle/rayint.git
    GIT_TAG         cuda
    UPDATE_COMMAND  ""
    SOURCE_DIR      ${CMAKE_BINARY_DIR}/src/rayint
    CONFIGURE_COMMAND ""
    BUILD_COMMAND   ""
    INSTALL_COMMAND ""
)

externalproject_add(eigen
    PREFIX          ${CMAKE_BINARY_DIR}/libs/eigen
    URL             https://bitbucket.org/eigen/eigen/get/3.3.2.tar.gz
    URL_MD5         36b5731ab7d7e0c10843ac93bd9fd270
    SOURCE_DIR      ${CMAKE_BINARY_DIR}/src/eigen
    CONFIGURE_COMMAND ""
    BUILD_COMMAND   ""
    INSTALL_COMMAND ""
)

externalproject_add(mve
    PREFIX          ${CMAKE_BINARY_DIR}/libs/mve
    GIT_REPOSITORY  https://github.com/nmoehrle/mve.git
    UPDATE_COMMAND  ""
    SOURCE_DIR      ${CMAKE_BINARY_DIR}/src/mve
    CONFIGURE_COMMAND ""
    BUILD_COMMAND   make -C libs/mve && make -C libs/util #not platform independent
    BUILD_IN_SOURCE 1
    INSTALL_COMMAND ""
)
