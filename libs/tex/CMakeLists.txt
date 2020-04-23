# MVS TEXTURING FILES
set(LIBRARY mvstex)

file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

# if(${MVS_TEXTURING_BUILD_STATIC})
    # set(MVS_TEX_LINKING_MODE STATIC)
# else()
    # set(MVS_TEX_LINKING_MODE SHARED)
# endif(${MVS_TEXTURING_BUILD_STATIC})

add_library(${LIBRARY} STATIC ${SOURCES})
# add_library(${LIBRARY} ${MVS_TEX_LINKING_MODE} ${SOURCES})

#set_property(TARGET ${LIBRARY} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)
# add_dependencies(${LIBRARY} ext_mve ext_rayint ext_eigen ext_mapmap)
# add_dependencies(${LIBRARY} ext_mve ext_rayint)
# add_dependencies(${LIBRARY} mve)
add_dependencies(${LIBRARY} mve)
target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} -lmve -lmve_util ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${TIFF_LIBRARIES})
#target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} -lmve -lmve_util -lpng -ltiff -ljpeg)
#target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} -lmve -lmve_util)
#target_link_libraries(${LIBRARY} ${TBB_LIBRARIES} mve mve_util)
install(TARGETS ${LIBRARY} ARCHIVE DESTINATION lib)

