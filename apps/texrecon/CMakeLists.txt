file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

set(BIN texrecon)

add_executable(${BIN} ${SOURCES})
set_property(TARGET ${BIN} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)

add_dependencies(${BIN} mve)
include_directories(${MVS_TEXTURING_INCLUDE})
target_link_libraries(${BIN} mvstex ${TBB_LIBRARIES} -lmve -lmve_util)
