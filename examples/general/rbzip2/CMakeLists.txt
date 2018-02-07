find_package( LIBRT )
find_package( BZip2 REQUIRED)
if (TARGET cmdargs) 
set( APP rbzip2 )

add_executable( ${APP} "${APP}.cpp" )

target_link_libraries( ${APP} 
                       raft
                       cmdargs
                       ${BZIP2_LIBRARIES}
                       ${CMAKE_SCOTCH_LIBS}
                       ${CMAKE_THREAD_LIBS_INIT} 
                       ${CMAKE_RT_LIBS} )

file( COPY alice.txt
      DESTINATION ${CMAKE_CURRENT_BINARY_DIR} )

endif()
