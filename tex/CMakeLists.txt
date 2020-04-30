set(LIBRARY mvstex)

file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

add_library(${LIBRARY} STATIC ${SOURCES})
set_property(TARGET ${LIBRARY} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)

include_directories(SYSTEM ${MVE_INCLUDE_DIRS})

add_dependencies(${LIBRARY} mve)

target_link_libraries(${LIBRARY} ${MVE_LIBRARY})

install(TARGETS ${LIBRARY} ARCHIVE DESTINATION lib)
