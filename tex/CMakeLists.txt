set(LIBRARY mvstex)

file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

add_library(${LIBRARY} STATIC ${SOURCES})
set_property(TARGET ${LIBRARY} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)

include_directories(SYSTEM ${MVE_INCLUDE_DIRS})
link_directories(${MVE_LIBS_DIR})

add_dependencies(${LIBRARY} mve)
target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} -lmve -lmve_util ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${TIFF_LIBRARIES})
install(TARGETS ${LIBRARY} ARCHIVE DESTINATION lib)