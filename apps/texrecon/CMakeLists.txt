file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

set(BIN texrecon)
add_executable(${BIN} ${SOURCES})
set_property(TARGET ${BIN} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)

add_dependencies(${BIN} mvstex)
include_directories(${MVE_INCLUDE_DIRS})

target_link_libraries(${BIN} mvstex ${MVE_LIBRARY})
install(TARGETS ${BIN} RUNTIME DESTINATION bin)
