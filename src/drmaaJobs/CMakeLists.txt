file( GLOB headers RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_CURRENT_SOURCE_DIR}/*.h" )
file( GLOB sources RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_CURRENT_SOURCE_DIR}/*.cc" )


set( headers ${headers} ${CMAKE_CURRENT_SOURCE_DIR}/drmaa/drmaa.h )
#BRAINVISA_COPY_AND_INSTALL_HEADERS( headers soma/workflow headersTarget )

#if( NOT LIBDRMAA_FOUND )
#  add_library( drmaa SHARED drmaa/drmaa_empty.c )
#  set_property( TARGET drmaa PROPERTY LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}" )
#  add_dependencies( drmaa ${headersTarget} )
#endif()

add_library( somadrmaajobs SHARED ${sources} )
#add_dependencies( somadrmaajobs ${headersTarget} )
add_dependencies( somadrmaajobs ${headers})

set_property( TARGET somadrmaajobs PROPERTY VERSION ${${PROJECT_NAME}_VERSION} )
set_property( TARGET somadrmaajobs PROPERTY SKIP_BUILD_RPATH TRUE )

#target_link_libraries( somadrmaajobs "${CMAKE_CURRENT_SOURCE_DIR}/drmaa/libcondordrmaa.so" ) #condor
#target_link_libraries( somadrmaajobs "${CMAKE_CURRENT_SOURCE_DIR}/drmaa/libdrmaa.so" ) #SGE
target_link_libraries( somadrmaajobs ${LIBDRMAA_LIBRARIES} ) # built-in

# message ("LIBDRMAA_LIBRARIES=${LIBDRMAA_LIBRARIES}")

set_target_properties( somadrmaajobs PROPERTIES
                LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
                )

install (TARGETS somadrmaajobs 
	 DESTINATION lib)

#BRAINVISA_INSTALL( TARGETS somadrmaajobs
#                   DESTINATION lib
#                   COMPONENT ${PROJECT_NAME} )
