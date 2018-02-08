find_package( BZip2 )
if (BZip2_FOUND)
    if (TARGET cmdargs) 
        set( APP rbzip2 )
        add_executable( rbzip2 rbzip2.cpp )
        target_link_libraries( rbzip2
                               raft
                               cmdargs
                               ${BZIP2_LIBRARIES}
        )
        file( COPY alice.txt
              DESTINATION ${CMAKE_CURRENT_BINARY_DIR} )
    endif()
endif()
