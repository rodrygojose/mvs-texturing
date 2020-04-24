file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

set(BIN texrecon)
add_executable(${BIN} ${SOURCES})
set_property(TARGET ${BIN} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)

add_dependencies(${BIN} mve)

include_directories(${MVE_INCLUDE_DIRS})
link_directories(${MVE_LIBS_DIR})

target_link_libraries(${BIN} mvstex ${TBB_LIBRARIES} -lmve -lmve_util)
install(TARGETS ${BIN} RUNTIME DESTINATION bin)
